# Module 01: Pipeline forensics

**Scenario:** An author says "the columns look broken." Before you change any code, you need to see what EDS delivered, what the client-side code changed, and when each step ran.

**Local content:** `/drafts/m01-anatomy`

## Task 1: Compare the three versions of a page

**Why:** Most EDS bugs come from assuming the wrong markup.

```bash
curl -s localhost:3000/drafts/m01-anatomy.plain.html    # authored content only (what fragments fetch)
curl -s localhost:3000/drafts/m01-anatomy                # full server HTML (head.html + metadata → <meta>)
```

Then open the page in DevTools **Elements** to see the decorated DOM. Write down:
- which `<meta>` tags came from the metadata block
- what `section-metadata` became (a class on the `.section`, or a `data-*` attribute?)
- which wrapper/container classes `decorateSections`/`decorateBlocks` added (`columns-wrapper`, `columns-container`, `default-content-wrapper`)
- why the `<strong><a>` became `a.button.primary` (read `decorateButtons` in `scripts/scripts.js`)

## Task 2: Instrument the lifecycle

**Why:** You need to know which phase your code runs in (eager, lazy or delayed) before you add anything that affects performance.

In `scripts/scripts.js`, add `performance.mark()` calls at the start and end of `loadEager`, `loadLazy` and `loadDelayed`. Add one more inside `loadBlock` timing, but **without editing aem.js**: watch `data-block-status` with a `MutationObserver` instead. Print the result as a `console.table` when `?debug=perf` is in the URL.

<details><summary>Hint</summary>

```js
const debug = new URLSearchParams(window.location.search).get('debug') === 'perf';
const mark = (n) => debug && performance.mark(n);
```
To print timings: `performance.getEntriesByType('mark')`. To find LCP: `new PerformanceObserver(...).observe({ type: 'largest-contentful-paint', buffered: true })`.
</details>

## Task 3: Find out what's in the LCP section

**Why:** Only the **first section** is loaded eagerly, and `waitForFirstImage` decides whether to wait for the first image. Prove it.

Answer from your table: which blocks loaded before LCP? When did `columns` (section 2) load? Throttle the network to Slow 4G and run it again.

## Done when

- [ ] `?debug=perf` prints a table with the eager, lazy and delayed phases, per-block load times, and the LCP element
- [ ] You can explain why `body.appear` exists (it prevents a flash of undecorated content)
- [ ] Without `?debug=perf`, nothing extra runs and the code stays out of the critical path

## Author it for real

Create a Doc `m01-anatomy` containing a Hero table, two sections separated by `---`, a `Section Metadata` table (`Style | highlight`) and a `Metadata` table. Preview it, then compare `.plain.html` from `aem.page` with your local draft. **Note:** on real previews, metadata keys are normalized to lowercase. The local drafts use lowercase keys for the same reason.
