# `docs/` Tree Structure Guide

This document defines the recommended documentation layout produced by `project-docs-generator` and how to adapt it per project type.

## Default tree

```
docs/
├── README.md                     # Index / navigation
├── overview.md                   # What the project does + architecture sketch
├── getting-started.md            # Local dev setup (clone → install → run → test)
├── deployment.md                 # Production requirements + release flow
├── architecture/
│   ├── project-structure.md      # Directory map + responsibility per folder
│   ├── http-api.md               # If the project exposes HTTP
│   ├── cli.md                    # If the project exposes a CLI
│   ├── library-api.md            # If the project is consumed as a library
│   ├── cron-jobs.md              # If the project schedules work
│   ├── workers.md                # If the project has background workers / queues
│   └── integrations.md           # External services consumed (DBs, APIs, SaaS)
├── domains/
│   └── <one-file-per-domain>.md  # One file per major feature, automation, or module
└── reference/
    ├── environment-variables.md
    └── database.md               # Only if the project uses a DB
```

## Per-file content guide

### `docs/README.md` (the index)
- Tagline (one sentence).
- Grouped link list under three or four headings: **Start here**, **Architecture**, **Domains**, **Reference**.
- Every file in the tree should be reachable from here.

### `overview.md`
- What the project does (concrete, grounded in code).
- Why it matters (business / user value).
- High-level architecture (ASCII or mermaid diagram is welcome).
- Tech stack at a glance.
- Entry-point file table (file path + purpose).
- Link to `architecture/project-structure.md` for the full layout.

### `getting-started.md`
- Prerequisites (runtime version, package manager, optional tools).
- Clone + install commands (from real manifest).
- Configure (.env example with the minimum vars needed to boot).
- Run (dev command + Docker form if available).
- Verify (a liveness check, a `--help`, or a basic API call).
- Optional: tests.
- Optional: manual CLI helpers / scripts.

### `deployment.md`
- Runtime requirements (host, OS, system dependencies — e.g. Chrome for Puppeteer, ffmpeg, JDK).
- Required credentials / secrets (env-var groups).
- Persistence requirements (volumes, databases, caches).
- Health-check endpoint(s).
- Release flow (CI/CD pipeline, branch model, tag conventions).
- Rollback procedure.
- Observability checklist (logs, metrics, error reporting).

### `architecture/project-structure.md`
- Top-level layout (tree).
- Per-folder responsibility table.
- Where to put new code by concern.

### `architecture/<interface>.md` (http-api / cli / library-api)
- Endpoint or command or symbol catalog.
- Request/response (or input/output) shape for each.
- Error-handling conventions.
- Authentication, if any.

### `architecture/cron-jobs.md` / `workers.md`
- Schedule table (name → cron expr → timezone → what it runs).
- Enable flags / config that controls activation.
- Concurrency guards.
- Failure handling.

### `architecture/integrations.md`
- One section per external system (DB, API, mail provider, monitoring, etc.).
- For each: library used, configuration file, credentials, expected permissions, failure modes.

### `domains/<feature>.md`
- One per major feature or automation.
- Section 1: Where the logic lives (folder map + entry points).
- Section 2: High-level flow (numbered steps).
- Section 3: Inputs / outputs.
- Section 4: How it's triggered (HTTP route, CLI command, cron, message).
- Section 5: Tests.

### `reference/environment-variables.md`
- Grouped tables (Service / DB / Third-party / Monitoring / etc.).
- Columns: Var, Default, Required, Description.
- Source of truth: cross-reference with `.env.example`.

### `reference/database.md`
- Per-database section (host config, schemas, search path).
- Repository → table mapping.
- Pointers to SQL/migration files.

## Adaptation rules

**Library / SDK projects** — replace `http-api.md` with `library-api.md`. Drop `deployment.md` and add a publish/release section to `getting-started.md`. Drop `domains/` if the library has a single concern.

**Pure CLI tools** — use `cli.md` instead of `http-api.md`. Often `domains/` collapses into a few "command groups" instead of features.

**Background-only services** — drop `http-api.md`; lead with `workers.md` or `cron-jobs.md`.

**Monorepos** — add a top-level `packages.md` that links into each package's section. Domains then live per package.

**Greenfield / very small projects** — collapse into a flat `docs/`:
```
docs/
├── README.md
├── overview.md
├── getting-started.md
└── reference.md
```

## Writing rules

- Use **GitHub-flavored Markdown**.
- Link source files with relative paths from the doc's location (`../../src/...`).
- Every claim must be checkable in the code. **No invented APIs, endpoints, env vars, or commands.**
- **Practical-overview** depth by default — describe modules and where their logic lives, don't catalogue every function.
- One topic per file. If a file is growing past ~300 lines, split it.
- Internal cross-links liberally — readers should never get stuck.
- Code blocks use language tags (` ```bash`, ` ```json`, ` ```js`).
