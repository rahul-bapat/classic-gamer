# EDS Hands-On Lab: Summit Outfitters

You're the lead developer moving **Summit Outfitters**, a fictional outdoor-gear retailer, onto AEM Edge Delivery Services. Each module gives you a business problem to solve in code. The explanations are short. You write the code.

> Assumes you've finished the Adobe EDS Developer Professional course. Concepts aren't re-taught. Each task tells you what to build and why, with hints folded away.

## Two-machine workflow

| Machine | What you do there |
|---|---|
| **This machine (code)** | Write code, run `aem up`, and test against **local mock content** in `drafts/`. Commit and push to GitHub. |
| **Other machine (content)** | Author the matching Google Docs/Sheets, then Preview and Publish with Sidekick. |

Every module ships with ready-made local content in `drafts/`, so you never have to wait for the other machine to start coding. Each module also includes an **"Author it for real"** section. When you get to the other machine, follow it to build the same content in Google Drive and validate on `*.aem.page`.

```
aem-eds-lab/
├── blocks/          your blocks (boilerplate + yours)
├── scripts/         scripts.js (yours), aem.js (vendored: never edit)
├── styles/
├── widgets/         you create this in Module 8
├── drafts/          LOCAL MOCK CONTENT (.plain.html + .json), git-ignored by the CDN via .hlxignore
└── tutorial/        this guide
```

## Daily loop

```bash
npm run dev          # = aem up --html-folder drafts  → http://localhost:3000/drafts/m01-anatomy
npm run lint         # run before every commit
```

- `http://localhost:3000/drafts/<page>` serves local mock content with your local code.
- `http://localhost:3000/<page>` serves content proxied from your real site (once Module 0 is done) with your local code.
- `http://localhost:3000/drafts/<page>.plain.html` is the raw authored markup. Always look at it before writing a block.

## Modules

| # | Module | Real-world problem | Level |
|---|---|---|---|
| 00 | [Setup & two-machine workflow](00-setup.md) | Getting your own site, repo, and branch previews wired up | Core |
| 01 | [Pipeline forensics](01-pipeline-forensics.md) | "Why does my block look different from what the author made?" | Core |
| 02 | [Defensive blocks & variants](02-defensive-cards.md) | Authors break your blocks. Survive it. | Core |
| 03 | [Accordion from scratch](03-accordion.md) | An FAQ that's accessible and deep-linkable | Core |
| 04 | [Tabs with full ARIA](04-tabs.md) | Product specs that pass an accessibility audit | Intermediate |
| 05 | [Config blocks: scheduled promos](05-scheduled-promo.md) | Marketing wants banners that switch on and off on their own | Intermediate |
| 06 | [Sections, templates & themes](06-templates.md) | An article template, with no extra work for authors | Intermediate |
| 07 | [Auto-blocking](07-auto-blocking.md) | Authors paste YouTube links and forget hero tables | Intermediate |
| 08 | [Fragments & widgets](08-fragments-widgets.md) | Reusable promos and an embeddable store-finder app | Intermediate |
| 09 | [Spreadsheet-driven blocks](09-spreadsheet-products.md) | A product grid run from a Google Sheet: filter, sort, paginate | Advanced |
| 10 | [Query index & listings](10-query-index.md) | A blog listing and "related articles" that maintain themselves | Advanced |
| 11 | [Header & navigation hardening](11-navigation.md) | Active states, a sticky header, mega-menu a11y | Advanced |
| 12 | [Localization & placeholders](12-localization.md) | Launching `/fr/` without copying code | Advanced |
| 13 | [Performance: keep it 100](13-performance.md) | A marketing page dropped to a Lighthouse score of 62. Fix it. | Advanced |
| 14 | [Consent, martech & CSP](14-consent-martech.md) | Adding analytics without killing performance or breaking GDPR | Advanced |
| 15 | [Forms](15-forms.md) | A contact form defined in a spreadsheet | Advanced |
| 16 | [SEO & structured data](16-seo.md) | Product rich results, canonicals, redirects, sitemaps | Advanced |
| 17 | [Quality: tests & CI](17-testing-ci.md) | Stopping regressions before they ship | Expert |
| 18 | [Operating the platform](18-operations.md) | Admin API, cache, CDN, Sidekick extensions | Expert |
| 19 | [Capstone: Summer Sale](19-capstone.md) | Ship a full campaign page end to end | Expert |

## Working rules

1. **Look at the markup first.** `curl localhost:3000/drafts/x.plain.html` before you write any decoration.
2. **One branch per module** (`git switch -c m03-accordion`). Push it and test on `https://m03-accordion--<repo>--<owner>.aem.page/`.
3. **No build step, no frameworks, no runtime dependencies.** Everything ships as plain ES modules.
4. **Never edit `scripts/aem.js`.** Put your helpers in `scripts/` (for example `scripts/utils.js`).
5. **Scope CSS to `.blockname`.** Lint must pass.
6. **Check the "Done when" list** before moving on. It's your acceptance criteria.

## References

- Developer docs: https://www.aem.live/developer/
- Block collection (reference implementations): https://www.aem.live/developer/block-collection
- Keeping it 100: https://www.aem.live/developer/keeping-it-100
- Admin API: https://www.aem.live/docs/admin.html
