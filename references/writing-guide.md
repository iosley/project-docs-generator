# Writing Guide

Voice, tone, and depth guidelines for the docs and README produced by this skill.

## Voice

- **Concise and confident.** Write for a developer who is in a hurry but capable. No throat-clearing, no marketing fluff.
- **Specific over general.** "The server binds `PORT` (default `3000`) on startup" beats "the service runs on a configurable port."
- **Active voice, present tense.** "The worker processes the queue" — not "The queue is processed by the worker."
- **Second person for instructions** ("Run `npm install`"), third person for descriptions ("The handler validates the payload").

## Tone

- **Developer-friendly, not salesy.** No "blazing fast" / "next-generation" / "powerful platform."
- **Honest about limitations.** If there's no auth middleware, say so. If a feature is partial, say so.
- **Slight personality is fine in the README** (emojis on section headers, a closing "Built with ❤️" line); the `docs/` content stays sober.

## Depth

**Default: practical overview.**
- Explain what each module *does* and where its logic *lives*.
- Show the high-level flow as a numbered list.
- Link to source files instead of inlining their contents.
- Do **not** catalog every helper function unless the user explicitly asks for deep reference.

Go deeper only when the topic warrants it — non-obvious environment-variable behaviors, multi-database setups, custom protocols, framework workarounds.

## Markdown patterns

**Tables for parallel data** — env vars, endpoints, cron schedules, dependencies. Always include a header row.

**Code blocks with language tags** — `bash` for shell, `json` for payloads, `env` for env files, plus the project's primary language tag.

**Relative links** — never absolute paths. Always linkable from the file the reader is in.

**Inline code for** identifiers, file names, env vars, commands.

## Anti-patterns to avoid

- Restating the section title in the first sentence. Just start with the content.
- "This document describes..." / "In this section we will..." — cut it.
- Listing features the code doesn't have.
- Inventing example endpoints / commands / env vars. Pull them from the code.
- Burying the actionable step under three paragraphs of context.
- Walls of text — break into short paragraphs, lists, or tables.
- Emoji spam (one per section header is plenty; none inside paragraphs).
- "TODO" / "TBD" / placeholders in a final doc. Either fill them or remove them.

## Length budgets (soft guidelines)

| Doc | Target length |
| --- | --- |
| Root `README.md` | ~150–250 lines |
| `docs/README.md` | ~30–60 lines |
| `overview.md` | ~80–150 lines |
| `getting-started.md` | ~80–150 lines |
| `deployment.md` | ~80–150 lines |
| `architecture/*.md` | ~80–200 lines each |
| `domains/*.md` | ~80–150 lines each |
| `reference/*.md` | as long as needed (often tabular) |

Over budget? Likely doing too much in one file — split.
