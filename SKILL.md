---
name: project-docs-generator
description: Generate a structured `docs/` documentation tree plus a polished root `README.md` for a software project, derived from the actual source code. Use this skill whenever the user asks to "document the project", "create documentation", "set up docs/", "write a README", "generate project docs", "criar documentação", "gerar documentação", or any equivalent request about producing project-level documentation. Triggers for projects of any language or stack (Node.js, Python, Go, Rust, Java, etc.) and works on greenfield as well as existing repos — including cases where `docs/` or `README.md` already exist (the skill prompts before overwriting).
---

# Project Docs Generator

This skill produces two artifacts for a software project, **grounded in the real source code** (never hallucinated):

1. **`docs/`** — a structured documentation tree (index + overview + getting-started + deployment + architecture + domains + reference), broken into separate files per domain/concern, with internal links.
2. **`README.md`** — a polished, scannable project README at the repo root, derived from a generic template adapted to the actual project.

The skill is **interactive at two specific checkpoints** to avoid destroying user work, and **language-/stack-agnostic** — it inspects the repo to figure out what kind of project it is rather than assuming.

---

## When to use this skill

Use it any time the user wants project-level documentation produced. Typical triggers:

- "Document this project"
- "Create a docs/ folder"
- "Write a README for this repo"
- "Generate documentation"
- "Crie/gere documentação estruturada"
- "Adicione um README"
- "Set up project docs"

Do **not** use this skill for:
- Inline code comments / docstrings (different concern)
- API reference generation from typed schemas (e.g. OpenAPI, Sphinx autodoc)
- A single one-off file the user already named (e.g. "write me a CONTRIBUTING.md") — just write the file directly

---

## Workflow

Follow these steps in order. The two interactive checkpoints (steps 2 and 4) exist specifically to avoid destroying existing user work — do not skip them.

### Step 1 — Understand the project

Before writing anything, build a real mental model of the repository:

- Read manifest / lock files to identify language and stack: `package.json`, `pyproject.toml`, `requirements.txt`, `Cargo.toml`, `go.mod`, `pom.xml`, `composer.json`, `Gemfile`, `mix.exs`, etc.
- Read entry-point files (e.g. `server.js`, `main.py`, `cmd/*/main.go`, `src/main/...`).
- Skim the top-level directory layout; identify domain folders.
- Read existing `.env.example` / config samples, `Dockerfile`, `docker-compose.yml`, CI config (`.github/workflows/*`, `.gitlab-ci.yml`, etc.).
- Identify integrations (databases, queues, third-party APIs).
- Look at recent commit messages (`git log --oneline -20`) to learn the commit-message convention.
- Note testing setup (Jest, pytest, go test, etc.).

You're allowed to dispatch parallel reads — the goal is to understand the project well enough that every claim in the docs is verifiable in the code. **Never invent features, endpoints, env vars, or commands. Cite real file paths in the docs.**

### Step 2 — Decide what to do with `docs/` (interactive)

Check whether `docs/` exists at the repo root:

- **If `docs/` does NOT exist**: proceed to step 3 and create it.
- **If `docs/` exists**: ask the user, with these two options:
  1. **Update / regenerate `docs/`** — proceed to step 3.
  2. **Keep `docs/` as-is and only generate the README** — skip to step 4.

Use the `AskUserQuestion` tool (or equivalent) when available. The question should be specific, e.g.:

> "I noticed `docs/` already exists. Do you want me to (a) refresh/regenerate the documentation tree, or (b) keep it as-is and only generate the root README?"

Respect the answer exactly.

### Step 3 — Generate the `docs/` tree

Use the structure documented in [`references/docs-structure.md`](references/docs-structure.md). The default layout is:

```
docs/
├── README.md                     # Index / navigation
├── overview.md                   # What the project does + high-level architecture
├── getting-started.md            # Local dev setup
├── deployment.md                 # Production requirements + release flow
├── architecture/
│   ├── project-structure.md
│   ├── <interface>.md            # e.g. http-api.md, cli.md, library-api.md
│   ├── <scheduled>.md            # e.g. cron-jobs.md, workers.md (only if relevant)
│   └── integrations.md
├── domains/
│   └── <one-file-per-domain>.md  # one file per major feature/automation/module
└── reference/
    ├── environment-variables.md
    └── database.md               # only if the project uses a DB
```

**Adapt the structure to the project** — don't include files that don't apply. For example:
- Pure library? Replace `http-api.md` with `library-api.md`, drop `deployment.md` in favor of a publish section in `getting-started.md`.
- CLI tool? Use `cli.md` instead of `http-api.md`.
- No scheduled jobs? Omit the file.
- No databases? Drop `reference/database.md`.

**Writing rules for `docs/`:**

- **English** (unless the user explicitly requested another language).
- Each file is self-contained and links to siblings via relative paths.
- Link source files with markdown links, e.g. `[src/api/routes.js](../../src/api/routes.js)`.
- Every factual claim must be verifiable in the code you read.
- Practical-overview depth by default — explain what each module does and where to find logic; do not exhaustively document every helper function.
- `docs/README.md` is the navigation index — it points to every other doc and groups them under "Start here / Architecture / Domains / Reference" headings.

If the project uses anything non-obvious (special env-var conventions, multiple databases, framework quirks), put those in `reference/` rather than scattering them across other files.

### Step 4 — Decide what to do with root `README.md` (interactive)

Check whether `README.md` exists at the repo root:

- **If `README.md` does NOT exist**: proceed to step 5.
- **If `README.md` exists**: ask the user:

> "A `README.md` already exists at the project root. Do you want me to overwrite it with a generated one based on this skill's template?"

- **If the user declines**: stop. Do not write any README. Report what was done in `docs/` (if anything) and exit.
- **If the user accepts**: proceed to step 5.

This checkpoint is non-negotiable: silently overwriting a hand-written README is a serious regression.

### Step 5 — Generate the README from a temporary template

The template is embedded in [`assets/readme-template.md`](assets/readme-template.md). It is intentionally generic so it works for any stack. Process:

1. **Copy** the template to a temporary location (e.g. `/tmp/readme-<random>.md` or a system temp file). **Never write the template into `docs/`** — the template is a skill internal, not a project artifact.
2. **Adapt** the template to the actual project:
   - Replace every `{{PLACEHOLDER}}` with concrete content.
   - Rename / drop sections that don't fit (e.g. an API "curl" example for a CLI tool should become a CLI invocation; a library should show import + usage).
   - Adjust badges to match the real stack (Node.js / Python / Go badge; license badge matching the actual license in `package.json` / `pyproject.toml` / `LICENSE`).
   - Trim the feature list to features the code actually has.
   - Build the project tree from the actual directory layout.
   - Pull installation / run commands from real scripts in `package.json` / `Makefile` / `pyproject.toml` / etc.
   - Use real endpoint paths / CLI commands for the Usage section.
   - Adapt the "Tech Stack" table to actual dependencies.
3. **Write** the adapted README to `<project-root>/README.md`.
4. **Delete** the temporary template file.

**Tone of the README:**
- Concise, scannable, developer-friendly.
- Emojis are welcome for section headers (matches the template's style) — but don't pepper every line.
- GitHub-flavored Markdown.
- In English (unless the user asked otherwise).

If `docs/` was generated (or already exists with meaningful content), the README's "Links" section should link to `docs/README.md` as the entry point for deeper documentation.

### Step 6 — Wrap up

Report to the user, briefly:

- Which files were created / updated under `docs/` (or "left as-is" if the user opted to keep it).
- Whether the root `README.md` was created, overwritten, or skipped.
- Any flags raised during code reading (e.g. an obvious bug, a typo in a filename, a missing env-var in `.env.example`).

Do **not** commit. Let the user review and commit themselves.

---

## Adapting the template per stack

The template in `assets/readme-template.md` is intentionally light on assumptions. Use this guide to adapt it intelligently:

**Node.js / TypeScript project**
- Install: `npm install` / `pnpm install` / `yarn`
- Run: extract from `package.json` `scripts`
- Tech stack table: Node version, framework (Express / Fastify / Next.js / Nest), DB driver, test runner
- Usage: HTTP examples if it's a server; CLI if there's a `bin` field; `import` if it's a library

**Python project**
- Install: `pip install -r requirements.txt` or `pip install -e .` or `poetry install` or `uv sync` — pick whichever matches the lock/manifest present
- Run: `python -m <module>` / `uvicorn app:main` / `python main.py` — read the entry point
- Tech stack table: Python version, framework (FastAPI / Django / Flask), DB lib, test runner (pytest)
- Usage: API examples for web apps; `python -c "..."` for libraries

**Go project**
- Install: `go build ./...`
- Run: `go run ./cmd/<name>` or the produced binary
- Tech stack table: Go version, key modules
- Usage: CLI invocation or HTTP examples

**Rust project**
- Install: `cargo build --release`
- Run: `cargo run` or the binary path
- Tech stack table: Rust edition, key crates
- Usage: CLI or library `use` example

**Java / Kotlin project**
- Install: `./mvnw install` or `./gradlew build`
- Run: `java -jar target/...jar` or `./gradlew bootRun`
- Tech stack: Spring Boot / Quarkus / Ktor, etc.

**Polyglot / monorepo**
- Treat each top-level package separately in the project tree; in the install/usage section, document the **most common** entry path and link to package-specific docs for the rest.

Always cross-check the README against the docs you produced (or that already exist) — sections must agree.

---

## Critical rules (don't break these)

1. **Never invent.** Every command, env var, path, endpoint, feature, and dependency must exist in the code you actually read.
2. **Never overwrite a `README.md` without explicit consent.** The interactive checkpoint in step 4 is mandatory.
3. **Never put the README template inside `docs/`.** The template is a skill-internal asset; it must only land in a temporary location during generation.
4. **Adapt, don't transplant.** Treat the template's sections as a *layout*, not a verbatim output. Sections that don't fit should be renamed or dropped.
5. **Write in English by default.** Switch only if the user explicitly says otherwise.
6. **Don't commit.** Leave files staged in the working directory for the user to review.

---

## Reference files

- [`assets/readme-template.md`](assets/readme-template.md) — the generic README layout, copied to a temp location and adapted per project. Source of truth for README structure.
- [`references/docs-structure.md`](references/docs-structure.md) — recommended `docs/` tree, per-file content guide, and adaptation rules.
- [`references/writing-guide.md`](references/writing-guide.md) — voice, tone, and depth guidelines for the generated docs and README.
