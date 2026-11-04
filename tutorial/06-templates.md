# Module 06: Sections, templates & themes

**Scenario:** The editorial team publishes 20 articles a month. Each one needs a byline (author, date, read time), tag chips and a narrow reading layout. Authors refuse to build those tables by hand, so everything must come from **metadata**.

**Local content:** `/drafts/m06-article` (metadata `template: article`), plus `/drafts/blog/*`

## Task 1: Section metadata styles & backgrounds

**Why:** Authors control layout through `Section Metadata` without needing new blocks.

- Look at how `Style: dark, narrow` reaches the DOM (`decorateSections` in aem.js), then style `.section.dark` and `.section.narrow`.
- `Background` is a picture. Turn it into a CSS background image for the section. Read the value from `section.dataset` or from the original metadata block, depending on what aem.js leaves behind. Inspect it first.
- Keep text contrast WCAG AA on dark backgrounds.

## Task 2: Template loader

**Why:** Page-type logic shouldn't load on pages that don't need it.

In `scripts/scripts.js`, after `decorateTemplateAndTheme()`, read `getMetadata('template')`. If it's set, dynamically `import(\`/templates/${name}/${name}.js\`)` and load the matching CSS, **before** the first section loads. Use an allowlist so a typo can't trigger a 404 fetch.

<details><summary>Design question</summary>

Should the template run in `loadEager`, which blocks LCP, or `loadLazy`? The byline sits above the fold, so if it renders late it causes CLS. Measure both options and pick one.
</details>

## Task 3: `templates/article/article.js`

Build these from metadata:
- **Byline** under the H1: author, a formatted `publication-date` (`Intl.DateTimeFormat`), and a read time computed from the word count of `main`
- **Tag chips** at the end of the article, linking to `/journal?tag=<tag>`
- The first picture after the H1 becomes a full-width figure, which is the LCP image. Make sure it's `loading=eager`.

## Task 4: Theme

Add a `theme` metadata value `night`, which uses dark tokens. Override CSS custom properties on `body.night` only, with no block-specific CSS changes.

## Done when

- [ ] `/drafts/m06-article` shows the byline, read time and tags, and narrow dark section 2 has its background image
- [ ] Pages without `template` make **zero** template requests (check the Network panel)
- [ ] Lighthouse CLS is 0 with the template applied

## Author it for real

Add `Template | article`, `Author`, `Publication Date` and `Tags` to a Doc's Metadata table. Bonus: use the **bulk metadata** sheet (`metadata` spreadsheet with a `URL` column like `/blog/**`) to apply `template: article` to every blog page at once.
