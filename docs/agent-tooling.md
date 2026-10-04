# Agent tooling (`.claude/`)

## Settings (`.claude/settings.json`)

The committed allowlist covers `pnpm install | dev | build | preview | check`, `astro check`, read-only `git status | diff | log | show | branch`, and the `astro-docs` MCP search. Anything that can run arbitrary code or destroy work (`pnpm exec`, `pnpm add`, `git push`, `git reset --hard`, broad `git *`) stays behind a prompt.

A PreToolUse hook (`.claude/hooks/block-build-artifacts.py`) refuses Edit / Write / MultiEdit when the path has a `dist` or `.astro` segment.

Personal extras go in `.claude/settings.local.json`, which is gitignored.

## Reviewer subagent (`astro-tailwind-reviewer`)

Read-only. Checks recent `.astro` and `src/styles/global.css` changes against [`styling.md`](styling.md) and [`architecture.md`](architecture.md): token discipline, no `tailwind.config.*`, no `--container-content`, `gh-*` / `hp-*` split, feature-folder placement, alias imports, strict TypeScript. CJK text in localized strings (e.g. `src/lib/ui.ts`) is expected and not flagged.

Runs automatically at the end of `new-section`, or manually with `@astro-tailwind-reviewer`.

## Skills (user-invocable only)

| Skill | Does |
| --- | --- |
| `/new-section <route> <SectionName>` | Writes `src/features/<route>/<SectionName>.astro` from the template, prints the import line to paste, then runs the reviewer |
| `/create-work-post` | Writes a new `src/content/work/en-us/<slug>.md` with validated frontmatter |
| `/create-writing-post` | Writes a new `src/content/writing/en-us/<slug>.md` with validated frontmatter |

## `CLAUDE.md`

`CLAUDE.md` is a symlink to `AGENTS.md`. Keep both at the repo root so relative links in `AGENTS.md` resolve for Claude Code too.
