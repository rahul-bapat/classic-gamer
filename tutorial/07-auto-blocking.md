# Module 07: Auto-blocking

**Scenario:** Authors keep forgetting the Hero table and just type an H1 above an image. They also paste raw YouTube URLs. You'll turn both patterns into blocks automatically, without hurting performance.

**Local content:** `/drafts/m07-autoblock`, `/drafts/m13-perf`

## Task 1: Auto hero

**Why:** Auto-blocking turns authoring conventions into structure.

In `buildAutoBlocks` (`scripts/scripts.js`), if the first section starts with an `<h1>` and a `<picture>`, and no hero block was authored, wrap them in a `hero` block with `buildBlock('hero', { elems: [picture, h1] })` and put it in its own section at the top.

Edge cases you must handle:
- The picture comes **before** the H1 (both orders are common)
- The picture sits in a `<p>`. Move the `<picture>` and remove the empty `<p>`.
- Opt-out: metadata `hero: off` disables it (for landing pages)

## Task 2: YouTube auto-block with a facade

**Why:** A real YouTube iframe costs about 1 MB of JavaScript. A facade loads only a thumbnail until the user clicks.

- A paragraph whose only content is a YouTube link (`youtube.com/watch?v=`, `youtu.be/`) becomes an `embed` block. Inline links stay links. Reuse the "only child" logic from `buildWidgetAutoBlocks`.
- Create `blocks/embed`: render a `<button>` with the thumbnail `https://i.ytimg.com/vi/<id>/hqdefault.jpg` and a play icon. On click, swap in a `youtube-nocookie.com` iframe with `autoplay=1`.
- Check the `frame-src` in the CSP in `head.html`. Does it allow `https:`?
- Add an `IntersectionObserver` that preconnects to YouTube domains only when the block is near the viewport.

## Task 3: Ordering bug hunt

`buildAutoBlocks` runs **before** `decorateSections`/`decorateBlocks`. Add a `console.log` to show that your hero exists in the DOM before `decorateBlocks`. Then explain what would break if you built the hero inside the hero block's own `decorate()` instead.

## Done when

- [ ] `/drafts/m07-autoblock` shows a hero, and the hero image is the LCP element, loaded eagerly
- [ ] The video makes **no** youtube.com requests until it's clicked
- [ ] The inline video link in the last paragraph stays a plain link
