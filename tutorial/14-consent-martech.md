# Module 14: Consent, martech & CSP

**Scenario:** Legal requires that no analytics or marketing tags load before consent. Marketing wants Adobe Analytics/Launch (or GTM) live by next sprint. Security requires the strict CSP in `head.html` to stay as it is.

**Local content:** `/drafts/m14-consent`

## Task 1: Understand the existing plumbing

Read `scripts/consent-check.js` and `scripts/consented.js`. Map out the `?consent=accept` override, the `consent.update` event, and why `consented.js` is imported from `loadDelayed`.

## Task 2: Build a real consent banner

Replace the dummy `hasConsent()`:
- `blocks/consent-banner` or a delayed-loaded module with Accept, Reject and Manage (categories: analytics, marketing)
- Persist the choice (cookie or `localStorage`, with an expiry and a version, so that changing the policy re-prompts users)
- Dispatch `consent.update` with a category-level `detail`, and support the user changing their mind later through a footer link
- The banner must not cause CLS. Use a fixed overlay at the bottom, rendered in the delayed phase.

## Task 3: Load martech correctly

In `consented.js`, load your tag manager **only for the consented categories**, using `loadScript` from aem.js. Use a fake endpoint such as `https://example.com/analytics.js`, and watch the Network panel.

Then work through the CSP: `script-src 'nonce-aem' 'strict-dynamic'` with `require-trusted-types-for 'script'`.
- Why does a script injected by `loadScript` run even though it has no nonce? (`'strict-dynamic'`)
- What breaks if a vendor uses `innerHTML` with a script string? Read the Trusted Types `default` policy in `scripts.js`.
- Why would an inline `<script>` pasted into `head.html` without `nonce="aem"` fail?

## Task 4: Data layer

Push `pageview` and the Module 9 `product-click` events to `window.adobeDataLayer` (or `dataLayer`) **regardless of consent**. Pushing to an array is free. Only the vendor library that reads it is gated.

## Done when

- [ ] Fresh visit: zero third-party requests until Accept
- [ ] Reject persists across reloads, and changing your mind through the footer link loads the tags without a reload
- [ ] No CSP violations in the console, and Lighthouse TBT is unchanged
