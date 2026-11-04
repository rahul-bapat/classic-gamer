# Module 04: Tabs with full ARIA

**Scenario:** Product detail pages need Overview / Specifications / Reviews tabs. Last quarter's accessibility audit flagged the old site's tabs for having no keyboard support.

**Local content:** `/drafts/m04-tabs`. It has **two** tabs blocks on the same page.

## Task 1: Build `blocks/tabs`

Follow the [WAI-ARIA tabs pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/):
- A `role="tablist"` containing `button[role=tab]` elements with `aria-selected`, `aria-controls` and roving `tabindex`
- Panels marked `role="tabpanel"` with `aria-labelledby`, and inactive panels `hidden`
- IDs that are unique **per block instance**. Count blocks with a module-level counter or use `block.closest('.section')` index logic.

## Task 2: Keyboard

Support Left/Right (wrapping), Home and End. Use **automatic activation**, so the focused tab becomes selected, and explain the trade-off against manual activation in a code comment.

## Task 3: State in the URL

`?tab=specifications` opens that tab on load, for marketing emails. If there are multiple blocks, only the first block that has a matching label reacts.

## Task 4: Stretch: content from fragments

If a panel cell contains only a link to `/fragments/...`, fetch it with `loadFragment` **when the tab is first activated**, not on page load. This is lazy loading inside a block.

## Done when

- [ ] axe DevTools or the Lighthouse accessibility audit reports 0 issues on the page
- [ ] Both blocks work independently
- [ ] With JS disabled, all the content can still be read (consider whether that's possible with EDS and what that means for SEO)
