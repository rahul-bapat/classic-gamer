# Module 16: SEO & structured data

**Scenario:** The SEO agency's audit asks for product rich results (price and availability in Google), correct canonicals for products with filter URLs, hreflang for `/fr/`, and 301s for 400 old URLs from the legacy site.

**Local content:** `/drafts/m16-pdp` (metadata: sku, price, currency, availability, canonical)

## Task 1: What EDS already does

```bash
curl -s localhost:3000/drafts/m16-pdp | grep -i "<meta\|<link\|<title"
```
Note which metadata keys become standard tags (title, description, og:*, twitter:*), and which become plain `<meta name>` tags that you have to act on (`sku`, `price`, `canonical`). On aem.live, also check how a `canonical` value is rendered.

## Task 2: JSON-LD for `template: product`

Create `templates/product/product.js` (using the loader from Module 6). Build a `schema.org/Product` with an `Offer` from metadata, and inject it as `<script type="application/ld+json">`.
- The CSP and Trusted Types are strict. Does setting `textContent` on a script element of type `application/ld+json` count as script execution? Test it, and read the console.
- Validate the output with Google's Rich Results Test against your **aem.page** URL.

## Task 3: Canonical & hreflang

- When filter query params are present (`?category=`), make sure the canonical points to the clean URL
- Add `<link rel="alternate" hreflang>` pairs for EN and FR. Decide whether to do it client-side or through authored metadata, and look up what Google says about JS-injected hreflang. (Hint: authored metadata wins.)

## Task 4: Platform-level SEO (config, not code)

Read the docs and plan each of these, then implement them on your real site:
- **Redirects:** a `redirects` Sheet with `Source | Destination` columns. Write a Node script that converts a legacy CSV of 400 rows into that format, normalizing trailing slashes and lowercase.
- **Sitemap:** configured through the configuration service and backed by the query index. Exclude `/drafts/**` and `/fragments/**`.
- **robots.txt** and `noindex` for `aem.page`/`aem.live` hosts versus your production domain. Check what EDS does by default.
- **Bulk metadata Sheet** for default `og:image` and `description` per folder.

## Done when

- [ ] The Rich Results Test detects a valid Product on your PDP preview
- [ ] Filtered product URLs have a clean canonical
- [ ] Redirects and the sitemap work on `aem.live`
