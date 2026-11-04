# Module 05: Config blocks: scheduled promo banner

**Scenario:** Marketing wants banners that switch on and off at set times, with no developer and no republish at midnight. Users can dismiss a banner, and once they do, it stays dismissed for that campaign only.

**Local content:** `/drafts/m05-promo`. It has one active banner and one expired banner.

## Task 1: Key-value block with `readBlockConfig`

**Why:** Key-value tables are the standard way for authors to configure a block.

- Read `message`, `cta`, `start`, `end`, `dismissible` and `id` with `readBlockConfig`. Inspect what it returns for `cta`: a string or a URL? Read the function in aem.js to see how it handles links, images and lists.
- **Note:** `message` may contain rich text, but `readBlockConfig` flattens it. Read that row's DOM directly if you need to keep formatting.

## Task 2: Scheduling logic

- Parse ISO dates with offsets, and also date-only values (`2026-03-01`). Decide which timezone a date-only value means and document the decision.
- If the banner is outside its window, remove its `.promo-banner-wrapper`. If that leaves the `.section` empty, remove the section too, so it leaves no empty padding.
- Put the date logic in `scripts/utils.js` as a pure function `isActive(now, start, end)`. You'll unit-test it in Module 17.

<details><summary>Real-world gotcha</summary>

Pages are cached at the CDN. The **HTML doesn't change** at midnight, but your JS runs on every page view, so client-side scheduling works. If the banner were in the first section, consider the CLS cost and how you would avoid a flash.
</details>

## Task 3: Dismissal

`dismissible: true` renders a close button. Store `promo-dismissed:<id>` in `localStorage` (wrapped in try/catch, because private mode can throw). A new campaign `id` shows again.

## Task 4: Placement without CLS

The banner is authored at the top of the page. Reserve space only when the banner will show: decide synchronously **before** the first paint of that section, or position it so it doesn't push content down.

## Done when

- [ ] Only the summer banner shows, and adding `?now=2026-03-15` shows only the urgent one. Build this debug override yourself, for QA.
- [ ] A dismissed banner stays hidden after reload, but changing `Id` brings it back
- [ ] CLS = 0 on load

## Author it for real

In a Doc, create a `Promo Banner` table with two columns (key | value), using the same keys as the draft. Ask a "content author" (you, on the other machine) to change `End` to yesterday and Preview. The banner disappears without a code change.
