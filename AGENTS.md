# AGENTS.md

Single source of truth **index** for contributors and AI agents. Claude Code reaches this file via `CLAUDE.md`, which is a symlink to this file.

Detailed information lives in [`docs/`](docs/). This file stays short on purpose: scan the table, jump to the file you need.

Root `README.md` is visitor-facing; `AGENTS.md` is the contributor index. Subjects covered in both must agree, but tone may differ.

## Project at a glance

Astro 7 static site for a personal portfolio, deployed to GitHub Pages at `https://GormWH.github.io` (user/org root, no project subpath).

- Package manager: **pnpm** (Node `>=22.12.0`).
- Deploy gate: `pnpm build`. Run `pnpm check` before pushing.
- No linter, no formatter. Vitest (unit + integration) and Playwright (E2E) suites exist.

## Where to look

| Topic | File |
| --- | --- |
| Commands & gates | [`docs/commands.md`](docs/commands.md) |
| Stack, path aliases, feature folders, `lib/` vs `scripts/` | [`docs/architecture.md`](docs/architecture.md) |
| Tailwind v4 tokens, `gh-*` / `hp-*` conventions | [`docs/styling.md`](docs/styling.md) |
| Routes, legacy redirects, deploy | [`docs/routing.md`](docs/routing.md) |
| Internationalization (locales, fallback, hreflang) | [`docs/i18n.md`](docs/i18n.md) |
| Content collections & MarkdownLayout | [`docs/content-pipeline.md`](docs/content-pipeline.md) |
| Testing (Vitest, Playwright) | [`docs/testing.md`](docs/testing.md) |
| Copy voice, glyph allowlist, languages | [`docs/brand-voice.md`](docs/brand-voice.md) |
| Commit message rules (no AI byline) | [`docs/commit-style.md`](docs/commit-style.md) |
| `.claude/` settings, reviewer subagent, skills | [`docs/agent-tooling.md`](docs/agent-tooling.md) |

Each subject has one owning file in `docs/`; edit that file, not a copy. The hard rules below are the one deliberate duplicate, so update both places when one changes.

## Hard rules that always apply (do not move into `docs/`)

These are short enough to live here and important enough to be unmissable:

- **No emoji** in copy. Allowed glyphs: `→ · — ※ ✻`. ([`docs/brand-voice.md`](docs/brand-voice.md))
- **No `tailwind.config.*`.** Tokens live in `@theme { ... }` inside `src/styles/global.css`. ([`docs/styling.md`](docs/styling.md))
- **No `--container-content`** — collides with the `max-content` CSS keyword. ([`docs/styling.md`](docs/styling.md))
- **No AI / tooling attribution in commits.** No `Co-Authored-By: Claude …`, no `.omc/plans/…` references. ([`docs/commit-style.md`](docs/commit-style.md))
- **Sections go in `src/features/<route>/`**, not `src/components/`. ([`docs/architecture.md`](docs/architecture.md))
