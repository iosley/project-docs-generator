# Extras Templates — CONTRIBUTING & CODE_OF_CONDUCT

These templates are generated only when the user opts in via `--extras`, `--contributing`, `--code-of-conduct`, or an explicit phrase ("include contributing", "gerar code of conduct", "add CoC").

Both files land at the **project root** (not under `docs/`). Each generation respects the same overwrite checkpoint as `README.md`: if the file already exists, ask before replacing it.

---

## CONTRIBUTING.md

Generate from real signals detected in the repo. **Do not invent commands** — pull them from `package.json`, `Makefile`, `pyproject.toml`, `Cargo.toml`, CI configs, and recent commit messages.

### Detection checklist

| Signal | Where to find it | Used in section |
| --- | --- | --- |
| Install command | manifest scripts, README | "Local setup" |
| Test command | `package.json` `scripts.test`, `pytest.ini`, `go test`, CI config | "Running tests" |
| Lint / format command | `scripts.lint`, `.eslintrc`, `ruff.toml`, `.golangci.yml` | "Code style" |
| Commit convention | `git log --oneline -30` — look for `feat:` / `fix:` prefixes | "Commit messages" |
| PR template | `.github/pull_request_template.md` | "Submitting changes" |
| Branch model | CI triggers, recent branch names | "Branching" |
| Issue templates | `.github/ISSUE_TEMPLATE/` | "Reporting issues" |

### Template

```markdown
# Contributing to {{PROJECT_NAME}}

Thanks for your interest in contributing! This guide covers how to set up the project locally, the conventions we follow, and the path from idea to merged change.

## Code of Conduct

By participating in this project you agree to abide by the [Code of Conduct](./CODE_OF_CONDUCT.md). Please report unacceptable behavior to {{CONTACT_EMAIL}}.

## Getting started

1. Fork and clone the repository.
2. Install dependencies:
   ```bash
   {{INSTALL_COMMAND}}
   ```
3. Copy the example environment file (if any):
   ```bash
   cp .env.example .env
   ```
4. Run the project locally:
   ```bash
   {{RUN_COMMAND}}
   ```

See [docs/getting-started.md](./docs/getting-started.md) for a deeper walkthrough.

## Making changes

### Branching

- Create a feature branch from `{{DEFAULT_BRANCH}}`: `git checkout -b feat/short-description`
- Keep branches focused — one feature or fix per branch.

### Commit messages

This project follows {{COMMIT_CONVENTION}}. Examples:

- `feat: add CSV export to the report endpoint`
- `fix: handle empty payload in webhook handler`
- `docs: clarify env-var defaults`

### Tests

Run the suite before pushing:

```bash
{{TEST_COMMAND}}
```

Add or update tests for any behavior change. PRs that drop coverage on changed lines will be asked to add tests.

### Code style

Run the linter / formatter:

```bash
{{LINT_COMMAND}}
```

The CI will run the same checks — fixing locally is faster than waiting for the pipeline.

## Submitting changes

1. Push your branch and open a Pull Request against `{{DEFAULT_BRANCH}}`.
2. Fill out the PR template (if one exists). Otherwise include:
   - **What** — a one-line summary.
   - **Why** — the user-facing problem or motivation.
   - **How** — short notes on the approach if non-obvious.
   - **Testing** — what you ran locally.
3. Address review comments by pushing follow-up commits (don't force-push unless asked).
4. A maintainer will merge once CI is green and at least one approval is in.

## Reporting issues

Use the issue templates in `.github/ISSUE_TEMPLATE/` when available. For bug reports, include:

- What you expected vs. what happened.
- Steps to reproduce.
- Environment (OS, runtime version, project version).
- Logs or stack traces (redact secrets).

For security-sensitive reports, email {{CONTACT_EMAIL}} instead of opening a public issue.

## License

By contributing you agree that your contributions will be licensed under the project's [LICENSE](./LICENSE).
```

### Placeholders

| Placeholder | Source |
| --- | --- |
| `{{PROJECT_NAME}}` | manifest `name` field |
| `{{INSTALL_COMMAND}}` | detected install step |
| `{{RUN_COMMAND}}` | detected run script |
| `{{TEST_COMMAND}}` | detected test script |
| `{{LINT_COMMAND}}` | detected lint script — drop the whole section if no linter is configured |
| `{{DEFAULT_BRANCH}}` | `git symbolic-ref refs/remotes/origin/HEAD` or fallback `main` |
| `{{COMMIT_CONVENTION}}` | "Conventional Commits" if `feat:`/`fix:` prefixes dominate recent history; otherwise "the existing commit style in `git log`" |
| `{{CONTACT_EMAIL}}` | `package.json` `author.email`, `pyproject.toml` `authors`, or `git config user.email` |

Trim any section whose data isn't available — don't leave placeholders unresolved.

---

## CODE_OF_CONDUCT.md

The skill uses the **Contributor Covenant 2.1** — the de-facto standard adopted by GitHub, GitLab, Kubernetes, and most major OSS projects.

**Do not author a Code of Conduct from scratch.** Instead, generate a short adoption file that points to the official text and supplies the project-specific contact email. This keeps the document accurate, legally consistent, and trivial to update if the upstream version changes.

### Template

```markdown
# Code of Conduct

This project has adopted the [Contributor Covenant](https://www.contributor-covenant.org/) Code of Conduct, version 2.1.

The full text is available at:

- **English:** https://www.contributor-covenant.org/version/2/1/code_of_conduct/
- **Other languages:** https://www.contributor-covenant.org/translations/

## Our pledge (summary)

We pledge to make participation in {{PROJECT_NAME}} a respectful and harassment-free experience for everyone, regardless of background or identity. We commit to acting and interacting in ways that foster an open, welcoming, diverse, and healthy community.

## Scope

This Code of Conduct applies within all project spaces — issues, pull requests, discussions, chat channels — and when an individual is officially representing the project in public.

## Enforcement

Concerns about behavior covered by this Code of Conduct can be reported to the maintainers at **{{CONTACT_EMAIL}}**. All reports will be reviewed and investigated promptly and fairly. The maintainers are responsible for clarifying and enforcing standards, and will take appropriate action in response to any behavior they deem inappropriate.

For the full enforcement guidelines, see the [Contributor Covenant 2.1 text](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).

## Attribution

This document is based on the [Contributor Covenant](https://www.contributor-covenant.org/), version 2.1, available at https://www.contributor-covenant.org/version/2/1/code_of_conduct.html.
```

### Placeholders

| Placeholder | Source |
| --- | --- |
| `{{PROJECT_NAME}}` | manifest `name` field |
| `{{CONTACT_EMAIL}}` | `package.json` `author.email`, `pyproject.toml` `authors`, or `git config user.email` — fall back to a generic "the maintainers (see [MAINTAINERS.md] or repository owners)" if no email is detectable |

### Localization

When `LANG != en`:

- The summary paragraph, "Scope", "Enforcement", and "Attribution" sections are translated.
- The link to the **translated** Contributor Covenant for that language is added at the top (the project hosts translations for pt-BR, es, and many others at `https://www.contributor-covenant.org/translations/`).
- The English link remains as a canonical reference.

---

## Generation order

When `--extras` is requested, generate in this order:

1. `CONTRIBUTING.md` (references `CODE_OF_CONDUCT.md` — needs to exist or be planned)
2. `CODE_OF_CONDUCT.md`

Each file follows the same overwrite checkpoint as `README.md`. If the user declines overwrite for either, skip that file and continue.

## Reporting back

In the final wrap-up message, list each extra produced and its path, alongside the rest of the generated artifacts.
