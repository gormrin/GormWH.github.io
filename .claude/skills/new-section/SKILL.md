---
name: new-section
description: Scaffold a new feature-folder section at src/features/<route>/<SectionName>.astro using the project's standard template. User-invocable only.
disable-model-invocation: true
---

# new-section

Scaffolds a new page section that follows the feature-folder convention described in `docs/architecture.md`.

## Usage

```
/new-section <route> <SectionName>
```

Examples:

```
/new-section work CaseStudies
/new-section about Timeline
/new-section writing Recent
```

## Steps

When invoked, perform these steps in order — do not skip the reviewer step.

### 1. Validate args

- `<route>` must be a single lowercase identifier (letters, digits, hyphens). Reject `..`, slashes, or empty.
- `<SectionName>` must be a `PascalCase` identifier valid as both an Astro component name and a JS import binding.
- If either is invalid, stop and tell the user the exact rule that failed.

### 2. Compute derived values

- `slug` = kebab-case of `<SectionName>` (e.g. `CaseStudies` → `case-studies`).
- `id` = `<route>-<slug>` (used as the section's DOM id).
- `key` = camelCase of `<SectionName>` (e.g. `CaseStudies` → `caseStudies`).
- `uiKey` = `<route>.<key>` (e.g. `home.caseStudies`), the `src/lib/ui.ts` namespace for the section's copy.
- `number` = the next 2-digit ordinal for sections in this route. Section labels live in `src/lib/ui.ts`, not in the `.astro` files: read the `en` dictionary's `<route>` block, take the highest `eyebrow: 'NN — …'`, add one. If the block has no eyebrows, start at `01`. If the user wants the section somewhere other than last, say which existing eyebrows would need renumbering; do not renumber them yourself.

### 3. Read the template

Read `.claude/skills/new-section/template.astro`. Substitute:

| Placeholder | Replace with |
| --- | --- |
| `__SECTION_ID__` | the computed `id` |
| `__UI_KEY__` | the computed `uiKey` |
| `__SECTION_NAME__` | the original `SectionName` (used in the header comment) |

### 4. Write the new file

Target path: `src/features/<route>/<SectionName>.astro`. If the file already exists, stop and tell the user — do not overwrite.

If `src/features/<route>/` does not yet exist, create it.

### 5. Print the wire-up snippets

The skill does **not** edit `src/lib/ui.ts` or the page. `UiKey` is typed from `UiDict`, so `pnpm check` fails until the strings below are added. Print the exact lines for the user to paste:

```
1. src/lib/ui.ts

   In the UiDict interface, inside <route>:
     <key>: { eyebrow: string; heading: string };

   In each of the en, ja and ko dictionaries, inside <route>:
     <key>: { eyebrow: '<number> — <SectionName>', heading: 'Heading TBD' },

   (ja / ko copy is hand-written; leave the English placeholder if no translation exists yet.)

2. src/pages/[lang]/<route>.astro (src/pages/[lang]/index.astro for home)

   Frontmatter:
     import <SectionName> from "@features/<route>/<SectionName>.astro";

   Body (inside <BaseLayout>, in the desired position):
     <<SectionName> locale={lang} t={t} />
```

Include the actual computed values in the printed output, not the placeholders.

### 6. Invoke the reviewer

Mention `@astro-tailwind-reviewer` so the reviewer subagent runs against the new file and any other recent `.astro` / `src/styles/global.css` edits. This is a required step, not optional.

## What this skill does NOT do

- It does not edit `src/lib/ui.ts` or `src/pages/[lang]/…` (so the user reviews the copy and wire-up before it renders).
- It does not modify `src/styles/global.css` or invent new `gh-*` classes.
- It does not run `pnpm dev` or `pnpm check` — let the user run those.
