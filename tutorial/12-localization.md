# Module 12: Localization & placeholders

**Scenario:** Summit Outfitters is launching in France. Content lives under `/fr/`, and UI strings in blocks ("Add to cart", "{count} products") must be translated by authors without a code release.

**Local content:** `/drafts/m12-i18n`, `/drafts/fr/m12-i18n`, `/drafts/placeholders.json`, `/drafts/fr/placeholders.json`

## Task 1: Locale detection

In `scripts/utils.js`, write `getLocale()`. It uses the `locale` metadata if present, otherwise the first path segment (allowlist `['fr','de']`), otherwise `en`. Set `document.documentElement.lang` from it. `scripts.js` currently hard-codes `'en'`.

## Task 2: Placeholders

The boilerplate no longer ships `fetchPlaceholders`, so write your own:
- `fetchPlaceholders(locale)` loads `/<prefix>/placeholders.json`, converts it to `{ key: text }`, and caches the **promise**
- **Fallback chain:** if a French key is missing (`readMore` is missing in the FR draft), use the English value, and log a warning in development only
- `t(key, vars)` interpolates `{count}`

Use it in `product-list` for "Add to cart", "Out of stock" and the results count.

## Task 3: Locale-aware formatting

Prices use `Intl.NumberFormat(locale, { style: 'currency', currency })`. Dates in `article-list` use the locale too, so the same block renders `£349.00` on the English page and `349,00 £` on the French one.

## Task 4: Locale-aware nav and footer

`/fr/*` pages should load `/fr/nav` and `/fr/footer` unless metadata overrides it. Implement this without editing aem.js. Consider where `loadHeader` reads the nav path.

## Done when

- [ ] The EN and FR pages show translated UI strings, with the EN fallback for missing keys
- [ ] `<html lang>` is correct on both
- [ ] Only one placeholders request per page, even with two blocks using `t()`

## Author it for real

Create a `placeholders` Sheet at the root and in `/fr/`, with `Key | Text` columns. **Advanced:** read about the multi-site and multi-language patterns on aem.live (folder-mapped locales and the translation workflow), and hreflang through metadata (Module 16).
