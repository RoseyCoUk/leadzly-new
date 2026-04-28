---
phase: 05-booking-experience-ctas
plan: 02
subsystem: floating-cta
tags: [cta, floating, sticky, animation, css, vanilla-js]
dependency_graph:
  requires: [05-01]
  provides: [BOOK-02]
  affects: [index.html, page.css]
tech_stack:
  added: []
  patterns: [fixed-position sticky CTA, CSS transition reveal, pulse-ring keyframe animation, JS IIFE setTimeout]
key_files:
  created: []
  modified:
    - index.html
    - page.css
key_decisions:
  - Floating CTA hidden via opacity 0 + translateY(16px) + pointer-events none — CSS transition reveal on --visible modifier class
  - 3000ms setTimeout in JS IIFE adds floating-cta--visible class — no scroll trigger needed
  - display none at max-width 639px — completely removes from layout on mobile, no overlap risk
  - pulse-ring uses inset -4px on ::before pseudo-element — ring expands beyond dot without affecting layout
metrics:
  duration_minutes: 5
  completed_date: "2026-04-28"
  tasks_completed: 2
  tasks_total: 2
  files_modified: 2
---

# Phase 05 Plan 02: Floating Sticky CTA Summary

**One-liner:** Fixed bottom-right "Book a Call" button with 3s entrance delay, CSS opacity/transform transition, and continuously pulsing green dot via keyframe animation.

## What Was Built

A floating sticky CTA button fixed to the bottom-right corner of the viewport:

- **Hidden state:** `opacity: 0`, `transform: translateY(16px)`, `pointer-events: none` — invisible and non-interactive on page load
- **Visible state:** JS IIFE `setTimeout(3000)` adds `.floating-cta--visible` → `opacity: 1`, `translateY(0)`, `pointer-events: auto` — slides up smoothly
- **Green pulse dot:** `::before` pseudo-element with `pulse-ring` keyframe animation — scales from 1x to 1.5x at 50% with opacity fade, loops every 2s
- **Mobile hide:** `@media (max-width: 639px)` sets `display: none` — completely removed from layout below 640px

## Tasks Completed

| Task | Description | Commit |
|------|-------------|--------|
| 1 | Add floating CTA HTML + JS IIFE to index.html | 2d01d4a |
| 2 | Add floating CTA CSS to page.css | eaaaca6 |

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

None — button links to live Calendly URL `https://calendly.com/allan-chan-leadzly`.

## Self-Check: PASSED

- index.html: floating-cta div present with correct id, role, aria-label, dot span, Calendly href
- index.html: JS IIFE with setTimeout 3000ms and classList.add('floating-cta--visible')
- page.css: .floating-cta base state with position fixed, bottom 24px, right 24px, z-index 9999, opacity 0, pointer-events none
- page.css: .floating-cta--visible modifier with opacity 1, translateY(0), pointer-events auto
- page.css: @keyframes pulse-ring defined and referenced in .floating-cta__dot::before
- page.css: @media (max-width: 639px) hides floating-cta entirely
