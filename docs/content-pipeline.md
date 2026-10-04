# Content pipeline

Two collections — `work` and `writing` — defined in `src/content.config.ts` via the `glob` loader. Content is organized by locale subdirs. Entries are rendered through `src/layouts/MarkdownLayout.astro`.

## Entry organization

**Directory structure:**
```
src/content/
  work/
    en-us/
      slug-a.md
      slug-b.md
    ja-jp/
      slug-a.md         # translation of en-us/slug-a
    ko-kr/
  writing/
    en-us/
      slug-x.md
      slug-y.md
    ja-jp/
    ko-kr/
```

**Entry ID shape:** `entry.id = '<localeDir>/<slug>'`
- `en-us/slug-a`
- `ja-jp/slug-a` (same slug, different locale)
- `en-us/slug-x`

**Slug is the join key:** A translation is recognized by matching ASCII slugs across locale subdirs. No `translationKey` field is used; a file named `slug-a.md` in `ja-jp/` is a translation of the English `en-us/slug-a.md` with zero config.

**Schema:** Both collections enforce ISO-8601 dates via the Astro content config schema.

## Rendering

Dynamic routes `src/pages/[lang]/work/[slug].astro` and `src/pages/[lang]/writing/[slug].astro` mount entries through `MarkdownLayout.astro`.

When a localized entry exists it renders as a real translation; otherwise the English source renders as a fallback. Canonical, hreflang and sitemap rules for both cases are in [`i18n.md`](i18n.md#fallback--canonical-policy).

Listing pages (`src/pages/[lang]/{work,writing}/index.astro`) show, per slug, the entry in that locale if one exists, else the English one.

## ID parsing utilities

The `src/lib/i18n.ts` module exports:
- `getLocaleFromId(id: string)` — extracts locale dir from `id` (e.g., `'ja-jp'` from `'ja-jp/slug'`)
- `getSlugFromId(id: string)` — extracts slug from `id` (e.g., `'slug'` from `'ja-jp/slug'`)

**Invariant (unit-tested):** URLs and route params always use `getSlugFromId`, never raw `entry.id` — this prevents double-locale segments like `/en-us/writing/en-us/slug`.

## Future: localized slugs + translationKey

Currently deferred. If CJK-slug SEO becomes a priority (e.g., a Japanese article needs a Japanese slug for search visibility), the scheme can evolve to:
- Add an optional `translationKey` field (shared across locales)
- Allow slugs to differ per locale (e.g., `en-us/algorithm-introduction.md` ↔ `ja-jp/アルゴリズム入門.md`)
- Keep shared ASCII slug as fallback join key for backwards compatibility

This change is non-disruptive — the schema and routes already support it.
