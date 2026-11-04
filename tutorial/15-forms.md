# Module 15: Forms

**Scenario:** Customer service wants a contact form whose fields they can change in a spreadsheet, with validation, accessible errors, and submission to their ticketing API.

**Local content:** `/drafts/m15-form`, with the definition in `/drafts/data/contact-form.json`. It submits to `https://httpbin.org/post` as a stand-in endpoint.

## Task 1: Form renderer, `blocks/form`

- The block has two rows: row 1 is the definition URL, and row 2 is the submit endpoint
- Build fields from the sheet (`Type`: text, email, select, textarea, checkbox, submit). `Options` is comma-separated, and `Pattern` maps to the `pattern` attribute.
- Every input gets an associated `<label>`, and required fields are marked visually and with `required`

## Task 2: Validation UX

- Use the Constraint Validation API (`checkValidity`, `validity.patternMismatch`) and **custom messages**, which can come from Module 12 placeholders
- Validate on blur, revalidate on input once a field has been touched, and on submit focus the first invalid field
- Connect errors with `aria-describedby`, and announce the summary with `aria-live="polite"`

## Task 3: Submission

- `fetch` POST as JSON, disable the button while the request is in flight, and add a timeout with `AbortController`
- On success, replace the form with a thank-you message (optionally from a fragment). On failure, show a retry option and keep the user's input.
- Use a honeypot field for spam, and explain why secrets (API keys) can **never** appear in this code or in the sheet. Where would a server-side relay go? (An App Builder action, Cloudflare Worker, or similar.)

## Done when

- [ ] Changing a label in the JSON changes the form, with no code change
- [ ] Bad input (`SO-12`) produces a pattern error that a screen reader announces
- [ ] httpbin echoes back your JSON payload

## Author it for real

Create a `contact-form` Sheet with the same columns and Preview it. Also look up Adobe's official **Forms for EDS** offering (document-based forms / the forms block in the block collection) and compare it with what you built.
