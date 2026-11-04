# Module 10: Query index & content listings

**Scenario:** The journal landing page and the "related articles" rail must update themselves whenever an article is published. Nobody should maintain lists by hand.

**Local content:** `/drafts/m10-blog`, `/drafts/blog/*`, and the mock index `/drafts/query-index.json`

## Task 1: Read the index like production

```bash
curl -s localhost:3000/drafts/query-index.json
```
The mock contains three realistic problems:
- `tags` is a **JSON-encoded string** (`"[\"Tents\"]"`), not an array
- One `date` is an **Excel serial number** (`46160`), which is what Sheets and indexing produce for date-typed cells. The others are ISO strings.
- There's a non-article row (`/drafts/m09-products`) and an empty unpublished row

## Task 2: `blocks/article-list`

- Reuse `fetchSheet` from Module 9
- Filter by the `path` prefix from the config and drop rows with no title
- Normalize dates to `Date` with a `parseSheetDate(v)` helper that handles ISO, serial numbers (`(serial - 25569) * 86400000`) and Unix seconds. Put it in `scripts/utils.js`.
- Sort newest first, and support `?tag=Tents` filtering (the chips from Module 6 link here)
- Use the `image` column for thumbnails through `createOptimizedPicture`

## Task 3: Related articles (auto-block)

On `template: article` pages, append a "Related" section that shows up to 3 articles sharing at least one tag, ranked by overlap and then by date, and excluding the current page. Run this in `loadLazy` (it's below the fold) or later.

## Task 4: Configure the real index

The current boilerplate has **no** `helix-query.yaml`. Index definitions live in the **configuration service**. Using the docs (https://www.aem.live/developer/indexing), define an index that includes `/blog/**` and extracts `title`, `description`, `image`, `tags` (from meta `article:tag` or a custom meta) and `date`.

## Done when

- [ ] The listing shows 4 articles in the right order with correctly formatted dates (including the serial one)
- [ ] `?tag=Safety` shows 2 articles
- [ ] The tent guide's related rail shows "Winter camping checklist" first, because it shares the most tags

## Author it for real

Publish three Docs under `/blog/` with metadata. Wait for indexing, then open `https://main--<repo>--<owner>.aem.live/query-index.json`. Switch the block's `Source` to `/query-index.json` and the `Path` to `/blog/`.
