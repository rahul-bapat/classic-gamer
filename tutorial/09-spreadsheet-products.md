# Module 09: Spreadsheet-driven blocks

**Scenario:** Merchandisers keep the product list in a Google Sheet (23 rows today, 2,000 by Black Friday). They want a filterable, sortable, paginated grid on the page, with filters shareable by URL.

**Local content:** `/drafts/m09-products`. Data is in `/drafts/data/products.json` (single sheet) and `/drafts/data/catalog.json` (multi-sheet).

## Task 1: Know the JSON contract

```bash
curl -s localhost:3000/drafts/data/products.json | head -20
curl -s localhost:3000/drafts/data/catalog.json | head -20
```
Note `total`, `offset`, `limit`, `:type` and `:names`. On a real site, a sheet is served at `/products.json?offset=0&limit=500`, and multi-sheet files are filtered with `?sheet=products`. **Every value is a string.**

## Task 2: Data layer in `scripts/data.js`

**Why:** You'll fetch sheets in many blocks, so build the helper once and do it well.

`fetchSheet(url, { sheet, limit = 500 })`:
- Pages through `offset`/`limit` until `total` is reached, because a large sheet isn't returned in one response
- Handles both single-sheet and multi-sheet responses
- Caches in memory for each URL, so two blocks don't fetch twice. Store the promise, not the result.
- Type-coerces values with a schema: `{ price: Number, inStock: (v) => v === 'true' }`

## Task 3: `blocks/product-list`

- Read the config with `readBlockConfig` (`source`, `page-size`, `filters`)
- Render cards with `createOptimizedPicture`. Show `salePrice` with a struck-through `price` when present.
- Add facets for `category` and `inStock`, sorting by price and name, and pagination
- Sync state with `URLSearchParams` (`?category=tents&sort=price-asc&page=2`). Use `history.replaceState`, and support back/forward with `popstate`.
- Show a skeleton state with a fixed height while loading, to avoid CLS. Show an error state if the fetch fails, and an empty state for no results.

## Task 4: Scale test

Write a quick Node script that generates `drafts/data/products-big.json` with 2,000 rows, and point the block at it. Measure render time. Consider rendering only the current page and reusing DOM nodes, and when you'd move to a real search API.

## Done when

- [ ] Nav links like `/drafts/m09-products?category=sleeping` open pre-filtered
- [ ] The back button restores the previous filter state
- [ ] Renaming the file in `Source` produces a friendly error, not a blank block
- [ ] The 2,000-row test keeps the main thread free of long tasks (>50 ms) after load

## Author it for real

Create a Google **Sheet** `products` with the same headers, then Preview it. You'll get `/products.json`. Add a second tab `categories` and confirm the multi-sheet format. **Gotcha:** in a workbook, tab naming (for example the `shared-` prefix) controls which tabs are delivered. Read https://www.aem.live/developer/spreadsheets, then check which tabs come back with `?sheet=`.
