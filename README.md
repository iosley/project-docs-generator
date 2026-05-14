# 📚 project-docs-generator

> A Claude Code / Claude Agent SDK skill that generates a structured `docs/` documentation tree plus a polished root `README.md` for a software project — grounded in the actual source code, language- and stack-agnostic.

<p align="center">
  <img alt="Claude Code Skill" src="https://img.shields.io/badge/Claude%20Code-Skill-D77757">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-blue">
</p>

---

## ✨ What it does

Point it at any repository and it will:

1. **Read the actual source code** (manifests, entry points, configs, CI files) so every claim in the docs is verifiable.
2. **Generate a structured `docs/` tree** — index, overview, getting-started, deployment, architecture/, domains/, reference/ — adapted to the project's shape (HTTP service, CLI, library, monorepo, etc.).
3. **Generate a polished root `README.md`** from an embedded template, adapted to the real stack.
4. **Respect existing work** — interactive checkpoints before regenerating `docs/` or overwriting `README.md`.

Works on **Node.js, Python, Go, Rust, Java, Kotlin, Ruby, Elixir, PHP** — basically any language with a manifest file.

## 🎯 When it triggers

Use any of:

- "document this project"
- "create documentation"
- "set up docs/"
- "write a README"
- "generate project docs"
- "criar/gerar documentação"

Or invoke explicitly via the Skill tool with `project-docs-generator`.

## 🛡️ Safety guarantees

- **Never overwrites your `README.md` without explicit consent.**
- **Never modifies an existing curated `docs/`** unless you opt in.
- **Never invents content** — every command, env var, endpoint, dependency cited in the output must exist in the code that was read.
- **README template is processed in a temp location** — it never lands inside `docs/`.

## 📦 Installation

### Claude Code (recommended)

Download the latest [`project-docs-generator.skill`](https://github.com/iosley/project-docs-generator/releases) release and install:

```bash
# from a Claude Code session
/skill install ./project-docs-generator.skill
```

Or clone directly into your skills directory:

```bash
git clone https://github.com/iosley/project-docs-generator.git \
  ~/.claude/skills/project-docs-generator
```

The skill is auto-discovered on the next session — no extra config required.

### Codex CLI

Codex auto-discovers skills from your home skills directory. Clone the repo into it:

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/iosley/project-docs-generator.git \
  ~/.codex/skills/project-docs-generator
```

Or, if you prefer the packaged form, download `project-docs-generator.skill` (it's a zip) and extract it into the same location:

```bash
mkdir -p ~/.codex/skills
unzip project-docs-generator.skill -d ~/.codex/skills/
```

Verify it's available by starting a Codex session — the skill should appear in `available_skills` under the name `project-docs-generator`. Codex maps Claude's tool names to its own equivalents automatically, so the skill works without modification.

### Other agent environments

The skill is plain markdown — copy the folder into whichever skills directory your environment uses (see your platform's docs for the exact path). Common locations:

| Environment | Default skills directory |
| --- | --- |
| Claude Code | `~/.claude/skills/` |
| Codex CLI | `~/.codex/skills/` |
| Copilot CLI | `~/.config/gh-copilot/skills/` (varies) |
| Gemini CLI | `~/.gemini/skills/` |

## 🚀 Usage

Once installed, just ask:

```text
> Document this project — generate docs/ and a README.md
```

The skill will:

1. Inspect your repo.
2. If `docs/` exists, ask whether to refresh it or keep it as-is.
3. If `README.md` exists, ask whether to overwrite it. If you say no, it stops without writing.
4. Write the documentation, grounded in your actual code.

### Example output structure

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

## 🗂️ Skill contents

```
project-docs-generator/
├── SKILL.md                           # Skill manifest + workflow
├── README.md                          # This file
├── assets/
│   └── readme-template.md             # Generic README layout (copied to temp + adapted)
└── references/
    ├── docs-structure.md              # Per-file content guide + adaptation rules
    └── writing-guide.md               # Voice, tone, depth, length budgets
```

## 🧪 Benchmark

Measured on three eval cases (Node.js greenfield, Python CLI with preserved README, Node service with existing docs):

| Metric | With skill | Baseline (no skill) | Delta |
| --- | --- | --- | --- |
| **Pass rate** | **100%** (25/25) | 83% (21/25) | **+17 pp** |
| Time | ~147s | ~108s | +38s |
| Tokens | ~45k | ~30k | +14k |

The skill enforces convention (file naming, structure, language adaptation) that an unguided model improvises away. The extra time/tokens are spent producing more files of consistent quality.

## 🗺️ Roadmap

- [x] Stack auto-detection (Node, Python, Go, Rust, Java, Ruby, …)
- [x] Interactive checkpoints for `docs/` and `README.md`
- [x] Adaptive `docs/` structure (HTTP vs CLI vs library)
- [x] Embedded generic README template
- [ ] Optional `--language pt-BR` / `--language es` for non-English output
- [ ] Mermaid diagram generation for architecture overview
- [ ] `--depth=deep` mode for full per-function reference docs
- [ ] CONTRIBUTING.md / CODE_OF_CONDUCT.md generation as opt-in extras

## 🤝 Contributing

Contributions welcome:

1. Fork and create a feature branch.
2. Open a PR against `main` describing the change and including:
   - An eval case in `evals/evals.json` (if you're changing behavior).
   - The before/after benchmark numbers (if you're optimizing).
3. Follow Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`).

## 📄 License

MIT. See [`LICENSE`](LICENSE).

## 🔗 Links

- 📖 **Skill manifest:** [`SKILL.md`](SKILL.md)
- 🧭 **`docs/` structure guide:** [`references/docs-structure.md`](references/docs-structure.md)
- ✍️ **Writing guide:** [`references/writing-guide.md`](references/writing-guide.md)
- 🤖 **Claude Code Skills docs:** [docs.claude.com/claude-code/skills](https://docs.claude.com/claude-code/skills)

---

<p align="center">
  Built for <strong>Claude Code</strong> and the <strong>Claude Agent SDK</strong>.
</p>
