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
| Branch model | `git branch -a`, CI triggers, recent branch names, presence of `develop` branch | "Branching" |
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

{{BRANCHING_BLOCK}}

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

1. Push your branch and open a Pull Request against `{{PR_TARGET_BRANCH}}`.
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
| `{{PR_TARGET_BRANCH}}` | derived from branching strategy detection — see "Branching strategy detection" below |
| `{{BRANCHING_BLOCK}}` | rendered branching guidance — see "Branching strategy detection" below |
| `{{COMMIT_CONVENTION}}` | "Conventional Commits" if `feat:`/`fix:` prefixes dominate recent history; otherwise "the existing commit style in `git log`" |
| `{{CONTACT_EMAIL}}` | `package.json` `author.email`, `pyproject.toml` `authors`, or `git config user.email` |

Trim any section whose data isn't available — don't leave placeholders unresolved.

### Branching strategy detection

Pick the branch model from real repo signals — **do not assume Git Flow blindly**. The skill follows whatever pattern the project already uses; Git Flow is only the default when the project has no established pattern of its own.

**Detection order (first match wins):**

1. **Project documents its own model** — if `CONTRIBUTING.md` (existing), `docs/`, repo wiki, or README already describe a branching workflow, mirror it verbatim. Stop here.
2. **Trunk-based** — only `{{DEFAULT_BRANCH}}` exists on the remote (no `develop`, no long-lived release branches), and recent merged PRs target `{{DEFAULT_BRANCH}}` directly. Use the **Trunk-based block**.
3. **GitHub Flow** — `{{DEFAULT_BRANCH}}` plus short-lived topic branches, no `develop`, no `release/*`. Use the **GitHub Flow block** (it is a labeled variant of trunk-based with a stronger PR emphasis).
4. **GitLab Flow** — environment branches like `staging`, `pre-production`, `production` alongside `{{DEFAULT_BRANCH}}`. Use the **GitLab Flow block**.
5. **Git Flow** — a `develop` branch exists on origin, or `feature/*`, `release/*`, `hotfix/*` prefixes appear in `git branch -a` / recent history. Use the **Git Flow block**.
6. **No clear signal** — fall back to the **Git Flow block** as the documented default for a new contributor workflow, but explicitly note in the wrap-up that no convention was detected so the maintainer can override.

Commands to run for detection:

```bash
git symbolic-ref refs/remotes/origin/HEAD          # default branch
git branch -a                                       # all branches (local + remote)
git for-each-ref --format='%(refname:short)' refs/remotes/origin/ | head -40
git log --all --oneline -50                         # recent activity / merge patterns
```

**`{{PR_TARGET_BRANCH}}`** is the branch contributors open PRs against:

- Git Flow → `develop`
- Trunk-based / GitHub Flow / GitLab Flow / unknown → `{{DEFAULT_BRANCH}}`

#### Branching blocks

Render the block that matches the detected strategy into `{{BRANCHING_BLOCK}}`. Substitute `{{DEFAULT_BRANCH}}` inside the block too.

**Git Flow block** (default when no convention is detected):

```markdown
This project follows the **Git Flow** branching model.

- `{{DEFAULT_BRANCH}}` — always reflects production-ready state. Only release and hotfix merges land here.
- `develop` — integration branch for the next release. All feature work merges here first.
- `feature/<short-description>` — new features, branched from `develop`, merged back into `develop`.
- `release/<version>` — release stabilization, branched from `develop`, merged into both `{{DEFAULT_BRANCH}}` and `develop`.
- `hotfix/<short-description>` — urgent production fixes, branched from `{{DEFAULT_BRANCH}}`, merged into both `{{DEFAULT_BRANCH}}` and `develop`.
- `bugfix/<short-description>` — non-urgent fixes targeting the current release, branched from `develop`.

Typical contributor flow:

```bash
git checkout develop
git pull
git checkout -b feature/short-description
# ... commit work ...
git push -u origin feature/short-description
# open a Pull Request against `develop`
```

Keep branches focused — one feature or fix per branch.
```

**Trunk-based block**:

```markdown
This project uses a **trunk-based** workflow — all work targets `{{DEFAULT_BRANCH}}` through short-lived branches.

- Create a topic branch from `{{DEFAULT_BRANCH}}`: `git checkout -b feat/short-description`
- Keep branches short-lived (typically < 1–2 days) and focused — one feature or fix per branch.
- Rebase on `{{DEFAULT_BRANCH}}` regularly to minimize merge conflicts.
- Open a Pull Request against `{{DEFAULT_BRANCH}}`.
```

**GitHub Flow block**:

```markdown
This project follows **GitHub Flow** — `{{DEFAULT_BRANCH}}` is always deployable, and all work happens on topic branches.

- Branch from `{{DEFAULT_BRANCH}}`: `git checkout -b feat/short-description`
- Push early and open a Pull Request against `{{DEFAULT_BRANCH}}` — even before the work is finished — so review can happen incrementally.
- Keep branches focused and short-lived.
- Merge to `{{DEFAULT_BRANCH}}` only after review approval and green CI.
```

**GitLab Flow block**:

```markdown
This project uses a **GitLab Flow** variant with environment branches.

- `{{DEFAULT_BRANCH}}` — main integration branch. Topic branches merge here first.
- Environment branches (e.g. `staging`, `production`) track what is deployed where; promotion is done by merging `{{DEFAULT_BRANCH}}` into the next environment branch.
- Create a topic branch from `{{DEFAULT_BRANCH}}`: `git checkout -b feat/short-description`
- Open a Pull Request against `{{DEFAULT_BRANCH}}`. Releases are driven by promoting commits to the environment branches, not by merging feature branches directly into them.
```

If the project's own documentation describes the model (detection step 1), reproduce that description instead of using these blocks.

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
