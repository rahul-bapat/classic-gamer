# Module 08: Fragments & widgets

**Scenario:** The same "Summer sale" promo appears on 200 pages, and it must change in one place. Separately, the stores team built a small store finder app that needs to live inside content pages.

**Local content:** `/drafts/m08-fragments`, `/drafts/fragments/summer-promo`

## Task 1: Fragment behavior

**Why:** Fragments are the main way to reuse content in EDS.

- Load the page. The `/drafts/fragments/...` link is auto-blocked into a fragment (see `buildAutoBlocks`). Confirm the fragment's `section-metadata` style (`highlight`) is applied.
- **Bug to find:** fragment loading in `buildAutoBlocks` is fire-and-forget, so `forEach(async …)` isn't awaited. When can this race with `decorateSections`? What happens if the fragment link is in the **first** section? Fix it so fragments in section 1 are loaded before LCP, or argue in a comment why that's a bad idea.
- Add protection against a fragment that includes itself (A → B → A).

## Task 2: Build a widget

**Why:** Widgets (`/widgets/<name>.html|css|js`, which live in **code**, not content) let developers ship small apps that authors place with a link. Read `blocks/widget/widget.js` first.

Create `widgets/store-finder.html`, `.css` and `.js`:
- The HTML is the static shell: a search input, a results list and a "use my location" button
- The JS default export receives the widget element and reads `region` and `limit` from `widget.dataset` (passed from the query string by `applyWidgetShell`)
- Data comes from `drafts/data/stores.json`, which you create: 6 stores with lat/lng/region
- "Use my location" sorts stores by distance (haversine). Handle a denied permission gracefully.

## Task 3: Widget vs block

Write three lines in `widgets/README.md` (ignored by `.hlxignore` through `*.md`) on when you'd choose a widget, a block or a fragment.

## Done when

- [ ] Changing the fragment draft updates the m08 page without touching m08
- [ ] The store finder renders with `region=west&limit=3` applied
- [ ] Self-referencing fragments don't hang the page

## Author it for real

Create `/fragments/summer-promo` as a Doc and Preview **and Publish** it. Fragments must be published too. Link to it from two pages.
