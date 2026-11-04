# Module 03: Accordion from scratch

**Scenario:** Customer support wants an FAQ page. Questions must be linkable from emails (`/help#do-you-ship-internationally`), and the page must pass an accessibility audit.

**Local content:** `/drafts/m03-accordion`. Row 4 uses plain-text cells with no `<p>`.

## Task 1: Create `blocks/accordion/accordion.js` + `.css`

**Why:** Native `<details>/<summary>` gives you keyboard support and accessibility semantics without any JavaScript.

- Each row becomes a `<details class="accordion-item">`. The first cell becomes the `<summary>`, and the second becomes `.accordion-item-body`.
- Move the existing nodes (`append`) instead of copying `innerHTML`. That keeps decorated buttons, icons and pictures intact.
- Give each item a stable `id` from `toClassName(questionText)`, and make sure ids stay unique if two questions match.

## Task 2: Deep linking

**Why:** Support agents paste links into tickets.

- On load, if `location.hash` matches an item, open it and scroll to it. Note that `scripts.js` already scrolls to the hash **after** `loadSections`. Consider why that works, and whether it would still work if the accordion were in a lazily loaded section.
- When a user opens an item, update the URL with `history.replaceState` without jumping the page.
- Handle `hashchange` too.

## Task 3: Exclusive mode variant

`Accordion (single)` allows only one open item at a time. Use the native `name` attribute on `<details>` and check browser support. Add a JS fallback only if you can justify it.

## Task 4: Motion

Animate open and close with CSS (`interpolate-size: allow-keywords` or the `::details-content` pseudo-element), and respect `prefers-reduced-motion`.

## Done when

- [ ] `localhost:3000/drafts/m03-accordion#is-there-a-warranty` opens and scrolls to row 4
- [ ] You can operate it fully by keyboard, and screen-reader names are the question text
- [ ] No layout shift when the block decorates (check the Performance panel for CLS)
- [ ] Two accordions on one page with the same question text produce no id collisions
