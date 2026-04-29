---
phase: 09-performance-core-web-vitals
verified: 2026-04-29T00:00:00Z
status: passed
score: 7/7 must-haves verified
re_verification: false
---

# Phase 09: Performance / Core Web Vitals Verification Report

**Phase Goal:** Deliver measurable improvements to Core Web Vitals and page load performance — trim unused font weights, externalise inline SVGs with lazy-loading, prevent Calendly CLS, confirm floating CTA is absent, and minify page.css with a cache-busting version bump.
**Verified:** 2026-04-29
**Status:** passed
**Re-verification:** No — initial verification

---

## Goal Achievement

### Observable Truths

| # | Truth | Status | Evidence |
|---|-------|--------|----------|
| 1 | Google Fonts loads only Inter weights 400, 600, 700 (no weight 500 variant) | VERIFIED | index.html line 48: `family=Inter:wght@400;600;700` |
| 2 | Plus Jakarta Sans loads only weights 400, 600, 700, 800 (no weight 500 variant) | VERIFIED | index.html line 48: `family=Plus+Jakarta+Sans:wght@400;600;700;800` |
| 3 | Pipeline board and outreach sequence SVGs are external files loaded lazily via `<img>` | VERIFIED | index.html lines 551-556, 596-601: both use `loading="lazy"` with explicit width/height |
| 4 | Calendly embed container pre-reserves 700px height on desktop and 650px on mobile | VERIFIED | page.css: `min-height:700px` on `.booking__calendar-wrap` and `min-height:650px` in `@media (max-width:767px)` |
| 5 | No floating CTA element exists anywhere in index.html or page.css | VERIFIED | grep returns 0 matches for floating/float-cta/fab-cta/sticky-cta/fixed-cta in both files |
| 6 | page.css is served as a minified single-line file | VERIFIED | `wc -l page.css` returns 0 (single line, no newlines); file is 34,717 bytes — comment-free, whitespace-stripped |
| 7 | index.html references page.css with version query string ?v=14 | VERIFIED | index.html line 51: `href="./page.css?v=14"` |

**Score:** 7/7 truths verified

---

### Required Artifacts

| Artifact | Expected | Status | Details |
|----------|----------|--------|---------|
| `outreach-sequence.svg` | Standalone SVG file, viewBox="0 0 480 520" | VERIFIED | File exists at project root; line 1 confirms `viewBox="0 0 480 520"` |
| `pipeline-board.svg` | Standalone SVG file, viewBox="0 0 640 500" | VERIFIED | File exists at project root; line 1 confirms `viewBox="0 0 640 500"` |
| `index.html` | Updated font URL, lazy-loaded img tags, ?v=14 | VERIFIED | Contains `Inter:wght@400;600;700`, both lazy img tags with width/height, `page.css?v=14` |
| `page.css` | Minified, no font-weight:500, min-height:700px, img selectors | VERIFIED | Single line (0 `\n`), `font-weight:500` count = 0, `min-height:700px` present, `.how__visual img` and `.channels__visual img` present |

---

### Key Link Verification

| From | To | Via | Status | Details |
|------|----|-----|--------|---------|
| index.html `.channels__visual` | outreach-sequence.svg | `<img src="./outreach-sequence.svg" loading="lazy">` | WIRED | index.html line 551-556 confirmed |
| index.html `.how__visual` | pipeline-board.svg | `<img src="./pipeline-board.svg" loading="lazy">` | WIRED | index.html line 596-601 confirmed |
| page.css `.booking__calendar-wrap` | Calendly widget height | `min-height: 700px` | WIRED | Confirmed in minified page.css output |
| index.html stylesheet link | page.css | `href="./page.css?v=14"` | WIRED | index.html line 51 confirmed |

---

### Data-Flow Trace (Level 4)

Not applicable — this phase produces no components that render dynamic data. All changes are HTML/CSS static optimisations.

---

### Behavioral Spot-Checks

| Behavior | Command | Result | Status |
|----------|---------|--------|--------|
| Font URL excludes weight 500 | grep for `wht@400;600;700` in index.html | Match found | PASS |
| No inline SVG blocks remain | grep for `viewBox="0 0 480 520"` and `viewBox="0 0 640 500"` in index.html | 0 matches | PASS |
| SVG files have correct viewBox | grep viewBox in each .svg file | `0 0 480 520` and `0 0 640 500` confirmed | PASS |
| No font-weight:500 in CSS | grep -c `font-weight: 500` page.css | 0 | PASS |
| page.css minified | wc -l page.css | 0 lines (single blob) | PASS |
| CSS custom properties preserved | grep `var(--color-primary` page.css | Match found | PASS |
| Cache-bust version bumped | grep `page.css?v=14` index.html | Match found | PASS |
| No floating CTA in HTML | grep count for floating/float-cta patterns | 0 | PASS |
| No floating CTA in CSS | grep count for floating/float-cta patterns | 0 | PASS |
| Calendly desktop min-height | grep `min-height:700px` page.css | Match found | PASS |
| Calendly mobile min-height | grep `min-height:650px` page.css | Match found | PASS |
| CSS selectors updated to img | grep `how__visual img` and `channels__visual img` | Both found | PASS |
| Old SVG selectors removed | grep `how__visual svg` and `channels__visual svg` | 0 matches | PASS |

---

### Requirements Coverage

| Requirement | Source Plan | Description | Status | Evidence |
|-------------|------------|-------------|--------|----------|
| PERF-01 | 09-01 | Google Fonts trimmed to weights 400, 600, 700 only (removes unused weight 500 variants) | SATISFIED | index.html line 48: `wght@400;600;700` for Inter, `wght@400;600;700;800` for PJS; REQUIREMENTS.md checked [x] |
| PERF-02 | 09-01 | Below-fold decorative SVG mockups lazy-loaded — not blocking initial HTML parse | SATISFIED | Both img tags have `loading="lazy"` with explicit width/height; inline SVGs removed from index.html; REQUIREMENTS.md checked [x] |
| PERF-03 | 09-01 | Calendly embed container given explicit `min-height` — no layout shift on load | SATISFIED | `min-height:700px` desktop, `min-height:650px` mobile in page.css; REQUIREMENTS.md checked [x] |
| PERF-04 | 09-02 | Floating CTA causes zero CLS — container pre-reserved or transition adjusted | SATISFIED | 0 grep matches for any floating/fab-cta/sticky-cta pattern; satisfied by absence; REQUIREMENTS.md checked [x] |
| PERF-05 | 09-02 | `page.css` minified — served as minified file, version-bumped | SATISFIED | page.css is single-line (34,717 bytes, 0 newline count); `?v=14` on stylesheet link; REQUIREMENTS.md checked [x] |

All 5 phase requirements (PERF-01 through PERF-05) are marked [x] in REQUIREMENTS.md and verified against actual codebase artifacts.

No orphaned requirements — all PERF-01 through PERF-05 appear in plan frontmatter and REQUIREMENTS.md traceability table.

---

### Anti-Patterns Found

| File | Pattern | Severity | Impact |
|------|---------|----------|--------|
| — | — | — | — |

No anti-patterns found. No TODO/FIXME/placeholder comments, no empty implementations, no hardcoded empty data flows, no stub indicators detected in the modified files.

---

### Human Verification Required

**1. Visual rendering after minification**

**Test:** Open index.html in a browser and visually compare against a pre-minification screenshot or memory.
**Expected:** Identical visual appearance — same layout, colours, typography, spacing.
**Why human:** CSS correctness after minification cannot be verified programmatically without a rendering engine. CleanCSS DEFAULT level is conservative, but a human eyeball on the live page confirms no property was unexpectedly altered.

**2. Lazy-loaded SVGs render correctly**

**Test:** Open the page in a browser and scroll to the "Channels" section (outreach-sequence.svg) and "How It Works" section (pipeline-board.svg). Verify both SVG mockup graphics appear.
**Expected:** Both decorative UI mockup graphics are visible, styled correctly (drop-shadow, border-radius), not broken.
**Why human:** The img tags with `loading="lazy"` require a browser to verify the external SVG path resolves and the CSS rules (`.how__visual img`, `.channels__visual img`) apply correctly.

**3. Calendly booking section — no layout shift on load**

**Test:** Open the booking section in a browser on desktop and mobile. Observe the calendar area as Calendly loads.
**Expected:** The space for the Calendly widget is pre-reserved — no jump or reflow occurs as the widget loads.
**Why human:** CLS measurement requires a browser with Calendly's CDN script loading in real conditions.

---

### Gaps Summary

No gaps identified. All 7 must-have truths are verified against the actual codebase. All 5 PERF requirements are satisfied and checked off in REQUIREMENTS.md.

Phase 09 goal is fully achieved: Google Fonts trimmed, SVGs externalised and lazy-loaded, Calendly CLS prevention in place, floating CTA confirmed absent, page.css minified and cache-busted.

---

_Verified: 2026-04-29_
_Verifier: Claude (gsd-verifier)_
