# Architecture

## Stack

- **Astro 7**, static output. Pages and sections are `.astro`; the React integration (`@astrojs/react`) is registered for islands.
- **Tailwind v4** via `@tailwindcss/vite` in `astro.config.mjs`. No `tailwind.config.*`; tokens live in `src/styles/global.css`. See [`styling.md`](styling.md).
- **TypeScript** via `astro/tsconfigs/strictest`.
- Fonts are self-hosted through `@fontsource*` packages, wired in `src/styles/fonts.css`.

## Path aliases (`tsconfig.json`)

| Alias | Resolves to | Holds |
| --- | --- | --- |
| `@components/*` | `src/components/*` | Site-wide chrome and shared collection UI (Header, Footer, TagFilter, ArticleHeader, …) |
| `@layouts/*` | `src/layouts/*` | `BaseLayout.astro` (head, SEO, Header/Footer) and `MarkdownLayout.astro` (article shell) |
| `@features/*` | `src/features/*` | Route-scoped sections, e.g. `features/home/{Hero,About,Work,Writing,Contact}.astro` |
| `@lib/*` | `src/lib/*` | Build-time TypeScript: i18n, SEO, UI strings, entry ordering, tag normalization |
| `@scripts/*` | `src/scripts/*` | Browser-side scripts |

## Feature-folder convention

A new page section goes in `src/features/<route>/<Section>.astro`, not `src/components/`. `src/components/` is for pieces used across routes. The `new-section` skill scaffolds this (see [`agent-tooling.md`](agent-tooling.md)).

## `lib/` vs `scripts/`

- `src/lib/` runs at build time (in frontmatter, `getStaticPaths`, or `astro.config.mjs`) and is covered by Vitest. `contentManifest.mjs` is plain JS on purpose, because `astro.config.mjs` loads it before `getCollection` exists.
- `src/scripts/` runs in the browser. A component pulls a script in with `<script>import "@scripts/<name>.ts";</script>`, and Vite bundles it.

## `design-system/`

A local-only brand reference (JSX UI kits and original tokens) that the homepage sections were ported from. It is gitignored, not part of the build, and absent from fresh clones. `src/styles/global.css` is the source of truth for what ships.
