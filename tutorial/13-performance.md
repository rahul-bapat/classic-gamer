# Module 13: Performance: keep it 100

**Scenario:** After the campaign launch, the summer landing page dropped to a mobile Lighthouse score of 62. The CMO wants it back to 100 before Monday. EDS gives you a fast baseline, and it's easy to lose it.

**Local content:** `/drafts/m13-perf`, plus every block you've built so far

## Task 1: Baseline

Run Lighthouse (mobile) on your **branch preview URL**, not localhost, because localhost has no CDN or compression. You can also use PageSpeed Insights. Record LCP, TBT, CLS, total bytes before LCP, and the request waterfall.

## Task 2: Sabotage, then fix

Make each of these regressions on purpose, measure it, then fix it. This is how you learn to recognize them:

| Sabotage | What to observe | Proper fix |
|---|---|---|
| Add `<script src="https://www.googletagmanager.com/gtm.js?id=GTM-XXXX">` to `head.html` | TBT, blocked LCP | Load it in `loadDelayed` behind consent (Module 14) |
| Import a 200 KB library at the top of `scripts.js` | Bytes before LCP | `import()` it inside the block that needs it |
| Make the hero image `loading=lazy` in a custom hero | LCP time | First image is eager, with `fetchpriority=high` |
| Load `fonts.css` eagerly on mobile | LCP, FOUT | Keep the boilerplate's fallback fonts and `size-adjust` |
| Put a product-list block in section 1 | LCP waits for JSON | Move it to section 2, or reserve space and stream |

## Task 3: Budget guard

Write a small Node script, `tools/perf-budget.mjs` (add `tools/*` to `.hlxignore`). It fetches a URL, finds every script and stylesheet loaded **before** the first section is decorated (head + eager), and fails if the total exceeds **100 KB** compressed. That's the EDS guideline for the critical path.

## Task 4: Real User Monitoring

`sampleRUM` is built in. Add custom checkpoints:
- `sampleRUM('promo-dismiss', { source: id })` when a banner is dismissed
- A `sampleRUM('error', …)` call from a global `window.onerror`/`unhandledrejection` handler in `loadDelayed`

Learn how to open the **RUM Explorer** / OpTel dashboard for your domain and what `?rum=on` does. Check the current docs for how to force sampling.

## Done when

- [ ] Mobile Lighthouse is 100/100/100/100 on `m13-perf` and the capstone page (on aem.page)
- [ ] The perf budget script runs in CI (Module 17)
- [ ] You can explain the three-phase loading model and where every one of your blocks loads
