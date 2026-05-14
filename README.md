# 📚 project-docs-generator

> A Claude Code / Claude Agent SDK skill that generates a structured `docs/` tree plus a polished root `README.md` for any software project — grounded in the actual source code, language- and stack-agnostic.

<p align="center">
  <a href="#-overview">Overview</a> •
  <a href="#-features">Features</a> •
  <a href="#%EF%B8%8F-tech-stack">Tech Stack</a> •
  <a href="#-installation">Install</a> •
  <a href="#-usage">Usage</a> •
  <a href="#-testing">Testing</a> •
  <a href="#%EF%B8%8F-roadmap">Roadmap</a> •
  <a href="#-contributing">Contributing</a>
</p>

<p align="center">
  <img alt="Claude Code Skill" src="https://img.shields.io/badge/Claude%20Code-Skill-D77757">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-blue">
</p>

---

## 🧭 Overview

**What it does.** Point it at any repository and it produces (1) a structured `docs/` tree — index, overview, getting-started, deployment, architecture/, domains/, reference/ — adapted to the project's shape (HTTP service, CLI, library, monorepo, etc.), and (2) a polished root `README.md` derived from an embedded template. Every claim is verifiable against the real code; nothing is invented.

**Why it matters.** Project documentation is consistently postponed and inconsistently structured across teams. This skill enforces a sensible default layout, reads the real source code, and respects existing work — making "document this project" a one-line ask instead of a half-day chore.

**Who it's for.** Developers and teams using Claude Code, the Claude Agent SDK, Codex CLI, or any other agent environment that loads skills — on greenfield repos as well as existing codebases with partial documentation.

---

## ✨ Features

- 🌍 **Stack auto-detection** — Node.js, Python, Go, Rust, Java, Kotlin, Ruby, Elixir, PHP — any language with a manifest file.
- 🛡️ **Safe by default** — interactive checkpoints before regenerating `docs/` or overwriting `README.md`; never invents commands, env vars, or endpoints.
- 🌐 **Localization** — pass `--language pt-BR` / `--language es` (or just say it) to produce non-English output; identifiers, paths, and commands stay in their original form.
- 📊 **Mermaid diagrams** — architecture flowcharts and sequence diagrams embedded in `overview.md` and `architecture/`, grounded in real modules.
- 🔬 **Deep reference mode** — `--depth=deep` adds per-function reference docs under `reference/api-reference.md`.
- 📜 **Opt-in extras** — `--extras` generates `CONTRIBUTING.md` and a pointer-style `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1).
- 🧩 **Adaptive structure** — HTTP service, CLI tool, library, monorepo, and very small projects each get a tailored tree.

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
| --- | --- | --- |
| 🤖 Runtime | Claude Code / Claude Agent SDK / Codex CLI | Hosts the skill |
| 📝 Format | Plain Markdown (`SKILL.md` + references) | Skill definition — no executable code |
| 🧠 Tools used | `Read`, `Write`, `Edit`, `Bash`, `AskUserQuestion` | Provided by the host agent |
| 📊 Diagrams | Mermaid (native GitHub/GitLab rendering) | Architecture visuals in generated docs |
| 🧪 Evals | `evals/evals.json` (3 fixture-based cases) | Regression and pass-rate measurement |

---

## 📦 Installation

### Prerequisites
- A skill-aware agent host: **Claude Code**, the **Claude Agent SDK**, **Codex CLI**, **Copilot CLI**, or **Gemini CLI**.
- `git` for the clone-based install path.

### Claude Code (recommended)

Install directly from the GitHub repository — no manual download required:

```bash
npx skills add https://github.com/iosley/project-docs-generator
```

The same command works with the SSH URL if you prefer:

```bash
npx skills add git@github.com:iosley/project-docs-generator.git
```

#### Alternative — clone into your skills directory

```bash
git clone https://github.com/iosley/project-docs-generator.git \
  ~/.claude/skills/project-docs-generator
```

#### Alternative — install a downloaded `.skill` bundle

Grab the latest [`project-docs-generator.skill`](https://github.com/iosley/project-docs-generator/releases) release, then from a Claude Code session:

```text
/skills install ./project-docs-generator.skill
```

In any case, the skill is auto-discovered on the next session — no extra config required.

### Codex CLI

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/iosley/project-docs-generator.git \
  ~/.codex/skills/project-docs-generator
```

Or extract the packaged form:

```bash
mkdir -p ~/.codex/skills
unzip project-docs-generator.skill -d ~/.codex/skills/
```

Codex maps Claude's tool names to its own equivalents automatically, so the skill works without modification.

### Other agent environments

The skill is plain markdown — copy the folder into whichever skills directory your environment uses:

| Environment | Default skills directory |
| --- | --- |
| Claude Code | `~/.claude/skills/` |
| Codex CLI | `~/.codex/skills/` |
| Copilot CLI | `~/.config/gh-copilot/skills/` (varies) |
| Gemini CLI | `~/.gemini/skills/` |

---

## 🚀 Usage

Once installed, just ask:

```text
> Document this project — generate docs/ and a README.md
```

Triggers also include: "create documentation", "set up docs/", "write a README", "generate project docs", "criar/gerar documentação".

### With options

The skill detects flags and natural phrases — you don't need to memorize a CLI grammar:

```text
> Document this project in pt-BR with deep reference docs and include CONTRIBUTING/CoC
```

Equivalent to `--language pt-BR --depth=deep --extras`. Mermaid diagrams are on by default; opt out with `--no-mermaid` or "skip diagrams".

### What happens

1. The skill inspects your repo (manifests, entry points, CI configs).
2. If `docs/` exists, it asks whether to refresh it or keep it as-is.
3. If `README.md` exists, it asks whether to overwrite it. If you say no, it stops without writing.
4. It writes the documentation grounded in your actual code and reports what changed.

### Example output

For a typical Node.js HTTP service:

```
your-project/
├── README.md                          # Polished, badges + sections
└── docs/
    ├── README.md                      # Navigation index
    ├── overview.md
    ├── getting-started.md
    ├── deployment.md
    ├── architecture/
    │   ├── project-structure.md
    │   ├── http-api.md
    │   ├── cron-jobs.md
    │   └── integrations.md
    ├── domains/
    │   └── <one-file-per-feature>.md
    └── reference/
        ├── environment-variables.md
        └── database.md
```

The exact tree adapts: a CLI tool gets `cli.md` instead of `http-api.md`; a library drops `deployment.md`; a small project collapses to a flat layout. See [`references/docs-structure.md`](references/docs-structure.md) for the full adaptation guide.

---

## 🗂️ Project Structure

```
project-docs-generator/
├── SKILL.md                           # Skill manifest + workflow (Step 0 → Step 7)
├── README.md                          # This file
├── LICENSE
├── assets/
│   └── readme-template.md             # Generic README layout (copied to temp + adapted)
├── references/
│   ├── docs-structure.md              # Per-file content guide, depth modes, adaptation
│   ├── writing-guide.md               # Voice, tone, depth modes, localization rules
│   ├── mermaid-patterns.md            # When to use diagrams + templates per project type
│   └── extras-templates.md            # CONTRIBUTING + CODE_OF_CONDUCT (pointer-style)
└── evals/
    └── evals.json                     # Fixture-based regression suite
```

---

## 🧪 Testing

The skill ships with three eval cases in [`evals/evals.json`](evals/evals.json):

| Case | Scenario |
| --- | --- |
| `greenfield-node-http-service` | Node.js + Express + Knex + Postgres, no docs or README yet |
| `python-cli-readme-overwrite-declined` | Python CLI with hand-written README that must be preserved |
| `existing-docs-keep-readme-only` | Node service with curated `docs/` that must stay untouched |

Run them through the skill-evaluation harness of your host agent. Each case asserts both **what was produced** and **what was preserved**.

### Benchmark

| Metric | With skill | Baseline (no skill) | Delta |
| --- | --- | --- | --- |
| **Pass rate** | **100%** (25/25) | 83% (21/25) | **+17 pp** |
| Time | ~147s | ~108s | +38s |
| Tokens | ~45k | ~30k | +14k |

The skill enforces convention (file naming, structure, language adaptation) that an unguided model improvises away. The extra time/tokens are spent producing more files of consistent quality.

---

## 🗺️ Roadmap

- [x] Stack auto-detection (Node, Python, Go, Rust, Java, Ruby, …)
- [x] Interactive checkpoints for `docs/` and `README.md`
- [x] Adaptive `docs/` structure (HTTP vs CLI vs library)
- [x] Embedded generic README template
- [x] Optional `--language pt-BR` / `--language es` for non-English output
- [x] Mermaid diagram generation for architecture overview
- [x] `--depth=deep` mode for full per-function reference docs
- [x] CONTRIBUTING.md / CODE_OF_CONDUCT.md generation as opt-in extras

---

## 🤝 Contributing

Contributions welcome:

1. Fork and create a feature branch off `main`.
2. Open a PR against `main` describing the change and including:
   - An eval case in [`evals/evals.json`](evals/evals.json) (if you're changing behavior).
   - Before/after benchmark numbers (if you're optimizing).
3. Follow Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`).

---

## 📄 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

## 🔗 Links

- 📖 **Skill manifest:** [`SKILL.md`](SKILL.md)
- 🧭 **`docs/` structure guide:** [`references/docs-structure.md`](references/docs-structure.md)
- ✍️ **Writing guide:** [`references/writing-guide.md`](references/writing-guide.md)
- 📊 **Mermaid patterns:** [`references/mermaid-patterns.md`](references/mermaid-patterns.md)
- 📜 **Extras templates:** [`references/extras-templates.md`](references/extras-templates.md)
- 🤖 **Claude Code Skills docs:** [docs.claude.com/claude-code/skills](https://docs.claude.com/claude-code/skills)

---

<p align="center">
  Built for <strong>Claude Code</strong> and the <strong>Claude Agent SDK</strong>.
</p>
