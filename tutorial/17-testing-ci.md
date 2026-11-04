# Module 17: Quality: tests & CI

**Scenario:** A teammate's change to `cards.js` broke the homepage on a Friday. You'll add guardrails: unit tests for pure logic, DOM tests for blocks, and CI checks on every PR, all **without** adding a build step to what gets shipped.

## Task 1: Unit tests with Node's built-in runner

**Why:** Pure functions such as `isActive`, `parseSheetDate`, `t()` and the `fetchSheet` pagination are the cheapest things to test.

- Add `test/` (already in `.hlxignore`) and `"test": "node --test test/"` to `package.json`
- Test `parseSheetDate` with ISO, serial and epoch-seconds inputs, and `isActive` around the start and end boundaries, including timezone offsets
- Test `fetchSheet` pagination by stubbing `globalThis.fetch`

## Task 2: Block DOM tests

**Why:** Blocks are functions of `(block) => DOM`, which makes them easy to test.

- Use a browser-like test runner (for example `@web/test-runner`) as a **devDependency** only
- Load `drafts/m02-cards.plain.html`, build the block element the way `decorateBlock` would, call `decorate()`, and assert on the result: the malformed rows from Module 2 must not throw
- Snapshot the accordion and tabs ARIA attributes

## Task 3: GitHub Actions

Look at `.github/workflows/` in the boilerplate (it already lints). Extend it to:
1. Run `npm run lint` and `npm test` on every PR
2. Run the perf budget script from Module 13 against `https://${{ github.head_ref }}--<repo>--<owner>.aem.page/`. Wait for Code Sync, polling until the branch URL returns your commit's code.
3. Fail the PR if the description doesn't contain a `--<repo>--<owner>.aem.page` link. That's an EDS convention, and the AEM PSI check app enforces something similar.

## Task 4: PR template

Add `.github/pull_request_template.md` with sections for test URLs (before and after), a screenshot, and a Lighthouse score.

## Done when

- [ ] `npm test` passes locally, and CI is green on a PR
- [ ] Reintroducing the Module 2 bug (removing the empty-cell guard) fails CI
- [ ] Nothing in `test/` or `tools/` is served (`curl https://<branch>--<repo>--<owner>.aem.page/test/...` returns 404)
