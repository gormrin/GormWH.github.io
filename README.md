# GormWH.github.io

Source for my personal site at [https://GormWH.github.io](https://GormWH.github.io) — built with Astro 7 and Tailwind v4 (CSS-first), with the React integration available for islands.

## What's on the site

- A homepage (hero, about, recent work, recent writing, contact).
- `work` and `writing` collections, each with a listing page (tag filter) and per-entry pages.
- A contact page and a custom 404.
- Three languages under URL prefixes: `/en-us/`, `/ja-jp/`, `/ko-kr/`. English is the source; untranslated entries fall back to English with a notice. Old unprefixed URLs redirect to `/en-us/…`.

## Stack

- **Astro 7**, static output, deployed to **GitHub Pages**.
- **Tailwind v4** through `@tailwindcss/vite`. Every design token lives in `@theme { ... }` inside `src/styles/global.css`. There is no `tailwind.config.*`.
- **React 19** via `@astrojs/react` for islands.
- **TypeScript** via `astro/tsconfigs/strictest`.
- **Vitest** for unit and integration tests, **Playwright** for browser E2E.
- **pnpm** on **Node ≥ 22.12.0**. No linter or formatter.

## Project structure

```text
src/
├── components/        site-wide chrome and collection UI (Header, Footer, TagFilter, …)
├── content/           work/ and writing/ markdown, one subfolder per locale
├── data/              static data (contact channels)
├── features/home/     homepage sections (Hero, About, Work, Writing, Contact)
├── layouts/           BaseLayout, MarkdownLayout
├── lib/               build-time helpers (i18n, SEO, UI strings, ordering)
├── scripts/           browser scripts (tag filter, language menu)
├── pages/             [lang]/ routes, legacy redirect stubs, 404
└── styles/            global.css (tokens + gh-*/hp-* classes), theme.css, fonts.css
tests/                 unit, integration, e2e
public/                favicons, OG image, robots.txt
```

## Develop, build, test

| Command | What it does |
| --- | --- |
| `pnpm install` | Install dependencies |
| `pnpm dev` | Dev server at `localhost:4321` |
| `pnpm build` | Build the site to `./dist/` |
| `pnpm preview` | Serve `./dist/` locally |
| `pnpm check` | Type-check `.astro` and TypeScript |
| `pnpm test` | Vitest unit + integration |
| `pnpm test:e2e` | Rebuild, then run Playwright |

## Deployment

Every push to `main` builds and deploys through `.github/workflows/deploy.yml` (`withastro/action`). `astro.config.mjs` sets `site` to `https://GormWH.github.io` and leaves `base` unset, because the repo is served at the Pages root.

## Contributing

Conventions for contributors and AI agents start at [`AGENTS.md`](AGENTS.md).
