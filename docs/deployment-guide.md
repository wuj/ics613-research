# Team 4 - Deployment Guide

This document explains how the production deploy works and how to operate
the live server. The site runs at https://localharvest.exchange.

Every push to the `main` branch deploys automatically through GitHub
Actions. You can also start a deploy by hand from the Actions tab (see
"Manual redeploy"). In normal operation, you do not run a deploy command
from a laptop.

## How GitHub Actions deploys

GitHub Actions is the automation service that runs commands after a GitHub
event, such as a pull request or a push to `main`. Each run uses a
temporary Ubuntu runner. The runner checks out the repo, installs the tools
the workflow asks for, runs the commands, and then is thrown away.

The repo has two active workflow files under `.github/workflows/`:

- `unit-tests.yml` is named "Checks". It lints the code, builds the
  frontend, and runs the tests.
- `deploy.yml` is named "Deploy". It deploys to the VPS after Checks
  passes.

### Checks workflow

The Checks workflow runs in three cases:

- on every pull request,
- when another workflow calls it with `workflow_call`, and
- when someone starts it by hand from the Actions tab.

The job runs on `ubuntu-latest` and has a 15-minute timeout. It checks out
the repo, sets up Node.js 22, sets up `uv`, installs dependencies with
`npm run setup`, runs `npm run lint`, runs `npm run build`, and runs
`npm test`.

Checks has its own concurrency rule. Concurrency means GitHub does not
keep wasting time on an older run after a newer run starts for the same
branch or pull request. For Checks, `cancel-in-progress: true` cancels the
older run.

### Deploy workflow

The Deploy workflow starts in two cases:

- every push to `main`, including a merged pull request, and
- a manual run from the Actions tab.

The workflow has one concurrency group named `deploy-production`. This
means GitHub never runs two production deploys at the same time. The
setting `cancel-in-progress: false` means a newer deploy waits for the
current deploy to finish instead of stopping it halfway through.

The Deploy workflow has two jobs:

1. `checks`: calls the Checks workflow. If lint, build, or tests fail, the
   workflow stops here and nothing is copied to the server.
2. `deploy`: waits for `checks` with `needs: checks`. It also has
   `if: github.ref == 'refs/heads/main'`, so a manual run from another
   branch can run Checks but cannot deploy. This job has a 15-minute
   timeout.

The deploy job runs these steps in order:

1. Check out the repo on the runner.
2. Set up Node.js 22 and enable npm caching for the frontend lock file.
3. Install `rsync` on the runner.
4. Build the frontend with `npm --prefix frontend ci` and
   `npm --prefix frontend run build`. The server does not install Node or
   build the frontend.
5. Write the SSH key and known-hosts file from GitHub secrets. The
   known-hosts file pins the VPS SSH host key, so the runner connects to
   the expected server instead of trusting a fresh scan during deploy.
6. Copy `backend/`, `docker-compose.yml`, and `scripts/` to
   `/opt/produce-exchange/app/` with `rsync`. This copy uses `--delete`,
   so files removed from those source paths are removed from the matching
   paths on the server. It excludes `.env`, `docker-compose.override.yml`,
   `backend/.venv`, Python cache folders, pytest cache, and ruff cache.
7. Run `bash /opt/produce-exchange/app/scripts/deploy-remote.sh` on the
   server over SSH.
8. Copy the freshly built `frontend/dist` to the server with `rsync`, but
   only if the remote script finished successfully.

`scripts/deploy-remote.sh` is the server-side gate. It checks that the
production `.env` file exists and has every required PostgreSQL key,
installs backend dependencies with `uv`, starts or updates the database
container and waits for it to become healthy, runs
`alembic upgrade head`, runs the seed script, restarts the backend through
systemd, waits for `/api/health`, and sends one request to
`/api/sample-endpoint` so the deploy proves it can read from the database.

The frontend is copied last on purpose. nginx serves `frontend/dist`
straight from disk, so the new pages go live the moment the files land.
Copying them after the backend is migrated and healthy means a failure
before that point keeps the old frontend in place. If the remote script
fails before the backend restart, the old backend keeps running too. If it
fails after the restart, the API may be down or serving the new backend
until someone deploys a fix or reverts.

## Deployment gates

A gate is a condition that must pass before the next deploy step can run.
The current gates are:

- Pull requests run Checks, so reviewers can see lint, build, and test
  status before merging.
- A push to `main` starts Deploy, but Deploy calls Checks again before it
  copies files to the VPS.
- The deploy job runs only on `main`, even for a manual run.
- The `deploy-production` concurrency group prevents overlapping deploys.
- The SSH setup step must have all four deployment secrets, and the
  known-hosts value must match the server key.
- The remote script refuses to deploy if the production `.env` file is
  missing or incomplete.
- The database container must be healthy before migrations run.
- Migrations must finish before the backend restarts.
- The backend must answer `/api/health` after restart.
- The database-backed check request must succeed before the workflow
  copies the new frontend.

As checked on June 10, 2026, GitHub itself does not add a separate merge
or deploy gate around these workflows. The `main` branch is not protected,
there are no repository rulesets, and there are no GitHub deployment
environments with approval rules. This means a user with write access can
push or merge to `main` without GitHub blocking the merge for required
status checks. The production deploy still has to pass the Checks job and
the deploy gates above before the new frontend is copied.

## Architecture

- nginx handles HTTPS, serves the built React files from `frontend/dist`,
  and sends any unknown path back to `index.html` so React Router can do
  client-side routing.
- nginx forwards `/api/` to the backend at `127.0.0.1:8000`.
- The backend is uvicorn running FastAPI, started by systemd as the
  `produce-backend.service` unit, one worker, as the `deploy` user.
- PostgreSQL runs in a Docker container from the repo's
  `docker-compose.yml`, listening on `127.0.0.1` only.
- The browser talks to one origin (the domain), so there are no CORS
  settings to manage. The frontend already uses relative `/api/...` paths.

## The server (vps2)

- Host: Vultr, 45.77.209.138, Debian 13, 961MB RAM plus 3GB swap, 30GB
  disk.
- Admin login: `ssh vps2`, an SSH config alias on the admin machine that
  connects as the server's admin user with passwordless sudo.
- App directory: `/opt/produce-exchange/app`, owned by the `deploy` user.
- Domain: `localharvest.exchange` (covers the apex and `www`).

### What provisioning installed

- Packages: nginx, ufw, rsync, curl, gnupg, ca-certificates, and Docker
  Engine plus the compose plugin from Docker's official apt repository.
- `/etc/docker/daemon.json`: json-file logging capped at three files of
  10MB each per container, so container logs cannot fill the disk.
- User `deploy` (in the `docker` group): owns the app directory, runs the
  backend, and receives deploys. Its only login credential is the deploy
  SSH key.
- `/etc/sudoers.d/deploy`: lets `deploy` run exactly
  `systemctl restart produce-backend.service` and
  `systemctl status produce-backend.service` as root, nothing else.
- `uv` for the deploy user at `~/.local/bin/uv`, with Python 3.14.2 (the
  version `backend/.python-version` pins).
- UFW firewall: allows SSH, 80, and 443; everything else is closed.
  Postgres is also unreachable from outside because Docker publishes it on
  `127.0.0.1` only.
- `/opt/produce-exchange/app/.env` (mode 600, owner `deploy`): the only
  plaintext copy of the real database password. It is never committed and
  never copied by a deploy.
- `produce-backend.service` (enabled): uvicorn, one worker,
  `127.0.0.1:8000`, `LOG_LEVEL=INFO`, restarts on failure.
- TLS: private key at `/etc/ssl/private/produce.key` (never leaves the
  server), certificate chain at `/etc/ssl/produce/fullchain.pem` (Sectigo
  leaf plus intermediate bundle, valid until 2026-12-21, covers apex and
  `www`).
- nginx site `produce-exchange`: redirects port 80 to HTTPS, serves
  `frontend/dist` with the SPA fallback, and proxies `/api/`.

## GitHub repository secrets

The Deploy workflow reads four GitHub Actions secrets (Settings -> Secrets
and variables -> Actions). As checked on June 10, 2026, these four secret
names exist. GitHub hides the secret values after they are saved, so the
CLI can confirm the names and update times but cannot print the values.
Recent Deploy runs have succeeded, which confirms the saved values are
usable by the workflow.

- `DEPLOY_SSH_KEY`: the private half of the deploy key. The workflow
  writes it onto the runner and uses it to reach the `deploy` user.
- `DEPLOY_HOST`: the server address, `45.77.209.138`.
- `DEPLOY_USER`: the login name, `deploy`.
- `DEPLOY_KNOWN_HOSTS`: the server's SSH host key line, so the runner
  trusts the right machine instead of scanning for it at deploy time.

## Manual redeploy

Open the Actions tab, choose the Deploy workflow, and use the "Run
workflow" button. The deploy job has a guard
(`if: github.ref == 'refs/heads/main'`), so a manual run only deploys from
`main`. A manual run does the same steps as a push: rebuild, copy,
migrate, restart. Because the database steps skip work that is already
done (see "Database migrations" and "Seeding"), redeploying the same
commit is safe.

## Rollback

- Code: revert the bad merge on `main`. The revert is a normal push to
  `main`, so it triggers a deploy that puts the previous code back.
- Frontend: there is no separate frontend rollback; reverting the merge
  redeploys the matching frontend along with the backend.
- Migration caveat: reverting code does not undo a database migration. See
  the next section.

## Database migrations

Schema changes are managed by Alembic. The schema is described by
migration files in `backend/migrations/versions/`, and each deploy runs
`alembic upgrade head` to apply any that the database has not seen yet.

### Shipping a schema change

1. Edit or add the model files under `backend/app/models/`.
2. Make sure any new model module is imported in
   `backend/app/models/__init__.py`. Alembic only sees tables that an
   import has registered. If you forget an import, Alembic compares the
   database to incomplete metadata. For a new table, the generated
   migration may omit the `create_table`. For an existing table that fell
   out of the imports, Alembic may write a `drop_table`. A surprise
   missing `create_table` or surprise `drop_table` is the warning sign.
3. Generate the migration: `npm run db:revision -- "short message"`.
4. Read the generated file under `backend/migrations/versions/` and
   confirm it does what you expect.
5. Commit the migration file together with the code change.

The next deploy applies it automatically. Keep migrations additive (add
tables and columns; do not drop or rename in the same change as the code
that stops using them), so the brief moment when the old code runs against
the new schema stays safe.

### Check what the live database is at

    ssh vps2 "cd /opt/produce-exchange/app/backend && sudo -u deploy .venv/bin/alembic current"

This prints the current revision id. The `sudo -u deploy` part matters:
only the `deploy` user can read the `.env` file that holds the database
password.

### Why a revert does not undo a migration

A migration that has already run leaves the database at the newer
revision. Reverting the merge removes the migration file from the repo but
does not change the database. The next deploy then runs
`alembic upgrade head`, cannot find the revision the database reports, and
fails with an unknown-revision error. The fixes, in order of preference:
write a new migration that undoes the change, or restore the backup you
took before the deploy. A manual `alembic downgrade` is a last resort, not
the normal path.

### Rules

- Never edit a migration that has already run in production. Write a new
  revision instead.
- Take a manual backup (below) before merging any PR that contains a
  migration.

## Seeding

Every deploy runs the seed script. It inserts the three demo rows only
when the table is empty, so it does nothing on a database that already has
data. If someone empties the `sample_data` table in production on purpose,
the next deploy puts the three demo rows back, because the seed skips
inserting only when at least one row is already present.

## Database backup and restore (manual)

Run these from Git Bash on the admin machine. Take a backup before any
schema-changing deploy, before rotating the password, or before anything
that touches the data volume:

    ssh vps2 "cd /opt/produce-exchange/app && sudo docker compose exec -T db pg_dump -U produce produce_exchange" > backup-$(date +%Y%m%d-%H%M%S).sql

Restore into a running database container. To restore into an empty
database, recreate the volume first:

    ssh vps2 "cd /opt/produce-exchange/app && sudo docker compose down --volumes && sudo docker compose up -d --wait db"
    ssh vps2 "cd /opt/produce-exchange/app && sudo docker compose exec -T db psql -U produce -d produce_exchange" < backup-FILE.sql

The `sudo` is needed because `ssh vps2` logs in as the admin user, who is
not in the docker group. Keep the .sql files on the admin machine, not the
server. The dump includes Alembic's `alembic_version` table, so a restored
database remembers which migrations it already has, and the next deploy
applies only newer ones. If the restored dump includes rows in
`sample_data`, the next deploy's seed step skips itself. If that table is
empty, the seed step inserts the three demo rows.

## Automated database backups

A systemd timer takes a database backup every 6 hours, so recovery does
not depend on hand-run dumps alone. The pieces:

- Backup directory: `/opt/produce-exchange/backups/`, mode 750, owned by
  `deploy:deploy`. It is a sibling of `app/`, so the deploy rsync (which
  only targets paths under `/opt/produce-exchange/app/`) never touches it.
- Backup script: `/opt/produce-exchange/backup-db.sh`, run as the `deploy`
  user. It runs `pg_dump` on the one database, compresses the result with
  gzip, and writes a timestamped file named
  `produce_exchange-YYYYmmdd-HHMMSS.sql.gz`. The script writes to a `.tmp`
  file first and checks that the dump is non-empty and passes `gzip -t`
  before giving it the final name, so a crashed dump never leaves a
  half-written file that looks like a complete dump. It then deletes dumps
  older than 30 days.
- systemd units: `produce-db-backup.service` (a oneshot that runs the
  script as `deploy` with the `docker` group added) and
  `produce-db-backup.timer` (fires every 6 hours, at 00:00, 06:00, 12:00,
  and 18:00 UTC).
  The timer has `Persistent=true`, so a backup missed while the box was off
  runs at the next boot. No sudoers change was needed: the service runs as
  `deploy`, which is already in the `docker` group.

This protects against logical loss such as a bad migration or an accidental
delete, where you restore from a recent dump. It does not protect against
losing the whole VPS or its disk, because the backups live on the same
server; an off-box copy was left out to avoid adding another account,
credential, and scheduled transfer.

The dump files are readable only by the `deploy` user (the directory is
mode 750 and each file is mode 600), so admin commands that read them use
`sudo` or `sudo -u deploy`.

### List the backups on the server

    ssh vps2 "sudo -u deploy ls -la /opt/produce-exchange/backups"

### Check the timer

    ssh vps2 "systemctl list-timers produce-db-backup.timer --all"
    ssh vps2 "sudo journalctl -u produce-db-backup.service"

The first command shows the next and last run times. The second shows the
one-line summary each run prints (the file it wrote, how many backups are
on disk, and the free space).

### Restore one of these dumps

These backups are gzip-compressed, so the restore unzips on the way in.
Run the remote command under `bash -o pipefail` so a failed `gunzip` is not
hidden by a later command in the pipe, and pass `ON_ERROR_STOP=1` to `psql`
so the restore stops on the first SQL error. The `sudo` before `gunzip`
reads the mode-750 backup directory; the `sudo` before `docker` is needed
because the SSH login is the admin user, not `deploy`:

    ssh vps2 "bash -o pipefail -c 'cd /opt/produce-exchange/app && sudo gunzip -c /opt/produce-exchange/backups/produce_exchange-YYYYmmdd-HHMMSS.sql.gz | sudo docker compose exec -T db psql -v ON_ERROR_STOP=1 -U produce -d produce_exchange'"

To restore into an empty volume, recreate the volume first
(`down --volumes`, then `up -d --wait db`), the same as the manual section
above. The dump includes the `alembic_version` table, so a restored
database remembers its migration state and the next deploy applies only
newer migrations.

## Database password rotation

The postgres image reads `.env` only when it first creates an empty data
volume. After that the password lives inside the volume, so editing `.env`
by itself makes the backend and the database disagree and login fails.
Rotate in this order (as the admin user):

1. Change it in the database:

        ssh vps2 "cd /opt/produce-exchange/app && sudo docker compose exec db psql -U produce -d produce_exchange -c \"ALTER ROLE produce WITH PASSWORD 'newvalue'\""

2. Edit `/opt/produce-exchange/app/.env` so `POSTGRES_PASSWORD` matches.
3. Restart the backend:

        ssh vps2 "sudo systemctl restart produce-backend.service"

## Certificate rotation

The certificate expires 2026-12-21. With the new issued files saved in a
local `ssl` folder, run these from the folder that contains it:

    scp ssl\localharvest_exchange.crt vps2:/tmp/
    scp ssl\localharvest_exchange.ca-bundle vps2:/tmp/
    ssh vps2 "sudo install -m 644 /tmp/localharvest_exchange.crt /etc/ssl/produce/ && sudo install -m 644 /tmp/localharvest_exchange.ca-bundle /etc/ssl/produce/ && sudo sh -c 'cat /etc/ssl/produce/localharvest_exchange.crt /etc/ssl/produce/localharvest_exchange.ca-bundle > /etc/ssl/produce/fullchain.pem' && sudo chmod 644 /etc/ssl/produce/fullchain.pem && rm /tmp/localharvest_exchange.crt /tmp/localharvest_exchange.ca-bundle && sudo nginx -t && sudo systemctl reload nginx"

The private key only changes if a new CSR was generated. Reusing the
existing CSR keeps `/etc/ssl/private/produce.key` valid.

## Docker log rotation

`/etc/docker/daemon.json` caps container logs at three files of 10MB each.
Confirm the driver with:

    ssh vps2 "sudo docker info --format '{{.LoggingDriver}}'"

It should print `json-file`. If disk use ever climbs, look under
`/var/lib/docker/containers`.

## Self-healing

The server recovers from these failures on its own:

- Reboot: Docker restarts the database container (the
  `docker-compose.override.yml` file sets `restart: unless-stopped`),
  systemd restarts the backend (the unit is enabled), and nginx starts.
  No manual step.
- Backend process dies: systemd restarts uvicorn within a few seconds
  (`Restart=always`).
- Database container crashes: the Docker restart policy starts it again,
  and the backend reconnects on the next request (the connection pool
  checks each connection before using it).

One case stays manual on purpose: `docker stop` is treated as an operator
decision, so the restart policy leaves it stopped. Start it again with:

    ssh vps2 "cd /opt/produce-exchange/app && sudo docker compose up -d db"
