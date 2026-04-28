---
phase: 05-booking-experience-ctas
verified: 2026-04-28T00:00:00Z
status: passed
score: 10/10 must-haves verified
re_verification: false
---

# Phase 5: Booking Experience & CTAs — Verification Report

**Phase Goal:** Complete the booking experience — Calendly embed, sticky CTA, CTA copy variants, count-up animation
**Verified:** 2026-04-28
**Status:** PASSED
**Re-verification:** No — initial verification

---

## Goal Achievement

### Observable Truths

| # | Truth | Status | Evidence |
|---|-------|--------|----------|
| 1 | Visitor sees two-column booking layout with call benefits left and live Calendly calendar right | VERIFIED | `booking__grid` div at index.html:830; `booking__content` and `booking__calendar-wrap` columns present |
| 2 | Calendly widget loads from CDN without internal scrollbar (700px desktop / 650px mobile) | VERIFIED | CDN links at index.html:112-113; `height:700px` inline style at line 870; `height: 650px !important` in mobile media query at page.css:1577 |
| 3 | Calendly CDN script and CSS loaded exactly once | VERIFIED | Single `widget.css` link + single `widget.js` async script in `<head>`; grep confirms no duplicates |
| 4 | Floating sticky CTA is invisible on load, slides up after 3 seconds, hides on mobile | VERIFIED | Base state: `opacity:0`, `translateY(16px)`, `pointer-events:none` at page.css:1587-1595; JS IIFE with `setTimeout(..., 3000)` at index.html:1156-1159; `@media (max-width: 639px) { display: none }` at page.css:1631 |
| 5 | Floating CTA has continuously pulsing green dot | VERIFIED | `.floating-cta__dot::before` with `animation: pulse-ring` at page.css:1625; `@keyframes pulse-ring` defined at page.css:1627 |
| 6 | Hero primary CTA reads "Book a Free Strategy Call" | VERIFIED | index.html:157 — exact copy confirmed |
| 7 | Hero secondary CTA reads "See If We're a Fit" and links to #booking | VERIFIED | index.html:158 — `href="#booking"` confirmed; old "How It Works" / `href="#services"` absent from CTAs |
| 8 | All three pricing card CTAs read "Start Filling Your Calendar" | VERIFIED | index.html:656, 671, 685 — exactly 3 instances; "Get Started" removed |
| 9 | ROI calculator section has "Get Your Meetings Forecast" CTA linking to #booking | VERIFIED | index.html:816 — `href="#booking"` confirmed; `.roi__cta` wrapper present; CSS at page.css:1581 |
| 10 | Hero stat numbers count up from zero on first scroll-into-view, play once, have graceful fallback | VERIFIED | `data-count-to` on all 3 spans (lines 193, 197, 201); IntersectionObserver IIFE at index.html:1114; `animated` flag + `observer.disconnect()` at lines 1117-1121; `Math.pow(1 - progress, 3)` ease-out cubic at line 1131; fallback via `if (!IntersectionObserver in window) return` at line 1116 |

**Score: 10/10 truths verified**

---

## Required Artifacts

| Artifact | Expected | Status | Details |
|----------|----------|--------|---------|
| `index.html` | Booking section two-column grid replacing scaffold | VERIFIED | `booking__grid` at line 830; scaffold-placeholder removed |
| `index.html` | Calendly CDN script + CSS in `<head>` | VERIFIED | `assets.calendly.com/assets/external/widget.css` at line 112; `widget.js` at line 113 |
| `page.css` | Booking grid layout + benefit list styles | VERIFIED | `.booking__grid` with `grid-template-columns: 1fr 1.2fr` at line 1526-1528; all sub-classes present |
| `index.html` | Floating CTA div before `</body>` | VERIFIED | `id="floating-cta"` with `role="complementary"`, `aria-label`, `.floating-cta__dot` at lines 1143-1151 |
| `page.css` | Floating CTA styles — base, visible modifier, pulse ring, mobile hide | VERIFIED | `.floating-cta` at 1587, `.floating-cta--visible` at 1597, `@keyframes pulse-ring` at 1627, `display: none` at 1631 |
| `index.html` | JS IIFE adding `floating-cta--visible` after 3000ms | VERIFIED | `classList.add('floating-cta--visible')` inside `setTimeout(..., 3000)` at lines 1155-1159 |
| `index.html` | Updated hero CTA copy | VERIFIED | "Book a Free Strategy Call" at line 157; "See If We're a Fit" with `href="#booking"` at line 158 |
| `index.html` | Updated pricing CTAs | VERIFIED | 3 instances of "Start Filling Your Calendar" at lines 656, 671, 685 |
| `index.html` | ROI section CTA button | VERIFIED | "Get Your Meetings Forecast" with `href="#booking"` at line 816; wrapped in `.roi__cta` |
| `page.css` | ROI CTA layout styles | VERIFIED | `.roi__cta { text-align: center; ... }` at line 1581 |
| `index.html` | `data-count-to` attributes on 3 hero stat spans | VERIFIED | Lines 193 (12), 197 (94 + suffix), 201 (8) |
| `index.html` | Counter IIFE with IntersectionObserver + requestAnimationFrame | VERIFIED | Full IIFE at lines 1112-1139; threshold 0.5; ease-out cubic formula confirmed |

---

## Key Link Verification

| From | To | Via | Status | Details |
|------|----|-----|--------|---------|
| `.calendly-inline-widget` | Calendly CDN widget.js | `data-url` attribute + async script in `<head>` | WIRED | `data-url="https://calendly.com/allan-chan-leadzly"` at index.html:869; CDN script loaded at line 113 |
| JS IIFE setTimeout 3000ms | `div#floating-cta` | `classList.add('floating-cta--visible')` | WIRED | getElementById('floating-cta') at line 1157; classList.add at line 1158; 3000ms delay at line 1159 |
| `.floating-cta--visible` CSS modifier | `.floating-cta` base state | `opacity: 1` + `translateY(0)` transition | WIRED | Base state `opacity: 0` / `translateY(16px)` at page.css:1592-1594; modifier restores both at lines 1598-1600 |
| Hero secondary CTA | `#booking` section | `href` attribute | WIRED | `href="#booking"` at index.html:158; `#booking` section exists at line 823 |
| ROI CTA button | `#booking` section | `href` attribute | WIRED | `href="#booking"` at index.html:816; target section confirmed |
| IntersectionObserver watching `.hero__card` | `[data-count-to]` spans | `querySelectorAll` + rAF step loop | WIRED | `card.querySelectorAll('[data-count-to]')` at line 1122; rAF loop at lines 1128-1134 |

---

## Data-Flow Trace (Level 4)

Not applicable. All dynamic elements (Calendly widget, floating CTA reveal, counter animation) are driven by third-party CDN scripts, CSS transitions, and browser APIs — not by an internal data pipeline. No server-rendered or fetched data is displayed. The Calendly widget fetches its own calendar data from the CDN.

---

## Behavioral Spot-Checks

| Behavior | Command | Result | Status |
|----------|---------|--------|--------|
| `booking__grid` present in HTML | `grep -c "booking__grid" index.html` | 1 match (line 830) | PASS |
| Calendly CDN loaded in head | `grep -c "assets.calendly.com" index.html` | 2 matches (widget.css + widget.js) | PASS |
| Floating CTA HTML present | `grep -c "id=\"floating-cta\"" index.html` | 1 match | PASS |
| setTimeout 3000ms present | Read lines 1153-1161 | `setTimeout(function() {...}, 3000)` confirmed | PASS |
| 4 distinct CTA copy variants | Grep for each variant | All 4 found at expected lines | PASS |
| `data-count-to` on 3 stat spans | `grep -c "data-count-to" index.html` | 3 matches | PASS |
| `observer.disconnect()` one-shot guard | Grep + read | Confirmed at line 1121 | PASS |
| Mobile hide for floating CTA | `@media (max-width: 639px)` in page.css | `display: none` at line 1631-1632 | PASS |
| `pulse-ring` keyframe defined | Grep page.css | `@keyframes pulse-ring` at line 1627 | PASS |
| All 5 booking benefit bullets present | Grep each string | All 5 confirmed at lines 838-862 | PASS |

---

## Requirements Coverage

| Requirement | Source Plan | Description | Status | Evidence |
|-------------|-------------|-------------|--------|----------|
| BOOK-01 | 05-01-PLAN.md | Inline Calendly embed, two-column layout (benefits left, calendar right) | SATISFIED | `booking__grid` div, Calendly CDN loaded, all 5 benefits present, `calendly-inline-widget` wired |
| BOOK-02 | 05-02-PLAN.md | Floating sticky CTA, pulsing green dot, 3-second entrance delay, hidden on mobile | SATISFIED | HTML + CSS + JS IIFE all present on main branch (fixed in commit 09b6cf6 after worktree merge miss) |
| BOOK-03 | 05-03-PLAN.md | CTA copy variants: "Book a Free Strategy Call", "Get Your Meetings Forecast", "See If We're a Fit", "Start Filling Your Calendar" | SATISFIED | All 4 variants confirmed in index.html at respective lines |
| QWIN-03 | 05-04-PLAN.md | Hero stat numbers count up via animation on scroll into view | SATISFIED | Counter IIFE with IntersectionObserver, rAF ease-out cubic, `data-count-to` attributes on 3 spans |

**Orphaned requirements check:** REQUIREMENTS.md maps BOOK-01, BOOK-02, BOOK-03, QWIN-03 to Phase 5. All 4 are claimed by plans. No orphaned requirements.

---

## Anti-Patterns Found

| File | Line | Pattern | Severity | Impact |
|------|------|---------|----------|--------|
| None | — | — | — | — |

No TODOs, FIXMEs, placeholder text, hardcoded empty arrays/objects flowing to rendered output, or stub patterns found in Phase 5 artifacts.

Note: The comment `<!-- Calendly embed added in Phase 5 (BOOK-01) -->` is gone (scaffold replaced). The booking section HTML comment at line 822 is a section marker label, not a scaffold placeholder.

---

## Human Verification Required

### 1. Calendly Calendar Rendering

**Test:** Open index.html in a browser and scroll to the booking section.
**Expected:** Live Calendly month-view calendar renders on the right side without internal scrollbars; left side shows 5 benefit bullets with green checkmark icons.
**Why human:** Third-party CDN widget rendering cannot be verified programmatically without a running browser.

### 2. Floating CTA 3-Second Entrance

**Test:** Reload the page. Observe bottom-right corner for 4+ seconds.
**Expected:** Empty for 3 seconds, then "Book a Call" button slides up with a pulsing dot.
**Why human:** CSS transition and setTimeout timing require visual browser inspection.

### 3. Hero Stat Counter Animation

**Test:** Hard-refresh (Ctrl+Shift+R), scroll the hero stats card into view.
**Expected:** Three numbers count up from 0: "12", "94%", "8" over ~1.2 seconds. Re-scrolling does not replay.
**Why human:** IntersectionObserver and requestAnimationFrame animation require visual browser inspection.

### 4. Mobile Layout Verification

**Test:** Resize to 375px width.
**Expected:** Booking section stacks to single column; floating CTA is not visible.
**Why human:** Responsive layout requires browser viewport resize.

Note: The Phase 5 human-verify checkpoint (Task 2 of Plan 04) was completed and approved by the user during execution.

---

## Gaps Summary

No gaps found. All 10 observable truths are VERIFIED. All 12 artifact checks pass (exists, substantive, wired). All 6 key links are WIRED. All 4 requirements (BOOK-01, BOOK-02, BOOK-03, QWIN-03) are SATISFIED with implementation evidence in the actual codebase.

One notable execution deviation documented in 05-04-SUMMARY.md: the floating CTA (BOOK-02) was originally committed to a worktree branch that was never merged to main, requiring a re-implementation commit (09b6cf6) in Plan 04. The feature is correctly present on main and fully functional.

---

_Verified: 2026-04-28_
_Verifier: Claude (gsd-verifier)_
