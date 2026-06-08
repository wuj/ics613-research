# Surplus: A Local Produce Exchange - Architecture

This document is the architecture reference for the Team 4 "Surplus: A Local
Produce Exchange" repository (`ICS613_Summer2026_FinalProject_Team4`). It covers
the application end to end: the npm-driven build and run tooling, the React and
TypeScript frontend, the FastAPI and Python backend, the PostgreSQL database with
its Alembic migrations, and the GitHub Actions pipeline. Deployment of the running
system to the VPS is outside the scope of this document.

It contains two diagrams, a static architecture view and the runtime request
path, and a description of each technology and how it contributes to the web
application.

The diagrams are embedded below as SVG images rendered from Mermaid source. The
SVG files and their editable Mermaid (`.mmd`) source live in `docs/diagrams/`.
See "Regenerating the diagrams" at the end.

## Architecture diagram (static view)

![Surplus end-to-end architecture: npm orchestration, React/Vite frontend, FastAPI/uvicorn backend, PostgreSQL, and GitHub Actions](diagrams/architecture.svg)

## Request lifecycle (runtime view)

This traces a request end to end, from a button click in the browser to a
database row and back.

![Request lifecycle: button click through Vite proxy, FastAPI, Pydantic validation, SQLAlchemy, and PostgreSQL, then back to the rendered response](diagrams/request-lifecycle.svg)

## Each technology and what it contributes

These are grouped by layer and bridged to stacks the reader may already know
(Java, C#, PHP, JS, MySQL/MariaDB).

### Orchestration layer

**npm and npm scripts.** The root `package.json` has no dependencies of its own.
It is pure task runner, the same role Maven/Gradle tasks, Composer scripts, or a
Makefile play. It is the single front door: every other tool is invoked through
an `npm run ...` target so a developer never has to remember the raw `uv` or
`docker compose` commands. The targets fall into groups: lifecycle (`setup`,
`fix:frontend`, `fix:backend`), run (`frontend`, `backend`, `db`), database
(`db:up`, `db:down`, `db:reset`, `db:migrate`, `db:revision`, `db:seed`), and
quality (`lint`, `typecheck`, `test`, `build`). One detail to note: the scripts
shell out across language boundaries. For example `npm run backend` runs
`uv run --locked --directory backend uvicorn app.main:app --reload --port 8000`,
so npm drives the Python server too.

**Astral uv.** This is the Python side of the toolchain and the counterpart to
npm. Think of it as the Python equivalent of npm plus a version manager: it pins
the interpreter (`.python-version` says 3.14.2), resolves dependencies, and
writes a `uv.lock` lockfile, the way `package-lock.json` or `composer.lock` pins
exact versions. The `--locked` flag everywhere means CI and every developer
install the identical dependency graph. Every Python process (uvicorn, Alembic,
pytest, the seeder) is launched through `uv run` so it gets the right virtual
environment without anyone activating one by hand.

**Docker / docker compose.** Only one thing runs in a container here:
PostgreSQL. `docker-compose.yml` defines a single `db` service. This is the
lightest possible use of containers, just enough to give every developer the same
database version without a local install. Compare it to running MySQL in a VM
versus installing it on the host; compose is the recipe that starts, stops, and
wipes that database with one command (`npm run db`, `db:down`, `db:reset`).

### Frontend layer

**React.** React 19 is the UI library. The shift from PHP/JSP server-rendered
pages is the big paradigm change: instead of the server building HTML per
request, the browser downloads a JavaScript bundle that builds and updates the
page itself. You describe what the screen should look like for a given state, and
React updates the real DOM to match. Entry is `main.tsx`, which mounts the `App`
component into the single `<div id="root">` in `index.html`. `App.tsx` is the
root component. State lives in components through hooks (`useState`, `useRef`);
for example, `HomePage` holds the request and response text in state and uses a
`useRef` counter so a slow earlier response cannot overwrite a newer one.

**TypeScript.** This is JavaScript with a static type layer added on, the same
relief you get from Java or C# after working in untyped PHP. Types are checked at
build time and then erased; the browser runs plain JavaScript. The project uses
project references (`tsconfig.json` points at `tsconfig.app.json` for browser
code and `tsconfig.node.json` for build tooling) with strict checks on. The build
step is `tsc -b && vite build`, so a type error fails the build before any bundle
is produced. That is also why `npm run typecheck` exists as its own fast target.

**Vite.** Vite is two tools in one: the dev server you run while coding and the
bundler that produces the production build. It is the modern replacement for
webpack, and the gain over older bundlers is speed, because it serves your source
over native ES modules during development with hot module replacement (edits show
up without a full reload). The piece that matters most for this app is the dev
proxy. `vite.config.ts` forwards every `/api` request from the dev server (port
5173) to the FastAPI backend (port 8000). That single rule is why the React code
can call `/api/sample-endpoint` with no host name and no CORS setup: the browser
thinks it is talking to one origin, and Vite relays the call to Python. One
naming note: the config binds the host to `127.0.0.1` rather than `localhost` on
purpose, to avoid an IPv6 resolution quirk on Windows.

**Vitest.** The frontend test runner, and it is deliberately Jest-compatible, so
the `describe`/`test`/`expect` shape will look familiar if you have used JUnit or
PHPUnit, just in TypeScript. It reuses the Vite config, so tests transform code
the same way the app does. Component tests (HomePage, AboutPage) run under jsdom,
a fake browser DOM, with React Testing Library to render components and query the
rendered output the way a user would. Service and utility tests run in plain Node
with no DOM. Network calls are never real: the tests replace the global `fetch`
with a stub (`vi.stubGlobal`), so they assert the exact URL, method, headers, and
timeout behavior without a running backend.

### Backend layer

**Python.** The backend language, pinned to 3.14. If the mental model is C# or
Java, the largest adjustments are dynamic typing softened by type hints (which
Pydantic and SQLAlchemy lean on heavily), indentation as block structure, and an
async model used by FastAPI for handling many requests on a small number of
threads.

**uvicorn.** This is the ASGI web server that holds the network socket and speaks
HTTP, the role Tomcat plays for a Java WAR, Kestrel for ASP.NET, or php-fpm
behind nginx. ASGI is the async successor to the older WSGI standard. FastAPI by
itself is just application code; uvicorn is what runs it. In development it is
launched with `--reload`, so saving a Python file restarts the server, the same
convenience as Spring DevTools.

**FastAPI.** The web framework, closest in spirit to Spring Boot controllers or
ASP.NET Web API. It provides routing, dependency injection, and request/response
validation. `app/main.py` creates the app and mounts two routers, both under the
`/api` prefix, which is the contract the Vite proxy relies on. Routes are plain
functions decorated with the HTTP method and path; parameters declared on the
function are filled in by the framework. Two features stand out versus older
frameworks: dependency injection through `Depends(...)` (the database session is
injected into the handler, not constructed inside it), and automatic
OpenAPI/Swagger documentation generated from the type hints, so the API is
self-describing with no extra annotations.

**Pydantic.** The data validation and serialization layer, version 2. The
closest analogy is Java Bean Validation plus Jackson, or C# DTOs with data
annotations, but here it is the same objects doing both jobs. You declare a class
with typed fields (`SampleRequest` has `foo: str` and `baz: int`), and FastAPI
uses it to parse and validate incoming JSON automatically. If a client sends the
wrong type or omits a field, FastAPI returns a 422 with a structured error before
your code runs, which is exactly what the frontend's "wrong type" and "malformed
JSON" buttons demonstrate. Response models (`SampleResponse`) work the same
direction, controlling the shape of the JSON sent back.

**SQLAlchemy.** The ORM, version 2.0, equivalent to Hibernate/JPA or Entity
Framework. It maps the `SampleData` Python class to the `sample_data` table so
handlers work with objects and queries instead of raw SQL strings. `app/db.py`
builds the connection URL from environment variables
(`postgresql+psycopg://produce:produce@127.0.0.1:5432/produce_exchange` by
default), creates the engine with `pool_pre_ping=True` so dead pooled connections
are detected before use, and exposes `SessionLocal` plus a `get_db_session()`
generator. That generator is the unit of work per request: FastAPI opens a
session, hands it to the handler, and closes it afterward in a `finally`, the
same open-use-close pattern as a JPA `EntityManager`. The driver underneath is
psycopg 3, the PostgreSQL client library (the role of the JDBC driver in Java).

**Alembic.** Database migrations, the counterpart to Flyway/Liquibase or EF
Migrations. It records schema changes as versioned Python files so every machine
can move its database to the same structure. The setup here is wired to the
models: `migrations/env.py` imports the SQLAlchemy `Base.metadata`, so
`npm run db:revision` can autogenerate a migration by diffing the models against
the live database. The initial migration, `cd9306b0de55` (down-revision `None`),
creates the `sample_data` table. `npm run db:migrate` applies migrations up to the
latest (`alembic upgrade head`). Alembic also tracks which migration a database is
on in its own `alembic_version` table, the same bookkeeping Flyway does with its
history table.

### Data layer

**PostgreSQL.** The database, version 18.4, running in the Docker container.
Coming from MySQL/MariaDB, the day-to-day SQL is familiar, but expect stricter
standard-SQL behavior, real sequences instead of `AUTO_INCREMENT`, a richer type
system, and `SERIAL`/identity columns for primary keys. It listens on
`127.0.0.1:5432` and stores its files in a named volume (`produce_db_data`) so
data survives container restarts. `npm run db:reset` is the one command that
deletes that volume. A health check using `pg_isready` lets `npm run db:up` wait
until the database accepts connections before returning, which keeps migrations
from racing a not-yet-ready server. The `sample_data` table has `id`, a unique
`slug`, `name`, and `note`, and `app/seed.py` fills it with three idempotent demo
rows (Manoa lettuce, apple bananas, Kahuku corn).

### Quality and CI layer

**pytest.** The backend test framework, comparable to JUnit or PHPUnit. Tests
call the code directly rather than over HTTP: they invoke the route function
`create_sample()` with a session, validate Pydantic models, and run the seeder.
The design choice worth noting is that these tests never need Docker or
PostgreSQL. They spin up an in-memory SQLite database (`sqlite:///:memory:`) and
build the schema from the same SQLAlchemy models, so the suite is fast and has no
external dependency. `test_db.py` checks the connection-URL builder with
environment overrides, `test_sample_endpoint.py` covers the happy path plus
validation and a forced database error (503), and `test_seed.py` checks that
seeding inserts three rows and does not duplicate on a second run. Configuration
lives in `pyproject.toml` (`testpaths = ["tests"]`).

**ruff and eslint.** The two linters, one per language. ruff is the Python linter
(a fast replacement for flake8/pylint, and it also formats the autogenerated
migration files via an Alembic post-write hook). eslint is the
JavaScript/TypeScript linter, using the newer flat-config style with the
typescript-eslint and React Hooks rule sets. `npm run lint` runs both so a single
command gates both halves of the codebase.

**GitHub Actions.** The CI service, equivalent to Jenkins or GitLab CI. There is
one workflow, `.github/workflows/unit-tests.yml`, named "Checks." It runs on
every pull request, on pushes to `main`, and on manual dispatch, with a
concurrency rule that cancels a superseded run on the same branch so there is no
spend on stale builds. The job runs on `ubuntu-latest`, sets up Node 22 and uv
8.2.0, then runs the same npm targets a developer would: `npm run setup`, then
`lint`, then `build`, then `test`. Because every step is an `npm run` target, CI
and local development execute the identical commands, so "passes on my machine"
and "passes in CI" mean the same steps. This is the gate in front of `main`.

## How it fits together

The through-line is that npm is the conductor and `/api` is the contract. A
developer runs three npm targets (`db`, `backend`, `frontend`) to bring up
Postgres, uvicorn, and Vite. The browser calls `/api/...` with no host, Vite's
proxy relays it to uvicorn, FastAPI validates with Pydantic, reads through
SQLAlchemy and psycopg into PostgreSQL, and the typed response flows back. Schema
changes go through Alembic; demo data through the seeder. Both test suites run
without the real database, and GitHub Actions runs the same npm commands on every
pull request before anything reaches `main`.

Here is the quick bridge from common stacks to what is used here:

| Common stack | This project uses | Role |
| --- | --- | --- |
| Maven/Gradle tasks, Make | npm scripts | Single entry point for every command |
| npm + a version manager | Astral uv | Python deps, lockfile, interpreter |
| JSP/PHP server pages | React 19 | UI built and updated in the browser |
| Java/C# static types | TypeScript | Build-time type checking |
| webpack | Vite | Dev server, HMR, bundler, `/api` proxy |
| JUnit/PHPUnit (JS) | Vitest + jsdom | Frontend unit and component tests |
| Tomcat/Kestrel/php-fpm | uvicorn | ASGI HTTP server hosting the app |
| Spring Boot / ASP.NET Web API | FastAPI | Routing, DI, OpenAPI docs |
| Bean Validation + Jackson | Pydantic v2 | Request/response validation and JSON |
| Hibernate/JPA, EF | SQLAlchemy 2.0 | ORM + session per request |
| JDBC driver | psycopg 3 | PostgreSQL client library |
| Flyway/Liquibase, EF Migrations | Alembic | Versioned schema migrations |
| JUnit/PHPUnit | pytest | Backend unit tests (in-memory SQLite) |
| MySQL/MariaDB | PostgreSQL 18.4 | Relational database (in Docker) |
| Jenkins/GitLab CI | GitHub Actions | Lint, build, test on every PR |

## Source references

- Root orchestration: `package.json`, `docker-compose.yml`, `.env.example`
- CI: `.github/workflows/unit-tests.yml`
- Frontend: `frontend/package.json`, `frontend/vite.config.ts`,
  `frontend/src/main.tsx`, `frontend/src/App.tsx`,
  `frontend/src/services/sampleEndpointService.ts`
- Backend: `backend/pyproject.toml`, `backend/app/main.py`, `backend/app/db.py`,
  `backend/app/routers/`, `backend/app/schemas/`, `backend/app/models/`,
  `backend/app/seed.py`
- Migrations: `backend/alembic.ini`, `backend/migrations/env.py`,
  `backend/migrations/versions/cd9306b0de55_create_sample_data_table.py`

## Regenerating the diagrams

The Mermaid source files are in `docs/diagrams/` next to the SVG output. To
re-render after editing the source, run these from the repo root (Node is
required; the first run downloads the Mermaid CLI and a headless browser):

```sh
npx -y @mermaid-js/mermaid-cli -i docs/diagrams/architecture.mmd -o docs/diagrams/architecture.svg -b white
npx -y @mermaid-js/mermaid-cli -i docs/diagrams/request-lifecycle.mmd -o docs/diagrams/request-lifecycle.svg -b white
```

High-resolution PNG copies are also kept for slides and print. The `-s 4` flag
renders at 4x pixel density so the text stays sharp when zoomed or printed:

```sh
npx -y @mermaid-js/mermaid-cli -i docs/diagrams/architecture.mmd -o docs/diagrams/architecture.png -b white -s 4
npx -y @mermaid-js/mermaid-cli -i docs/diagrams/request-lifecycle.mmd -o docs/diagrams/request-lifecycle.png -b white -s 4
```

Files in `docs/diagrams/`:

- `architecture.mmd` - source for the static architecture view
- `architecture.svg` - vector output (embedded above)
- `architecture.png` - high-resolution raster, 3136 x 3136 px
- `request-lifecycle.mmd` - source for the runtime request path
- `request-lifecycle.svg` - vector output (embedded above)
- `request-lifecycle.png` - high-resolution raster, 3136 x 1464 px

## Related documents

- `surplus-development-workflows.md` - the team's how-to guide for changing each
  tier. This document shows how a request flows at run time; the workflows
  document shows the step-by-step process you follow to change the frontend, the
  backend, or the database.
