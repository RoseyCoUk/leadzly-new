---
phase: 04-conversion-sections
plan: 04
subsystem: conversion-sections
tags: [roi-calculator, comparison-table, faq, css, vanilla-js, human-verification]
dependency_graph:
  requires: [04-01, 04-02, 04-03]
  provides: [SECT-03, DSGN-01]
  affects: [page.css, index.html]
tech_stack:
  added: []
  patterns: [vanilla-js-iife, css-grid, number-input-realtime, accordion-max-height-transition]
key_files:
  created: []
  modified:
    - path: index.html
      role: ROI calculator HTML section + JS IIFE between .faq and .booking
    - path: page.css
      role: All CSS for .roi*, .comparison (light theme), .faq (refined UX)
decisions:
  - "ROI calculator uses 10% meeting-to-close conversion rate as industry benchmark assumption"
  - "Comparison table switched from dark navy to light green-tinted (--color-primary-soft) background — consistent with page light theme"
  - "FAQ answer transition upgraded to 0.4s ease with 600px max-height ceiling for smoother feel"
  - "ROI inputs given explicit height: 48px to ensure cross-browser rendering parity"
metrics:
  duration: "Phase 4 Plan 4 — Tasks 1-2 prior session; post-verification fixes this session"
  completed_date: "2026-04-28"
  tasks_completed: 3
  files_modified: 2
---

# Phase 4 Plan 4: ROI Calculator + Phase 4 Visual Verification Summary

**One-liner:** ROI calculator (3 inputs, 2 live outputs, £8k/£96k defaults) with real-time JS formula; post-verification fixes for comparison table light theme, FAQ animation smoothness, and input height consistency.

---

## Tasks Completed

| # | Name | Commit | Files |
|---|------|--------|-------|
| 1 | Insert ROI calculator HTML section and JS IIFE | 3533d04 | index.html |
| 2 | Add ROI calculator CSS to page.css | 4349a18 | page.css |
| 3 | Human visual verification + post-review fixes | 74a76f7 | page.css |

---

## What Was Built

### ROI Calculator (SECT-03)

A three-input, two-output real-time calculator embedded between the FAQ and Booking sections:

- **Inputs:** Average deal value (£), Current meetings/month, Target meetings/month
- **Formula:** `monthly = Math.max(0, target - current) * dealValue * 0.10`; `annual = monthly * 12`
- **Default outputs:** £8,000/month, £96,000/year (deal=5000, current=4, target=20)
- **Behaviour:** Instantly recalculates on any `input` event; empty/zero inputs produce £0 not NaN
- **Locale formatting:** `toLocaleString('en-GB')` for proper UK number formatting

### Post-Verification Fixes

Three issues flagged during human visual review and fixed:

1. **Comparison table — light theme** — Removed `background: var(--color-dark)`, replaced with `var(--color-primary-soft)` (light green). Removed all white text overrides (`color-text-inverse`, `rgba(255,255,255,...)`) — reverted to standard `var(--color-text)` / `var(--color-text-muted)` / `var(--color-text-faint)`. Table borders changed from `--color-dark-border` to `--color-border`. Row zebra striping changed to `--color-bg` / `--color-surface`.

2. **FAQ accordion — three UX refinements** — Font-size reduced from `var(--text-lg)` to `0.95rem` for cleaner question text. Animation transition changed from `300ms cubic-bezier(0.16, 1, 0.3, 1)` to `0.4s ease` for perceptibly smoother expansion. Max-height ceiling raised from 400px to 600px to prevent jarring pause at animation end. Answer padding changed from `0 0 1rem 0` to `0 1.5rem 1.25rem 1.5rem` so text doesn't hug the left edge.

3. **ROI calculator inputs — equal height** — Added `height: 48px`, changed `padding` from `var(--space-3) var(--space-4)` to `0 var(--space-4)` (vertical padding handled by height), added `vertical-align: middle`. This ensures all three number inputs render at identical height across browsers.

---

## Deviations from Plan

### Auto-fixed Issues

None — the plan tasks executed as written.

### Post-Checkpoint Fixes (human-verify feedback)

These were not deviations from the original plan tasks — they were corrections requested after human visual review of the completed checkpoint.

**1. [User feedback] Comparison table dark background rejected**
- **Found during:** Task 3 (human visual verification)
- **Issue:** Dark navy background on .comparison section was visually jarring and inconsistent with the rest of the light-themed page
- **Fix:** Reverted to `var(--color-primary-soft)` background; removed all white text overrides from section-title, section-subtitle, section-label, table cells
- **Commit:** 74a76f7

**2. [User feedback] FAQ accordion UX refinements**
- **Found during:** Task 3 (human visual verification)
- **Issues:** Question text too large; animation too abrupt; answer content hugging left edge
- **Fix:** font-size → 0.95rem; transition → 0.4s ease; max-height → 600px; answer padding → 0 1.5rem 1.25rem 1.5rem
- **Commit:** 74a76f7

**3. [User feedback] ROI inputs unequal height**
- **Found during:** Task 3 (human visual verification)
- **Issue:** Three number inputs rendering at different heights due to browser default UA stylesheets
- **Fix:** Explicit `height: 48px`, adjusted padding, `vertical-align: middle`, `box-sizing: border-box`
- **Commit:** 74a76f7

---

## Known Stubs

None — all sections are fully wired with design tokens and real content.

---

## Phase 4 Complete

All four plans for Phase 04-conversion-sections are complete:

| Plan | Requirement | Description | Status |
|------|-------------|-------------|--------|
| 04-01 | DSGN-01 | Hero h1 clamp raised; .section-label font-weight 700 | Done |
| 04-02 | SECT-01 | Comparison table — 8-row, Leadzly column highlighted | Done |
| 04-03 | SECT-02 | FAQ accordion — 7 items, accessible, animated | Done |
| 04-04 | SECT-03 | ROI calculator — 3 inputs, live outputs, £8k/£96k defaults | Done |

---

## Self-Check: PASSED

- [x] `page.css` modified — comparison light theme, FAQ refinements, ROI input height
- [x] `index.html` has `roi__card`, `roi-deal-value`, `roi-monthly`, `roi-annual`, `CONVERSION_RATE = 0.10`
- [x] Commits 3533d04, 4349a18, 74a76f7 exist in git log
- [x] No dark background remains on .comparison section
- [x] FAQ answer padding includes left-side padding (1.5rem)
- [x] ROI inputs have `height: 48px`
