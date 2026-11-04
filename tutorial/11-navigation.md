# Module 11: Header & navigation hardening

**Scenario:** UX wants an active-page indicator, a header that hides on scroll down and reappears on scroll up, and dropdowns that work by keyboard. The audit also found focus escaping the open mobile menu.

**Local content:** `/drafts/m11-nav`. It uses a local nav through `nav: /drafts/nav` metadata.

## Task 1: Read before changing

Read `blocks/header/header.js` from top to bottom. Draw the nav DOM structure on paper: brand, sections and tools. Note where `isDesktop` changes behavior.

## Task 2: Active state

Mark the nav link that matches the current path with `aria-current="page"`. Its parent dropdown gets `.nav-drop.active`. Ignore query strings, so `/drafts/m09-products?category=sleeping` still matches the products page, and decide what happens when several links match.

## Task 3: Hide-on-scroll header

- Use a scroll listener throttled with `requestAnimationFrame` and `passive: true`
- Use `transform: translateY(-100%)` (compositor-only), never `top`. That avoids layout thrash.
- Never hide while a menu is open or focus is inside the header

## Task 4: Mobile focus trap

When the mobile menu is open: Tab and Shift+Tab cycle inside the nav, Escape closes it and returns focus to the hamburger, and the page behind gets `inert`.

## Task 5: Search tool

The `icon-search` in the tools section should open a search input that queries `/drafts/query-index.json` (title and description), debounced at 200 ms, with results as links. Load the index on **first focus**, not on page load.

## Done when

- [ ] `aria-current` is correct on every module page that uses `nav: /drafts/nav`
- [ ] No long tasks while scrolling (check the Performance panel)
- [ ] You can open, navigate and close the mobile nav using only the keyboard, without focus escaping
