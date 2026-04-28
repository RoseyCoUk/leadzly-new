---
phase: 04-conversion-sections
plan: "02"
subsystem: conversion-sections
tags: [comparison-table, dark-section, css-tokens, mobile-scroll]
dependency_graph:
  requires: [04-01]
  provides: [SECT-01]
  affects: [index.html, page.css]
tech_stack:
  added: []
  patterns:
    - overflow-x: auto wrapper for mobile horizontal scroll
    - CSS design token discipline — all colours via var() or rgba()
    - Inline SVG icons for check/cross/partial states
key_files:
  created: []
  modified:
    - index.html
    - page.css
decisions:
  - "comparison__cell--leadzly uses display: table-cell to ensure consistent cell rendering alongside other td elements"
  - "color-primary-highlight used for Leadzly thead cell — green tint visually separates the column header at a glance"
  - ".comparison section-title/subtitle/label have explicit colour overrides — dark background requires white text for readability"
metrics:
  duration_minutes: 8
  completed_date: "2026-04-27"
  tasks_completed: 2
  files_modified: 2
requirements:
  - SECT-01
---

# Phase 4 Plan 02: Comparison Table — Summary

**One-liner:** 8-row comparison table on dark navy background with green Leadzly column using design tokens and mobile overflow-x scroll.

## What Was Built

Replaced the `.comparison` section scaffold placeholder with a fully functional 8-row comparison table (Leadzly vs In-House vs Other Agencies). The section background switched from `var(--section-tint)` to `var(--color-dark)`, giving the table the dark navy treatment specified in the design contract.

### HTML (index.html)

- Removed `<p class="scaffold-placeholder">Comparison table coming in Phase 4.</p>`
- Added `comparison__table-wrap` div containing a `comparison__table` with thead (4 columns: blank label, Leadzly, In-House, Other Agencies) and tbody (8 rows)
- All 8 rows use class `comparison__row` with `comparison__label` for the criterion cell and `comparison__cell--leadzly` for the Leadzly data cell
- Inline SVG check icons (16×16, polyline) in Leadzly cells; cross icons (16×16, line×2) in competitor cells; tilde span for partial/variable cells
- HTML entities used throughout: `&ndash;`, `&amp;`, `&pound;`, `&mdash;`

### CSS (page.css)

- `.comparison` background changed from `var(--section-tint)` to `var(--color-dark)`
- Added white/rgba overrides for `.comparison .section-title`, `.section-subtitle`, `.section-label`
- `.comparison__table-wrap`: `overflow-x: auto` with `-webkit-overflow-scrolling: touch` for mobile horizontal scroll; `border-radius: var(--radius-xl)`; border uses `var(--color-dark-border)`
- `.comparison__table`: `min-width: 560px` ensures mobile scrolls rather than breaks; `border-collapse: collapse`
- Alternating row backgrounds: odd rows `var(--color-dark)`, even rows `var(--color-dark-surface)`
- `.comparison__th--leadzly`: `var(--color-primary-highlight)` background, `var(--color-primary)` text — visually distinct column header
- `.comparison__cell--leadzly`: `var(--color-primary-soft)` background, `var(--color-primary)` text, `font-weight: 700`
- Non-Leadzly body cells: `rgba(255, 255, 255, 0.65)` text on dark row backgrounds
- `.comparison__check`: green `var(--color-primary)`, inline-flex; `.comparison__cross`: muted `rgba(255,255,255,0.4)`; `.comparison__partial`: `rgba(255,255,255,0.65)`
- Zero hardcoded hex values — all colours use design tokens or rgba()

## Verification Results

| Check | Result |
|-------|--------|
| `grep -c "comparison__row" index.html` | 8 |
| `grep -c "comparison__table-wrap" index.html` | 1 |
| `grep "Comparison table coming" index.html` | No match (placeholder removed) |
| `.comparison background` in page.css | `var(--color-dark)` |
| `comparison__th--leadzly` with `color-primary-highlight` | Present |
| `overflow-x: auto` on table-wrap | Present |
| Hardcoded hex in comparison rules | None |

## Commits

| Task | Commit | Description |
|------|--------|-------------|
| Task 1 | a30ec8a | feat(04-02): replace comparison scaffold with full 8-row table HTML |
| Task 2 | 4132b10 | feat(04-02): add comparison table CSS and switch .comparison to dark navy |

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all 8 rows contain real content (pricing, setup time, guarantees, team location, channels, reporting, contracts, money-back guarantee). No placeholder or "coming soon" text remains in the comparison section.

## Self-Check: PASSED

- index.html exists and contains `comparison__table-wrap`: confirmed
- page.css exists and contains `comparison__table` CSS block: confirmed  
- Commits a30ec8a and 4132b10 exist in git log: confirmed
- SECT-01 requirement satisfied: 8-row comparison table fully rendered on dark navy background with green Leadzly column
