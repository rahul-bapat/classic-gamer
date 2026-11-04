# Module 19: Capstone: Summer Sale campaign

**Brief from marketing:** "We need a Summer Sale landing page live by Friday. It needs a hero, a countdown banner, our top 4 sale products, an FAQ, a French version, tracking, and Lighthouse 100. Authors must be able to change everything without developers."

**Local content:** `/drafts/capstone-summer-sale` (template `campaign`, a promo banner, a product list with `On Sale Only`, and an accordion)

## Requirements

1. **Template `campaign`** hides the default nav sections and shows only the brand plus a CTA (landing page header), using the header and template loader from Modules 6 and 11
2. The **promo banner** shows a live countdown to `End` (days:hours:mins) and updates once a minute, not every frame
3. **Product list** supports the new `On Sale Only: true` config, and the "Add to cart" click pushes a data layer event (Module 14)
4. The **FAQ accordion** is deep-linkable
5. **FR version**: create `/drafts/fr/capstone-summer-sale` yourself. It uses placeholders, with currency in EUR
6. **SEO:** campaign-specific `og:image`, a canonical, and an `ItemList` JSON-LD of the shown products
7. **Performance:** 100 on mobile, CLS 0, and nothing third-party before consent
8. **Quality:** unit tests for the countdown math, lint is clean, and the PR links both test URLs

## Process (as on a real project)

1. Write a short content model **before coding**: what the author creates in the Doc and Sheet. Give it to "the author" (you, on the other machine).
2. Build against the local drafts on a branch, `capstone`
3. Author the real Doc, Sheet and FR copy on the content machine, then preview on `capstone--<repo>--<owner>.aem.page`
4. Open a PR → CI green → merge → publish content → verify on `aem.live`

## Done when

- [ ] A colleague with no EDS knowledge can change the end date, swap products and edit the FAQ using only the Doc and Sheet
- [ ] All eight requirements are verified on `aem.live`
- [ ] You've written a 5-line retro: what you'd do differently on a real project
