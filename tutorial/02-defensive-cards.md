# Module 02: Defensive blocks & variants

**Scenario:** The boilerplate `cards` block assumes perfect authoring. Real authors leave out images, add stray cells, and forget CTAs. Merchandising also wants a **horizontal** variant and fully clickable cards.

**Local content:** `/drafts/m02-cards`. It contains five intentionally broken rows plus a `cards (horizontal)` block.

## Task 1: Find the breakage

Open the page and list what goes wrong for each of the five rows. Check `.plain.html` to see the exact structure of each one.

## Task 2: Rewrite `blocks/cards/cards.js` defensively

**Why:** A block should never throw an error or render empty boxes because of how something was authored.

Rules to implement:
- Identify cells **by content, not position**. The image cell is the one containing a `<picture>`, and the body is everything else.
- Drop empty cells, and merge extra text cells into the body.
- If a card has neither an image nor any text, skip it and log a `console.warn` that includes the row index, so authors can find it.
- If a card's body has exactly one link, make **the whole card clickable** with the stretched-link pattern. Keep a single `<a>` for accessibility, and don't wrap block-level content in an `<a>`.
- Keep `createOptimizedPicture` and pass sensible breakpoints.

<details><summary>Hint: stretched link</summary>

`li { position: relative }` and `.cards-card-body a::after { content: ''; position: absolute; inset: 0; }`. Leave the visible text as the link's accessible name.
</details>

## Task 3: Horizontal variant

**Why:** Variants let authors pick a layout without needing a new block. `Cards (horizontal)` in a Doc becomes `class="cards horizontal"`.

- In CSS only: `.cards.horizontal` places the image on the left (40%) and the body on the right, and stacks them below 600px.
- In JS, handle any variant-specific behavior with `block.classList.contains('horizontal')`. For example, use smaller image breakpoints, because the image is never wider than 40%.

## Task 4: Variant naming hygiene

Try `Cards (Horizontal, Dark)` in your head: what class names does it produce? (Read `toClassName` in aem.js.) Add a `dark` variant using CSS custom properties only.

## Done when

- [ ] No console errors. Every malformed row either renders sensibly or is skipped with a warning.
- [ ] Clicking anywhere on a card with one link navigates, and keyboard tab order has one stop per card
- [ ] The horizontal variant is responsive, and lint passes (`npm run lint`)
- [ ] Hero/LCP behavior is unchanged (cards are never in section 1 here)

## Author it for real

Make a `Cards (horizontal)` table in a Doc with three rows, deliberately leaving out one image. Preview it on your branch URL.
