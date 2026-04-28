---
phase: 05-booking-experience-ctas
plan: 01
subsystem: booking-section
tags: [calendly, booking, two-column-grid, conversion, cta]
dependency_graph:
  requires: []
  provides: [booking-section-live-calendar, calendly-cdn-loaded]
  affects: [index.html, page.css]
tech_stack:
  added: [Calendly inline widget CDN]
  patterns: [CSS Grid two-column layout, responsive stack]
key_files:
  created: []
  modified:
    - index.html
    - page.css
decisions:
  - "Calendly CDN loaded once in <head> — widget.css link + widget.js async script"
  - "Booking grid uses 1fr/1.2fr split — calendar column slightly wider to accommodate embed"
  - "Calendly height 700px desktop / 650px mobile — eliminates internal scrollbar in widget"
  - "Mobile breakpoint at 767px — grid collapses to single column, benefits stack above calendar"
metrics:
  duration: 73s
  completed: "2026-04-28"
  tasks_completed: 2
  files_modified: 2
---

# Phase 05 Plan 01: Booking Section Two-Column Grid Summary

**One-liner:** Live Calendly inline embed in two-column booking grid — benefits left, calendar right — loaded from CDN with mobile responsive stack.

## Tasks Completed

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Add Calendly CDN + replace booking scaffold with two-column HTML | dd80f5c | index.html |
| 2 | Add booking grid CSS and benefit list styles | c59e0ac | page.css |

## What Was Built

### Task 1 — HTML (index.html)

- Replaced `scaffold-placeholder` paragraph in the `#booking` section with a full `booking__grid` div
- Left column (`booking__content`): heading "What to expect on the call", 5 benefit list items with inline SVG checkmarks, reassurance text "No hard sell. No obligation."
- Right column (`booking__calendar-wrap`): `calendly-inline-widget` div with `data-url="https://calendly.com/allan-chan-leadzly"` and `style="min-width:320px;height:700px;"`
- Added Calendly widget.css and widget.js CDN links in `<head>` (exactly once each)

### Task 2 — CSS (page.css)

- `.booking__grid`: CSS Grid, `grid-template-columns: 1fr 1.2fr`, gap, align-items start
- `.booking__subheading`, `.booking__benefits`, `.booking__benefit`: benefit list layout using flex column
- `.booking__benefit-icon`: green primary colour, flex-shrink 0, 2px top margin for alignment
- `.booking__reassurance`: small muted text
- `.booking__calendar-wrap`: border-radius xl, overflow hidden (prevents widget border bleed)
- `@media (max-width: 767px)`: single column, Calendly height 650px override

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — Calendly widget loads live calendar from `https://calendly.com/allan-chan-leadzly`. No stub data.

## Self-Check: PASSED

- FOUND: index.html
- FOUND: page.css
- FOUND: .planning/phases/05-booking-experience-ctas/05-01-SUMMARY.md
- FOUND commit: dd80f5c (Task 1)
- FOUND commit: c59e0ac (Task 2)
