# Module 18: Operating the platform

**Scenario:** You're on the release team now. You need to bulk-preview 300 migrated pages, explain why an author's change "isn't live", put the site behind the company CDN, and give authors a custom Sidekick button.

This module is mostly HTTP and config, so run it from this machine with `curl`/Node. You don't need Google Drive here.

## Task 1: Admin API basics

**Why:** Everything Sidekick does is an Admin API call.

Read https://www.aem.live/docs/admin.html, then for one page on your site:
```bash
curl -s https://admin.hlx.page/status/<owner>/<repo>/main/<path>   # what's previewed, live, and the source
```
Interpret the `preview`, `live` and `edit` timestamps and status codes. Find out which operations need authentication (site authentication, API keys, and how to get a token). **Don't commit any tokens.**

## Task 2: Bulk operations script

Write `tools/bulk-preview.mjs`. It reads a list of paths (one per line), calls the preview endpoint with limited concurrency (for example 5 at a time), retries with backoff on 429 or 5xx, and writes a CSV report. Then add `--publish`. Look up the bulk job endpoints as well and compare the two approaches.

## Task 3: Debugging "not live"

Build a decision checklist (as a Markdown table in `tools/README.md`):
Doc edited → previewed? → published? → CDN cache (push invalidation)? → browser cache? → code on `main`? → is the content in a fragment or nav that wasn't republished?
Test each step using headers (`curl -I`) on `aem.page` and `aem.live`.

## Task 4: Production CDN & headers

- Read the **BYO CDN** setup docs for your company's CDN (Akamai, Cloudflare, Fastly or CloudFront), and identify the required headers and push invalidation settings
- Configure **custom HTTP headers** (for example `X-Frame-Options` and cache rules for `/drafts/**`) through the configuration service
- Explain why `head.html` has `move-to-http-header="true"` on the CSP meta tag

## Task 5: Sidekick customization

Through the project's Sidekick config (configuration service), add a custom button "Open in Lighthouse" that opens PageSpeed Insights for the current URL. Add a second custom button that's visible only on `/blog/**`.

## Done when

- [ ] The bulk script previews 20 pages and produces a report
- [ ] You can diagnose a "not live" ticket in under 2 minutes with your checklist
- [ ] The custom Sidekick buttons appear on the content machine
