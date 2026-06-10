# Database Backups and Restores: How They Work and How to Use Them

This is a hands-on guide to the automatic database backup that runs on the
production server (vps2), the box that serves https://localharvest.exchange. It
explains what a backup is, how the daily backup is wired up, how to set it up
from scratch, and, most important, how to get your data back when something goes
wrong.

Read it alongside `technical-design.md`, which describes the production stack
this backup protects. You do not need to have read that document first; this one
stands on its own.

The audience is a developer on the team who knows another SQL database (MySQL or
MariaDB) but may be new to PostgreSQL, Docker, and Linux service tooling. Where a
tool is new, this guide points back to the equivalent you already know.

## 1. What this guide covers

- The idea: what a database backup is, in plain terms.
- The three pieces that make the daily backup run on the server.
- A line-by-line walk through the backup script.
- A tutorial to set the whole thing up on a fresh server.
- How to confirm it is running.
- How to restore: the full rollback, and the surgical "recover a few rows"
  approach.
- How to verify a restore worked.
- Troubleshooting the common failures.
- A glossary and a one-page command cheat sheet.

## 2. The mental model

A backup of this database is a single text file that lists every command needed
to rebuild the database from empty: the `CREATE TABLE` statements, then the row
data, then the constraints and sequences. If you have used `mysqldump`, this is
the same idea. PostgreSQL's tool is called `pg_dump`, and the file it produces is
plain SQL you could open in any text editor.

That text file is then compressed with `gzip` to save space (the same gzip you
know from `.gz` files), so each saved backup is named something like
`produce_exchange-20260609-033000.sql.gz`.

Restoring is the reverse: you uncompress the file and feed the SQL back into the
database, which replays every `CREATE` and `INSERT` and ends up exactly where the
snapshot was.

Three facts about the production setup shape every command below:

- The database runs **inside a Docker container**, not directly on the server.
  The container's Docker Compose service is named `db`. To run a database tool,
  you run it inside that container with `docker compose exec db ...`.
- The database name is `produce_exchange` and the database user is `produce`.
  These come from the `app/.env` file and are created automatically the first
  time PostgreSQL starts with an empty data directory.
- Inside the container, the local connection uses **trust authentication**,
  which means a tool connecting over the container's own socket needs no
  password. That is why none of the commands below ask for one.

## 3. What gets installed

The backup is three small files plus one directory. Nothing is added to the
application code, and the database itself is not modified.

| Piece | Path on the server | Job |
| --- | --- | --- |
| Backups folder | `/opt/produce-exchange/backups/` | Holds the saved `.sql.gz` files |
| Backup script | `/opt/produce-exchange/backup-db.sh` | Does one backup: dump, compress, check, rotate |
| Service unit | `/etc/systemd/system/produce-db-backup.service` | Tells the server how to run the script |
| Timer unit | `/etc/systemd/system/produce-db-backup.timer` | Tells the server when to run it (daily) |

The folder sits next to the application folder (`app/`), not inside it, so the
deploy process, which mirrors files into `app/` and deletes anything extra there,
can never remove your backups.

## 4. How the schedule works: systemd timers

On this server the daily run is handled by **systemd**, the program that starts
and supervises background services on most modern Linux systems. A systemd
**timer** is its version of a cron job or the Windows Task Scheduler: it fires on
a schedule and starts something.

systemd splits the "when" and the "what" into two files that share a name.

The timer file (`produce-db-backup.timer`) holds the schedule:

```ini
[Unit]
Description=Run the Local Produce Exchange database backup daily

[Timer]
OnCalendar=*-*-* 03:30:00 UTC
Persistent=true

[Install]
WantedBy=timers.target
```

- `OnCalendar=*-*-* 03:30:00 UTC` means every day at 03:30 UTC. That hour is
  chosen because traffic is low then.
- `Persistent=true` means if the server was switched off at 03:30 and missed a
  run, it does that run once at the next boot, so a powered-down night does not
  silently skip a day.
- `WantedBy=timers.target` is what makes the timer start automatically on boot
  once you enable it.

The service file (`produce-db-backup.service`) holds what to run:

```ini
[Unit]
Description=Daily PostgreSQL backup for Local Produce Exchange
Wants=docker.service
After=docker.service

[Service]
Type=oneshot
User=deploy
Group=deploy
SupplementaryGroups=docker
ExecStart=/opt/produce-exchange/backup-db.sh
```

- `Type=oneshot` means this is a task that runs to completion and exits, not a
  server that stays up.
- `User=deploy` runs the script as the `deploy` user. That user owns the app
  folder and the backups folder.
- `SupplementaryGroups=docker` gives that run access to Docker, so the script can
  talk to the database container without `sudo`.
- `Wants=docker.service` and `After=docker.service` make sure the Docker engine
  is started before this script tries to use it.

A timer "starts" its matching service. You enable the timer, not the service, and
the timer pulls in the service on schedule.

## 5. What the backup script does, line by line

Here is the full script, followed by an explanation of each part.

```bash
#!/usr/bin/env bash
# Daily backup of the Local Produce Exchange database.
# Runs as the deploy user via the produce-db-backup systemd timer.
set -euo pipefail

# Only the deploy user should be able to read the dumps (they hold all the data).
umask 077

cd /opt/produce-exchange/app

backups=/opt/produce-exchange/backups

# These match POSTGRES_USER and POSTGRES_DB in app/.env.
db_user=produce
db_name=produce_exchange

# Build a timestamped name. Write to a .tmp file first so a crashed dump
# never leaves a half-written file that looks like a good backup.
stamp=$(date -u +%Y%m%d-%H%M%S)
final="$backups/produce_exchange-$stamp.sql.gz"
tmp="$final.tmp"

# Remove the temp file if anything below fails.
trap 'rm -f "$tmp"' EXIT

# Dump the database and compress it.
docker compose exec -T db pg_dump -U "$db_user" "$db_name" | gzip > "$tmp"

# Reject an empty or corrupt dump before it becomes the real backup.
test -s "$tmp"
gzip -t "$tmp"

# The dump is good. Give it its final name and stop the cleanup trap.
mv "$tmp" "$final"
trap - EXIT

# Keep backups for 30 days; delete older ones.
find "$backups" -type f -name 'produce_exchange-*.sql.gz' -mtime +30 -delete

# Print a one-line summary so journalctl shows what happened.
count=$(ls "$backups"/produce_exchange-*.sql.gz | wc -l)
space=$(df -h "$backups" | tail -n 1)
echo "wrote $final; $count backups on disk; $space"
```

What each part does:

- **`set -euo pipefail`** turns on fail-fast error handling. `-e` stops the
  script on the first command that fails. `-u` treats use of an undefined
  variable as an error. `pipefail` makes a pipeline fail if any stage fails, not
  only the last one. Without `pipefail`, `pg_dump | gzip` would report success
  whenever `gzip` succeeded, even if `pg_dump` had crashed and produced nothing.
  This single line is the difference between a backup system you can trust and
  one that quietly saves empty files.

- **`umask 077`** sets the default permissions for files the script creates so
  that only the owner (`deploy`) can read them. A database dump contains every
  row of real data, so it must not be world-readable.

- **`cd /opt/produce-exchange/app`** moves into the app folder because that is
  where the `docker-compose.yml` lives. `docker compose` commands read that file
  to know which containers exist.

- **The timestamped name and the `.tmp` trick.** The script first writes to a
  temporary name ending in `.tmp`, and only renames it to the real
  `.sql.gz` name after the dump passes its checks. A rename on the same disk is
  atomic, so the real backup name only ever appears once a complete, verified
  file exists. A crash mid-dump leaves a `.tmp` file, never a corrupt file
  wearing a good name.

- **`trap 'rm -f "$tmp"' EXIT`** is a safety net, the same idea as a `finally`
  block in Java or C#. No matter how the script exits after this line, the
  temporary file gets deleted. The trap is cancelled later with `trap - EXIT`
  once the real file is safely in place.

- **The dump itself:**
  `docker compose exec -T db pg_dump -U produce produce_exchange | gzip > "$tmp"`.
  `docker compose exec db` runs a command inside the running database container.
  The `-T` turns off terminal handling, which is required when you pipe data in
  or out. `pg_dump` writes SQL to its output, the pipe (`|`) hands that to
  `gzip`, and the compressed result is saved to the temp file.

- **The two checks.** `test -s "$tmp"` fails if the file is zero bytes (an empty
  dump). `gzip -t "$tmp"` tests the gzip file for corruption. If either fails,
  `set -e` stops the script and the trap deletes the temp file, so a bad dump
  never becomes a saved backup.

- **`mv "$tmp" "$final"`** promotes the verified temp file to its real name, and
  `trap - EXIT` then turns off the cleanup so the good file survives.

- **Rotation:**
  `find "$backups" ... -mtime +30 -delete` deletes any backup file older than 30
  days. This is by file age, not a fixed count, so a busy day with extra manual
  runs keeps all of them, and a quiet stretch still drops anything older than a
  month.

- **The summary line.** The final `echo` prints what happened: the file written,
  how many backups now exist, and free disk space. Because the script runs under
  systemd, this line lands in the system journal, where you can read it later
  (covered below).

A note on scope: this is a `pg_dump` of one database, not a `pg_dumpall` of the
whole server. That is enough here because a clean restore recreates the empty
database first, and PostgreSQL builds the `produce` user and `produce_exchange`
database automatically from `app/.env`. If the project later adds more databases
or server-wide roles, the backup would need to be extended to capture those too.

## 6. Tutorial: setting it up on a fresh server

Do this once. You run these from your own machine; `ssh vps2` connects to the
server. Steps 1, 3, and 4 need administrator rights, so they use `sudo`.

### Step 1: Create the backups folder

```
ssh vps2 "sudo install -d -m 750 -o deploy -g deploy /opt/produce-exchange/backups"
```

This makes the folder, sets its mode to 750 (owner full access, group read, no
access for anyone else), and gives it to the `deploy` user.

### Step 2: Install the backup script

Put the script from section 5 on the server at
`/opt/produce-exchange/backup-db.sh`. The simplest way from a Windows machine is
to keep a copy in your project at `deploy\vps-files\backup-db.sh`, copy it up,
then move it into place:

```
scp deploy\vps-files\backup-db.sh vps2:/tmp/backup-db.sh
ssh vps2 "sudo install -m 750 -o deploy -g deploy /tmp/backup-db.sh /opt/produce-exchange/backup-db.sh && rm /tmp/backup-db.sh"
```

The `install` command copies the file and sets its owner and mode in one step:
owned by `deploy`, mode 750 (only `deploy` can run or read it).

### Step 3: Install the two systemd unit files

Copy the service and timer files from section 4 into `/etc/systemd/system/`.
Those are system files owned by root, so they go in with `sudo`:

```
scp deploy\vps-files\produce-db-backup.service vps2:/tmp/
scp deploy\vps-files\produce-db-backup.timer vps2:/tmp/
ssh vps2 "sudo install -m 644 -o root -g root /tmp/produce-db-backup.service /etc/systemd/system/ && sudo install -m 644 -o root -g root /tmp/produce-db-backup.timer /etc/systemd/system/ && rm /tmp/produce-db-backup.*"
```

### Step 4: Turn on the timer

```
ssh vps2 "sudo systemctl daemon-reload && sudo systemctl enable --now produce-db-backup.timer"
```

`daemon-reload` tells systemd to read the new unit files. `enable --now` does two
things at once: `enable` makes the timer start on every boot, and `--now` starts
it immediately so you do not have to reboot.

### Step 5: Run one backup by hand to prove it works

You do not have to wait until 03:30 to test. Run the script directly:

```
ssh vps2 "sudo -u deploy /opt/produce-exchange/backup-db.sh"
```

You should see the summary line, something like:

```
wrote /opt/produce-exchange/backups/produce_exchange-20260609-141502.sql.gz; 1 backups on disk; /dev/vda1 30G 9.1G 21G 31% /
```

If you see that and the command exits without error, the backup works.

## 7. Checking that it keeps running

Three commands tell you the health of the backup at any time.

See when the next run is scheduled and when the last one fired:

```
ssh vps2 "systemctl list-timers produce-db-backup.timer --all"
```

The `NEXT` column should show an upcoming 03:30 UTC time. If your server's clock
is not UTC and the time looks off, add `TZ=UTC` in front:
`ssh vps2 "TZ=UTC systemctl list-timers produce-db-backup.timer --all"`.

Confirm the timer is set to start on boot:

```
ssh vps2 "systemctl is-enabled produce-db-backup.timer"
```

This should print `enabled`.

Read the log of past runs (the systemd "journal" is its built-in log):

```
ssh vps2 "sudo journalctl -u produce-db-backup.service -n 20 --no-pager"
```

You will see one summary line per run. A failed run shows up here with its error,
and `systemctl list-timers` will show the failure too.

List the saved files directly. The folder is owner-only, so read it as `deploy`:

```
ssh vps2 "sudo -u deploy ls -la /opt/produce-exchange/backups"
```

Confirm the 30-day cleanup is trimming old dumps. Two ways:

- Over time, the file count settles near 30 and stays there. Each new daily
  dump is matched by one that ages past 30 days and gets deleted, so the
  "N backups on disk" number in the summary line plateaus. If it climbs past
  about 31 and keeps going, the cleanup is not running.

- Without waiting a month, prove it on demand. On the server as `deploy`, plant
  a file with an old timestamp, run the backup, and watch it disappear:

```
ssh vps2
sudo -u deploy -s
cd /opt/produce-exchange/backups
printf x | gzip > produce_exchange-rotation-test.sql.gz
touch -d '40 days ago' produce_exchange-rotation-test.sql.gz
/opt/produce-exchange/backup-db.sh    # dumps, then runs the 30-day cleanup
ls                                    # the 40-day test file is gone
exit
```

  The cleanup matches on file age, not the date in the name, so the old
  timestamp is what makes the test file eligible for deletion. Remove any test
  file the cleanup did not catch.

## 8. Tutorial: restoring your data

This is the part that matters. A backup you cannot restore is not a backup.

A reminder of why the steps look the way they do: our dump is a plain `pg_dump`
without `DROP` statements (the same as a `mysqldump` taken without
`--add-drop-table`). If you load it on top of a database that already has the
tables, you get "relation already exists" and duplicate-key errors. So the
reliable path loads into an empty database.

First, get onto the server as the `deploy` user. Because `deploy` owns the
backups folder and can use Docker, nothing after this needs `sudo`:

```
ssh vps2
sudo -u deploy -s
cd /opt/produce-exchange/app
ls -la /opt/produce-exchange/backups    # pick a file, copy its exact name
```

### Option A: Full restore (roll the whole database back to a snapshot)

Use this after a disaster or a bad migration, when you want the database to be
exactly what the snapshot held. Be clear about the cost: any change made after
that snapshot was taken is lost.

```
# 1. Safety net: dump the current state first, so this restore is itself undoable.
/opt/produce-exchange/backup-db.sh

# 2. Make the pipeline fail loudly if the uncompress step fails.
set -o pipefail

# 3. Stop the stack and delete the database volume (this wipes the live data).
docker compose down --volumes

# 4. Start an empty database. With no data present, PostgreSQL recreates the
#    "produce" user and "produce_exchange" database from app/.env.
docker compose up -d --wait db

# 5. Load the chosen dump. ON_ERROR_STOP=1 makes psql stop at the first SQL
#    error instead of plowing ahead and leaving a half-loaded database.
gunzip -c /opt/produce-exchange/backups/produce_exchange-YYYYmmdd-HHMMSS.sql.gz \
  | docker compose exec -T db psql -v ON_ERROR_STOP=1 -U produce -d produce_exchange

# 6. Bring the rest of the app back up.
docker compose up -d
```

Two cautions:

- **`set -o pipefail` (step 2) is not optional.** Without it, if `gunzip` fails,
  `psql` still runs against an empty input and the pipeline reports success. With
  it, a failed uncompress fails the whole command and you find out.
- **`down --volumes` (step 3) removes every volume in the Compose project,** not
  only the database. If the stack later gains other stored data (uploaded files,
  for example), check `docker-compose.yml` before running this so you do not wipe
  something you meant to keep.

After step 5 the dump replays every row, including the `alembic_version` row that
records which database migration the schema is on, so the backend starts cleanly
against the restored data.

### Option B: Surgical restore (recover a few rows without losing today's data)

Full rollback is the wrong tool when, say, someone deleted ten rows this morning
and you do not want to throw away every other change made since the last snapshot.
Instead, load the snapshot into a throwaway database, copy out only what you need,
then delete the throwaway. Production is never wiped.

```
# 1. Create an empty scratch database next to the real one.
docker compose exec -T db createdb -U produce produce_scratch

# 2. Load the snapshot into the scratch database.
set -o pipefail
gunzip -c /opt/produce-exchange/backups/produce_exchange-YYYYmmdd-HHMMSS.sql.gz \
  | docker compose exec -T db psql -v ON_ERROR_STOP=1 -U produce -d produce_scratch

# 3. Pull just the table you need out of the scratch database as its own small
#    dump (data only), then load that one table into the live database.
docker compose exec -T db pg_dump -U produce -d produce_scratch --data-only -t public.sample_data \
  | docker compose exec -T db psql -v ON_ERROR_STOP=1 -U produce -d produce_exchange

# 4. Delete the scratch database when you are done.
docker compose exec -T db dropdb -U produce produce_scratch
```

Replace `public.sample_data` with the table you actually need. PostgreSQL does not let
one query read across two databases as easily as MySQL's `otherdb.table` syntax,
so dumping one table from the scratch database and loading it back is the simple,
reliable move. If the rows you need would collide with rows already in the live
table, tell the team before loading, because you may need to target specific rows
rather than the whole table.

## 9. Verifying a restore worked

After either option, run a couple of checks before you call it done.

List the tables (the `\dt` command is psql's "show tables"):

```
docker compose exec db psql -U produce -d produce_exchange -c "\dt"
```

Count rows in a table you expect to have data:

```
docker compose exec db psql -U produce -d produce_exchange -c "SELECT count(*) FROM sample_data;"
```

Then open https://localharvest.exchange and confirm the data you expected is back
and the site behaves normally.

## 10. Troubleshooting

| Symptom | Likely cause | What to do |
| --- | --- | --- |
| Manual run prints nothing and exits non-zero | Database container not running | `docker compose up -d --wait db`, then run the script again |
| `permission denied` reading the backups folder | You are not `deploy` and did not use sudo | Prefix with `sudo -u deploy`, or run from a `sudo -u deploy -s` shell |
| `gzip -t` fails on a backup file | The file is corrupt or was copied half-way | Use an earlier backup; check free disk space with `df -h` |
| Timer shows no `NEXT` time | Timer not enabled | `sudo systemctl enable --now produce-db-backup.timer` |
| Restore stops partway with a SQL error | Loading into a non-empty database, or a real data error | For a full restore, make sure you wiped the volume first (Option A, step 3); read the exact psql error |
| `docker compose` says "no such service: db" | You are not in the app folder | `cd /opt/produce-exchange/app` first |

To see why a scheduled run failed, read its log:

```
ssh vps2 "sudo journalctl -u produce-db-backup.service -n 50 --no-pager"
```

## 11. What this protects against, and what it does not

This backup protects against **logical loss**: a bad migration, an accidental
`DELETE`, a wrong `UPDATE`, a table dropped by mistake. In all of those the
server is fine and you restore from a recent snapshot. The most you lose is the
changes made since the last daily run, up to about a day.

It does **not** protect against losing the whole server or its disk, because
every copy lives on that one box. If the server itself is destroyed, the backups
go with it. Guarding against that needs an off-server copy, which was left out on
purpose to keep this simple. If the project's data becomes valuable enough to
need that, the next step is a scheduled job that pulls the newest backup off the
server and stores an encrypted copy elsewhere. That is a known, planned
extension, not a gap that was overlooked.

## 12. Glossary

- **systemd**: the program that starts and supervises background services on the
  server. Owns timers and services.
- **service unit**: a systemd file describing how to run something
  (`produce-db-backup.service`).
- **timer unit**: a systemd file describing when to run a service
  (`produce-db-backup.timer`). Like a cron entry.
- **oneshot**: a service that runs once and exits, rather than staying up.
- **journal / journalctl**: systemd's built-in log and the command to read it.
- **Docker container**: an isolated package that runs the database. You reach
  tools inside it with `docker compose exec`.
- **Compose service**: a named container in `docker-compose.yml`. The database's
  is `db`.
- **volume**: the disk storage Docker keeps for a container's data. Deleting the
  database volume erases the data; the dump file is how you put it back.
- **pg_dump**: PostgreSQL's tool that writes a database out as SQL. Like
  `mysqldump`.
- **psql**: PostgreSQL's command-line client. Like the `mysql` client.
- **gzip / gunzip**: compress and uncompress `.gz` files.
- **pipe (`|`)**: sends one command's output straight into the next command.
- **umask**: the setting that decides default permissions for new files.
- **trap**: a bash instruction that runs cleanup when the script exits, like a
  `finally` block.
- **ON_ERROR_STOP**: a psql setting that makes it abort on the first SQL error
  instead of continuing.
- **trust authentication**: a PostgreSQL mode where a local connection needs no
  password. Used inside the container only.
- **alembic_version**: a small table that records which database migration the
  schema is on. It is included in every dump.

## 13. Command cheat sheet

Setup (once):

```
ssh vps2 "sudo install -d -m 750 -o deploy -g deploy /opt/produce-exchange/backups"
# copy backup-db.sh and the two unit files into place (section 6)
ssh vps2 "sudo systemctl daemon-reload && sudo systemctl enable --now produce-db-backup.timer"
```

Run a backup now:

```
ssh vps2 "sudo -u deploy /opt/produce-exchange/backup-db.sh"
```

Check health:

```
ssh vps2 "systemctl list-timers produce-db-backup.timer --all"
ssh vps2 "systemctl is-enabled produce-db-backup.timer"
ssh vps2 "sudo journalctl -u produce-db-backup.service -n 20 --no-pager"
ssh vps2 "sudo -u deploy ls -la /opt/produce-exchange/backups"
```

Full restore (on the server, as deploy, in the app folder):

```
/opt/produce-exchange/backup-db.sh        # safety dump first
set -o pipefail
docker compose down --volumes
docker compose up -d --wait db
gunzip -c /opt/produce-exchange/backups/produce_exchange-YYYYmmdd-HHMMSS.sql.gz \
  | docker compose exec -T db psql -v ON_ERROR_STOP=1 -U produce -d produce_exchange
docker compose up -d
```
