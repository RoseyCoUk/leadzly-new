---
phase: 05-booking-experience-ctas
plan: "03"
subsystem: cta-copy
tags: [cta, copy, conversion, roi-calculator]
dependency_graph:
  requires: [05-02]
  provides: [varied-cta-copy, roi-cta-button]
  affects: [index.html, page.css]
tech_stack:
  added: []
  patterns: [inline-anchor-scroll, css-utility-class]
key_files:
  created: []
  modified:
    - index.html
    - page.css
decisions:
  - "Hero secondary CTA changed from #services to #booking — reduces friction by sending mid-page visitors directly to the calendar embed"
  - "All three pricing card CTAs unified as 'Start Filling Your Calendar' — consistent intent signal at the bottom of the pricing decision"
  - "ROI CTA uses .roi__cta wrapper div for layout isolation — margin-top applied via CSS token, not inline style"
metrics:
  duration_minutes: 5
  completed: "2026-04-28T15:36:01Z"
  tasks_completed: 2
  tasks_total: 2
  files_changed: 2
---

# Phase 5 Plan 03: CTA Copy Variants Summary

4 distinct CTA copy variants deployed across hero, pricing, and ROI sections with ROI section gaining its own conversion button.

## What Was Built

- **Hero primary CTA:** "Book a Free Strategy Call" (added "Free" — removes cost anxiety barrier)
- **Hero secondary CTA:** "See If We're a Fit" linking to `#booking` (was "How It Works" linking to `#services`)
- **All three pricing cards:** "Start Filling Your Calendar" (replaced "Get Started" / "Book a Call")
- **ROI calculator section:** New "Get Your Meetings Forecast" button below disclaimer, linking to `#booking`
- **page.css:** `.roi__cta` rule added for centred layout with `--space-8` top margin

## Task Outcomes

| Task | Name | Commit | Files |
|------|------|--------|-------|
| 1 | Update hero CTAs and all three pricing card CTAs | 8c16025 | index.html |
| 2 | Add ROI CTA button and styles | dd467bd | index.html, page.css |

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — all CTAs link to live anchors (`#booking` for secondary CTAs; Calendly URL for booking CTAs).

## Self-Check

Files exist:
- index.html — FOUND
- page.css — FOUND

Commits exist:
- 8c16025 — FOUND
- dd467bd — FOUND

## Self-Check: PASSED
