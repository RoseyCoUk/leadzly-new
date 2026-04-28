---
phase: 04-conversion-sections
verified: 2026-04-27T12:00:00Z
status: gaps_found
score: 9/11 must-haves verified
gaps:
  - truth: "The comparison table section uses a dark navy background (as specified in REQUIREMENTS.md SECT-01)"
    status: failed
    reason: "REQUIREMENTS.md SECT-01 explicitly states 'dark navy background'. After a post-verification human review in Plan 04, the background was changed from var(--color-dark) to var(--color-primary-soft) (light green). The implemented background is light green, not dark navy."
    artifacts:
      - path: "page.css"
        issue: ".comparison rule at line 1152 uses background: var(--color-primary-soft) — contradicts SECT-01 requirement text 'dark navy background'"
    missing:
      - "Requirement SECT-01 text specifies 'dark navy background' — either update REQUIREMENTS.md to reflect the approved light-theme decision, or revert .comparison background to var(--color-dark)"

  - truth: "Hero h1 uses font-weight 900 (as specified in REQUIREMENTS.md DSGN-01)"
    status: failed
    reason: "REQUIREMENTS.md DSGN-01 says 'Hero h1 uses font-weight 900'. The actual .hero__headline rule in page.css line 275 has font-weight: 800. The Plan 01 plan never changed this value (it was pre-existing at 800 and the plan only required the clamp change). This is a requirements text vs. implementation discrepancy — not a regression."
    artifacts:
      - path: "page.css"
        issue: ".hero__headline font-weight is 800 at line 275; DSGN-01 requires 900"
    missing:
      - "Either change .hero__headline to font-weight: 900 to satisfy DSGN-01 literally, or update REQUIREMENTS.md to accept 800 as equivalent (visually the difference is minor at heavy weights)"
human_verification:
  - test: "FAQ accordion interactive behaviour"
    expected: "Clicking any question opens it and collapses the previously open one. Clicking an open question collapses it (toggle off). Chevron rotates 180deg on open. Tab + Enter/Space also toggles."
    why_human: "JS IIFE class-toggle and CSS transition behaviour cannot be verified by grep — requires live browser interaction"
  - test: "ROI calculator real-time update"
    expected: "Changing deal value to 10000 (with current=4, target=20) shows £16,000/month and £192,000/year instantly. Clearing the field shows £0, not NaN."
    why_human: "JS input event listener and DOM textContent update require a live browser to exercise"
  - test: "Comparison table horizontal scroll on narrow viewport"
    expected: "At 375px viewport width the table container scrolls horizontally rather than breaking layout. Table content is not clipped."
    why_human: "overflow-x: auto behaviour depends on browser rendering — grep confirms the CSS rule exists but scroll behaviour needs DevTools or a physical device"
  - test: "Hero headline visual scale"
    expected: "At desktop (1200px+), computed font-size is at or above 56px. At 375px viewport, font-size is exactly 56px (clamp minimum 3.5rem = 56px at 16px base)."
    why_human: "clamp() rendering depends on viewport width; computed value requires DevTools"
---

# Phase 4: Conversion Sections Verification Report

**Phase Goal:** Visitors encounter a side-by-side comparison of Leadzly against alternatives, can self-qualify via FAQ, can calculate their own ROI, and experience consistent typographic hierarchy throughout.
**Verified:** 2026-04-27T12:00:00Z
**Status:** gaps_found — 2 requirement-text gaps (font-weight and comparison background); all functional features implemented
**Re-verification:** No — initial verification

---

## Goal Achievement

### Observable Truths

| #  | Truth | Status | Evidence |
|----|-------|--------|---------|
| 1  | Hero headline renders at minimum 3.5rem at any viewport width | VERIFIED | page.css line 274: `font-size: clamp(3.5rem, 3.5vw + 0.5rem, 4.75rem)` |
| 2  | Every section eyebrow label is visually bolder (weight 700) | VERIFIED | page.css line 19: `.section-label { font-weight: 700; }` |
| 3  | A comparison table is visible at the #comparison section | VERIFIED | index.html line 545: `<div class="comparison__table-wrap">` present |
| 4  | 8 rows of comparison data are present | VERIFIED | `grep -c comparison__row index.html` returns 8 |
| 5  | The Leadzly column has a visually distinct highlighted background | VERIFIED | page.css line 1183: `.comparison__th--leadzly { background: var(--color-primary-highlight) !important; }` and `comparison__cell--leadzly` has `background: var(--color-primary-soft)` |
| 6  | On narrow viewports the table scrolls horizontally rather than breaking | VERIFIED (code) | page.css line 1159: `.comparison__table-wrap { overflow-x: auto; }` and `min-width: 560px` on table — visual confirm needs human |
| 7  | 7 FAQ questions are in the accordion | VERIFIED | 7 `faq__item` `<div>` elements present; 7 `aria-expanded="false"` buttons; 7 `role="region"` answer divs |
| 8  | Clicking a question opens it and collapses others (one-at-a-time) | VERIFIED (code) | IIFE at index.html line 1013–1031: collapses all then opens clicked if not already open |
| 9  | Chevron rotates 180deg when a question is open | VERIFIED | page.css line 1280: `.faq__item--active .faq__icon { transform: rotate(180deg); }` |
| 10 | ROI calculator section exists between FAQ and Booking | VERIFIED | `<section class="roi">` at line 775; faq at 691; booking at 818 |
| 11 | 3 inputs update 2 outputs in real time; default outputs are £8,000/£96,000 | VERIFIED (code) | IIFE at lines 1037–1060: `addEventListener('input', calculate)`; formula `Math.max(0, target-current) * deal * 0.10`; static HTML fallback shows £8,000 / £96,000 |

**Score:** 11/11 truths verified at code level. 2 requirement-text gaps (see gaps section).

---

### Required Artifacts

| Artifact | Expected | Status | Details |
|----------|----------|--------|---------|
| `page.css` | Hero clamp at 3.5rem minimum | VERIFIED | Line 274: `clamp(3.5rem, 3.5vw + 0.5rem, 4.75rem)` |
| `page.css` | `.section-label font-weight: 700` | VERIFIED | Line 19: `font-weight: 700` |
| `index.html` | Comparison table with `.comparison__table-wrap` | VERIFIED | Line 545 |
| `page.css` | Comparison table CSS (`.comparison__table`, `.comparison__th--leadzly`, etc.) | VERIFIED | Lines 1157–1237 |
| `index.html` | FAQ accordion — 7 `.faq__item` blocks | VERIFIED | Lines 698–774; 7 items with aria attributes |
| `page.css` | FAQ accordion CSS (`.faq__answer`, max-height animation, `.faq__icon` rotation) | VERIFIED | Lines 1243–1299 |
| `index.html` | FAQ IIFE script block | VERIFIED | Lines 1012–1033 |
| `index.html` | ROI calculator `.roi__card` section between FAQ and Booking | VERIFIED | Line 782 inside section at line 775 |
| `index.html` | ROI IIFE with correct formula and `toLocaleString` | VERIFIED | Lines 1037–1060; `CONVERSION_RATE = 0.10`; `Math.max(0, target - current)` |
| `page.css` | ROI CSS (`.roi__card`, `.roi__output`, 3-col inputs at >=768px, 2-col outputs at >=640px) | VERIFIED | Lines 1409–1520; `repeat(3, 1fr)` at line 1429; `repeat(2, 1fr)` at line 1477 |

---

### Key Link Verification

| From | To | Via | Status | Details |
|------|----|-----|--------|---------|
| `page.css .hero__headline` | `index.html h1.hero__headline` | class selector | WIRED | clamp(3.5rem) confirmed in both |
| `page.css .section-label` | All section eyebrow spans | class selector | WIRED | font-weight: 700 at line 19; no overrides |
| `index.html .comparison__table-wrap` | `page.css .comparison__table-wrap` | class selector — overflow-x: auto | WIRED | Both present; mobile scroll rule confirmed |
| `index.html .faq__question (button)` | `.faq__answer` | aria-controls + IIFE class toggle | WIRED | 7 buttons with `aria-controls="faq-answer-N"`; IIFE adds `.faq__item--active` |
| `page.css .faq__item--active .faq__answer` | `max-height: 0` base rule | CSS transition | WIRED | max-height: 0 at line 1284; max-height: 600px at line 1288 |
| `index.html #roi-deal-value, #roi-current-meetings, #roi-target-meetings` | `#roi-monthly, #roi-annual` spans | JS `addEventListener('input', calculate)` | WIRED | All 5 element IDs present; IIFE connects them with `input` listener |
| `index.html section.roi` | Between section.faq and section.booking | DOM order | WIRED | faq: line 691; roi: line 775; booking: line 818 |

---

### Data-Flow Trace (Level 4)

N/A — this is a static HTML/CSS/vanilla JS page with no backend data pipeline. All "data" is authored directly in HTML or computed client-side from user inputs. The ROI calculator reads live DOM input values (not a static store) so the data-flow is self-contained and verified in the IIFE trace above.

---

### Behavioral Spot-Checks

| Behavior | Check | Result | Status |
|----------|-------|--------|--------|
| Hero clamp minimum | grep for `clamp(3.5rem` in page.css | 1 match at line 274 | PASS |
| section-label weight | grep for `font-weight: 700` in .section-label rule | Confirmed at line 19 | PASS |
| Comparison rows count | `grep -c comparison__row index.html` | 8 | PASS |
| FAQ items count | `grep -c "faq__item" index.html` returns 11 (7 divs + 3 JS strings + 1 active class) | 7 opening `<div class="faq__item"` confirmed | PASS |
| aria-expanded present | `grep -c aria-expanded index.html` | 12 (7 FAQ buttons + nav hamburger + 4 JS references) | PASS |
| roi__card present | grep in index.html | Line 782 confirmed | PASS |
| CONVERSION_RATE formula | grep in index.html | Line 1045 confirmed | PASS |
| Section order: faq→roi→booking | Line numbers 691, 775, 818 | Correct ascending order | PASS |
| scaffold-placeholder remaining | grep in index.html | Only booking placeholder at line 826 remains (Phase 5 item) | PASS |
| ROI CSS grid columns | grep repeat(3, 1fr) and repeat(2, 1fr) in page.css | Lines 1429 and 1477 confirmed | PASS |
| FAQ accordion JS | IIFE present with querySelectorAll + click listener | Lines 1013–1031 confirmed | PASS |
| No hardcoded hex in comparison CSS | grep "#" in comparison rules | Zero matches | PASS |

---

### Requirements Coverage

| Requirement | Source Plan | Description | Status | Evidence |
|-------------|------------|-------------|--------|---------|
| DSGN-01 | 04-01, 04-04 | Hero h1 font-weight 900 and >=3.5rem clamp; consistent eyebrow labels green | PARTIAL | Clamp 3.5rem: VERIFIED. Section-label weight 700: VERIFIED. Font-weight 900: NOT MET — actual is 800. |
| SECT-01 | 04-02 | Comparison table with dark navy background, 8 criteria rows, Leadzly column green | PARTIAL | 8 rows: VERIFIED. Leadzly column: VERIFIED. Dark navy background: NOT MET — actual is var(--color-primary-soft) light green (changed post-verification by user feedback). |
| SECT-02 | 04-03 | FAQ accordion, 7 questions, one open at a time, max-width 720px centred | VERIFIED | All criteria met. 7 items, IIFE toggle, max-width: 720px on .faq__list confirmed. |
| SECT-03 | 04-04 | ROI calculator — 3 inputs, 2 outputs, all client-side JS | VERIFIED | All criteria met. Inputs, formula, outputs, IIFE all confirmed. |

---

### Anti-Patterns Found

| File | Line | Pattern | Severity | Impact |
|------|------|---------|----------|--------|
| index.html | 826 | `<p class="scaffold-placeholder">Booking calendar coming in Phase 5.</p>` | INFO | Expected — booking embed is Phase 5 scope; not a Phase 4 anti-pattern |

No other TODO, FIXME, placeholder, or empty-implementation patterns found in Phase 4 modified files.

---

### Gaps Summary

**Gap 1: SECT-01 background colour vs. requirement text**

REQUIREMENTS.md SECT-01 specifies a "dark navy background" for the comparison table section. The implementation originally used `var(--color-dark)` (dark navy) but was changed to `var(--color-primary-soft)` (light green tint) during the human visual review in Plan 04 — the user preferred a light theme consistent look.

The code works correctly and looks good, but the REQUIREMENTS.md text is now stale/contradicted by the approved implementation. This is not a functional failure — it is a documentation vs. implementation drift.

**Resolution options (for `/gsd:plan-phase --gaps`):**
- Option A: Update REQUIREMENTS.md SECT-01 to say "light green-tinted background (var(--color-primary-soft))" to reflect the approved change.
- Option B: Revert page.css `.comparison` to `var(--color-dark)` if the dark-navy requirement is firm.

**Gap 2: DSGN-01 font-weight 900 vs. implemented 800**

REQUIREMENTS.md DSGN-01 says "Hero h1 uses font-weight 900". The `.hero__headline` rule in page.css uses `font-weight: 800`. The Plan 01 work never touched font-weight (the plan correctly stated "Do NOT change font-weight — already 800"). The original 800 value predates Phase 4.

At heavy weights (800 vs. 900) the visual difference is negligible with the Inter font, but the requirement text is not met.

**Resolution options:**
- Option A: Change `.hero__headline { font-weight: 900; }` in page.css (one-line fix, safe).
- Option B: Update REQUIREMENTS.md DSGN-01 to accept 800 as the canonical weight.

---

### Human Verification Required

#### 1. FAQ Accordion Interactive Behaviour

**Test:** Open index.html in a browser. Click the first FAQ question — it should expand smoothly with a max-height animation and the chevron should rotate 180deg. Click the second question — the first should collapse and the second expand. Click the open question again — it should collapse (toggle off). Tab to a button via keyboard and press Enter — it should open/close.
**Expected:** One-at-a-time toggle, smooth animation, keyboard accessible.
**Why human:** JS class-toggle and CSS transition behaviour cannot be verified by grep.

#### 2. ROI Calculator Real-Time Update

**Test:** Open index.html in a browser. Default outputs should show £8,000 (monthly) and £96,000 (annual). Change deal value to 10000 — outputs should instantly show £16,000 and £192,000. Clear the deal value field — outputs should show £0, not NaN or a blank string.
**Expected:** Real-time update on every keystroke; £0 on empty/zero inputs.
**Why human:** `addEventListener('input', ...)` and DOM mutation require a live browser.

#### 3. Comparison Table Horizontal Scroll (Narrow Viewport)

**Test:** Open index.html in a browser. Open DevTools and set viewport to 375px width. Scroll to the "How We Stack Up" section. Verify the table container scrolls horizontally and no column is clipped or layout broken.
**Expected:** Table scrollable inside its container; no overflow breaking the page layout.
**Why human:** `overflow-x: auto` rendering depends on the browser UA stylesheet and viewport.

#### 4. Hero Headline Visual Scale

**Test:** Open index.html in DevTools at 1200px viewport. Inspect the `h1.hero__headline` — computed font-size should be at or above 56px. At 375px viewport, computed font-size should be exactly 56px (clamp minimum 3.5rem at 16px base).
**Expected:** Hero appears visually commanding at desktop; large and bold at mobile.
**Why human:** clamp() computed values are viewport-dependent and require browser DevTools.

---

_Verified: 2026-04-27T12:00:00Z_
_Verifier: Claude (gsd-verifier)_
