# Routing & deployment

## Routing

Manual `[lang]` param routing with no Astro `i18n` config block. Locale is resolved from `Astro.params.lang` against a central table in `src/lib/i18n.ts`.

### Localized routes

File-based routes under `src/pages/[lang]/`, parametrized by locale path. Locale paths are `en-us` (English, default), `ja-jp` (Japanese), `ko-kr` (Korean).

**Dynamic routes (getStaticPaths per locale × slug):**
- `[lang]/index.astro` — homepage
- `[lang]/contact.astro` — contact page
- `[lang]/writing/index.astro` — writing listing
- `[lang]/writing/[slug].astro` — writing detail
- `[lang]/work/index.astro` — work listing
- `[lang]/work/[slug].astro` — work detail

Every locale gets every slug. Untranslated entries render the English source with canonical, hreflang and sitemap handling described in [`i18n.md`](i18n.md#fallback--canonical-policy).

### Legacy redirects (static meta-refresh stubs)

11 enumerated legacy URLs (unprefixed) route to their `/en-us/…` equivalents via status-less meta-refresh (GitHub Pages cannot 301/302). Each stub:
- Carries a `<link rel="canonical">` to the `/en-us/…` target
- Includes a noscript fallback link
- Is excluded from the sitemap

**Legacy stub routes:**
- `index.astro` → `/en-us/`
- `contact.astro` → `/en-us/contact/`
- `work/index.astro` → `/en-us/work/`
- `work/[slug].astro` (4 slugs via getStaticPaths) → `/en-us/work/{slug}/`
- `writing/index.astro` → `/en-us/writing/`
- `writing/[slug].astro` (3 slugs via getStaticPaths) → `/en-us/writing/{slug}/`

### Top-level 404

`src/pages/404.astro` (unprefixed) — rendered on unknown routes. English copy + links to `/en-us/`, `/ja-jp/`, `/ko-kr/`.

### Static-host constraints

- **No server redirects:** GitHub Pages is fully static; 301/302 responses are unavailable. All routing decisions resolve to build-time HTML.
- **Meta-refresh as strongest redirect:** Legacy stubs use meta-refresh with canonical tags as the single coherent SEO signal.
- **No Astro i18n config block:** Astro's built-in i18n option is NOT used. It offers no per-page canonical hook, expects physical locale folders, and adds automatic hreflang that conflicts with fallback-page canonicals. Locale resolution is entirely manual via `Astro.params.lang` ↔ central table.

## Deployment

- Target: GitHub Pages at `https://GormWH.github.io` (user/org root, no project subpath).
- `astro.config.mjs` sets `site: 'https://GormWH.github.io'` (used for canonical URLs and the sitemap).
- **No `base` is set**, so the deploy expects the user/organization Pages root, not a project subpath.
- Deploy gate: `pnpm build` (see [`commands.md`](commands.md)).
- `.github/workflows/deploy.yml` builds with `withastro/action` (Node 24, pnpm) and publishes with `actions/deploy-pages` on every push to `main`, or manually via `workflow_dispatch`. Tests are not part of the workflow.
