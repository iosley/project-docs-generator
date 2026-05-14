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

**Critical rule: the skill must never write the body text of any Code of Conduct.** Some standard CoC language (necessary to describe what is being prohibited) is flagged by AI content filters and will block skill execution mid-run. To avoid this entirely, the generated `CODE_OF_CONDUCT.md` is a **pointer file** — it links to the official Contributor Covenant page hosted by `contributor-covenant.org` and provides a project-specific contact line. Nothing more.

This is also the most maintainable form: the upstream text is the canonical source and updates without touching the repo.

### Template (use exactly this — do not expand it)

```markdown
# Code of Conduct

This project follows the **Contributor Covenant**, version 2.1.

- Read the full text: https://www.contributor-covenant.org/version/2/1/code_of_conduct/
- Translations: https://www.contributor-covenant.org/translations/

## Contact

To report a concern related to this Code of Conduct, contact the maintainers at **{{CONTACT_EMAIL}}**.

## Attribution

Adapted from the [Contributor Covenant](https://www.contributor-covenant.org/), version 2.1.
```

### What NOT to add

When generating this file, do **not**:

- Inline a "pledge" / "standards" / "scope" / "enforcement guidelines" / "examples of behavior" section. The upstream URL covers all of that.
- Summarize the CoC in your own words. Summaries reintroduce the exact phrases that trigger filters.
- Expand the file beyond the template above. Length is a feature here, not a limitation.

### Placeholders

| Placeholder | Source |
| --- | --- |
| `{{CONTACT_EMAIL}}` | `package.json` `author.email`, `pyproject.toml` `authors`, or `git config user.email` — fall back to "the maintainers (see repository owners)" if no email is detectable |

### Localization

When `LANG != en`:

- Translate **only** the headings ("Code of Conduct" → "Código de Conducta" / "Código de Conduta") and the "Contact" / "Attribution" labels and surrounding short phrases.
- Add the **translated** Contributor Covenant link for that language as the first bullet (translations are hosted at `https://www.contributor-covenant.org/translations/`); keep the English link as a canonical fallback.
- Do **not** translate-and-inline any prohibited-behavior text. The pointer-file principle holds in every language.

---

## Generation order

When `--extras` is requested, generate in this order:

1. `CONTRIBUTING.md` (references `CODE_OF_CONDUCT.md` — needs to exist or be planned)
2. `CODE_OF_CONDUCT.md`

Each file follows the same overwrite checkpoint as `README.md`. If the user declines overwrite for either, skip that file and continue.

## Reporting back

In the final wrap-up message, list each extra produced and its path, alongside the rest of the generated artifacts.
