<!--
  README LAYOUT TEMPLATE — for the project-docs-generator skill.
  This file is generic and stack-agnostic. The skill copies it to a temp
  location, then adapts EVERY section to the real project:
    - Replace {{...}} placeholders with concrete values.
    - Drop sections that don't apply (e.g. HTTP examples for a CLI tool).
    - Rename sections when the project shape demands it.
  Do NOT write this template verbatim into the user's project.
-->

# {{EMOJI}} {{Project Name}}

> {{One-sentence pitch grounded in what the code actually does.}}

<p align="center">
  <a href="#-overview">Overview</a> •
  <a href="#-features">Features</a> •
  <a href="#%EF%B8%8F-tech-stack">Tech Stack</a> •
  <a href="#-installation">Install</a> •
  <a href="#-usage">Usage</a> •
  <a href="#%EF%B8%8F-roadmap">Roadmap</a> •
  <a href="#-contributing">Contributing</a>
</p>

<!-- Badges: pick ones that match the actual stack. Drop the others. -->
<p align="center">
  <img alt="{{Language}}" src="https://img.shields.io/badge/{{language}}-{{version}}-blue">
  <img alt="License"      src="https://img.shields.io/badge/license-{{license}}-blue">
  <!-- Optional: <img alt="CI" src="..."> <img alt="Version" src="..."> -->
</p>

---

## 🧭 Overview

**What it does.** {{Concrete description anchored in the real code — what the service / CLI / library actually does.}}

**Why it matters.** {{The problem solved or the value delivered.}}

**Who it's for.** {{The intended audience — internal team, OSS users, specific role, etc.}}

---

## ✨ Features

<!-- Trim aggressively — only list features the code actually has. -->
- {{emoji}} **{{Feature 1}}** — {{one line}}
- {{emoji}} **{{Feature 2}}** — {{one line}}
- {{emoji}} **{{Feature 3}}** — {{one line}}
- {{emoji}} **{{Feature 4}}** — {{one line}}

---

## 🛠️ Tech Stack

<!-- Build this table from the actual manifest (package.json / pyproject.toml / Cargo.toml / etc.). -->

| Layer | Technology | Purpose |
| --- | --- | --- |
| {{emoji}} Runtime | {{e.g. Node.js 20 / Python 3.12 / Go 1.22}} | {{Purpose}} |
| {{emoji}} {{Layer}} | {{Tech}} | {{Purpose}} |
| {{emoji}} {{Layer}} | {{Tech}} | {{Purpose}} |
| {{emoji}} Testing | {{Test framework}} | {{Purpose}} |

---

## 📦 Installation

### Prerequisites
- {{Runtime version, e.g. Node.js 20+ / Python 3.11+ / Go 1.22+}}
- {{Package manager, e.g. npm / pip / cargo}}
- {{Optional: Docker, a database, a third-party API key}}

### Clone & install

```bash
git clone <repo-url>
cd {{project-folder}}

# {{install command — e.g.}} npm install
# {{or}}                      pip install -r requirements.txt
# {{or}}                      cargo build
```

### Configure

<!-- Only include this block if the project has env vars / config. -->
```bash
cp .env.example .env
# edit .env — see docs/reference/environment-variables.md (if present)
```

### Run

```bash
# {{development command — pull from package.json scripts / Makefile / pyproject / etc.}}
{{run-command}}

# {{optional docker form}}
docker compose up --build
```

---

## 🚀 Usage

<!--
  Adapt this section to the project type. Pick the form(s) that match:
  - HTTP service → curl examples against real endpoints
  - CLI tool    → terminal invocation with real subcommands/flags
  - Library     → import + minimal example in the project's language
-->

### {{HTTP example | CLI example | Library example}}

```bash
{{real, concrete invocation against this project — never a placeholder}}
```

```{{language}}
{{real, concrete code snippet using the project's API}}
```

For the full reference, see {{link to deeper docs — e.g. docs/architecture/http-api.md, or the CLI's --help}}.

---

## 🗂️ Project Structure

<!-- Build from the actual directory tree. Annotate the important folders. -->

```
{{project-folder}}/
├── {{entry-point}}                    # {{purpose}}
├── {{config-file}}                    # {{purpose}}
├── docs/                              # Full documentation (see docs/README.md)
└── {{src-folder}}/
    ├── {{folder}}/                    # {{purpose}}
    ├── {{folder}}/                    # {{purpose}}
    └── {{folder}}/                    # {{purpose}}
```

---

## 🧪 Testing

```bash
{{test-command}}                       # e.g. npm test / pytest / go test ./... / cargo test
```

{{Optional: coverage / lint / type-check commands if they exist in the project.}}

---

## 🗺️ Roadmap

<!-- Mark items that are actually shipped. Future items optional. -->

- [x] {{Shipped feature 1}}
- [x] {{Shipped feature 2}}
- [ ] {{Planned item 1}}
- [ ] {{Planned item 2}}

---

## 🤝 Contributing

Contributions are welcome. The recommended flow:

1. Fork / branch off the default branch with a descriptive name.
2. Install dev dependencies and confirm the test suite passes.
3. Follow the repo's commit-message convention {{e.g. Conventional Commits}}.
4. Open a PR / MR against the default branch.

{{If the project has a CONTRIBUTING.md or CODE_OF_CONDUCT.md, link them here.}}

---

## 📄 License

Distributed under the **{{License}} License**. See [`LICENSE`](LICENSE) {{or the relevant manifest field}} for details.

---

## 🔗 Links

- 📚 **Documentation:** [`docs/README.md`](docs/README.md)
{{- Drop this line if no docs/ was produced. -}}
- {{🌐}} **{{Homepage / Demo / Dashboard}}:** {{url}}
- 📧 **Contact:** {{email or team handle}}

---

<p align="center">
  Built with ❤️ by {{author / team}}.
</p>
