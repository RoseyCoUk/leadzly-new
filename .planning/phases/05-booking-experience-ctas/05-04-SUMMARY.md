---
phase: 05-booking-experience-ctas
plan: "04"
subsystem: ui
tags: [intersection-observer, animation, vanilla-js, hero, counter]

# Dependency graph
requires:
  - phase: 05-03
    provides: CTA copy variants across hero, pricing, and ROI sections
provides:
  - Hero stat counter animation — three numbers (12, 94%, 8) count up from 0 on first scroll using IntersectionObserver + requestAnimationFrame ease-out cubic
affects:
  - 06-design-polish-mobile

# Tech tracking
tech-stack:
  added: []
  patterns:
    - IntersectionObserver one-shot pattern with animated flag + disconnect() guard
    - ease-out cubic easing via 1 - Math.pow(1 - progress, 3) in rAF step loop
    - data-count-to / data-count-suffix data attributes as declarative animation targets

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "Hero stat counter uses animated boolean flag + observer.disconnect() to guarantee the animation plays exactly once per page load — re-scrolling does not replay"
  - "ease-out cubic (1 - Math.pow(1 - progress, 3)) chosen for deceleration effect that feels live and data-driven"
  - "Graceful fallback: if IntersectionObserver not supported, numbers stay at their final HTML values (no JS required for readability)"
  - "Counter IIFE added as final script block before </body> — isolated from existing nav/cookie/FAQ/ROI scripts"

patterns-established:
  - "data-count-to pattern: declarative HTML attributes drive JS animation targets — no hardcoded values in scripts"

requirements-completed:
  - QWIN-03

# Metrics
duration: 1min
completed: "2026-04-28"
---

# Phase 5 Plan 04: Hero Stat Counter Animation Summary

**Hero card numbers (12, 94%, 8) count up from 0 on first scroll-into-view using IntersectionObserver + requestAnimationFrame ease-out cubic over 1200ms**

## Performance

- **Duration:** ~1 min
- **Started:** 2026-04-28T15:38:30Z
- **Completed:** 2026-04-28T15:39:29Z
- **Tasks:** 1 of 2 auto tasks complete (Task 2 is a human-verify checkpoint)
- **Files modified:** 1

## Accomplishments

- Added `data-count-to` attributes to all three `.hero__card-stat-num` spans (12, 94%, 8) with `data-count-suffix="%"` on the percentage stat
- Implemented counter IIFE using IntersectionObserver at 0.5 threshold — fires once when hero card is 50% in view
- Animation runs 0 → target over 1200ms with ease-out cubic easing curve
- One-shot guard ensures animation plays exactly once per page load (animated flag + observer.disconnect())
- Graceful fallback: numbers display at final values if IntersectionObserver is unavailable (no JS dependency for readability)

## Task Commits

Each task was committed atomically:

1. **Task 1: Add data-count-to attributes to hero stat spans and add counter JS IIFE** - `faa8f95` (feat)

**Plan metadata:** (pending final docs commit)

## Files Created/Modified

- `index.html` - Added data-count-to/data-count-suffix attributes on three hero stat spans; added hero stat counter IIFE script block before </body>

## Decisions Made

- Hero stat counter uses `animated` boolean flag + `observer.disconnect()` to guarantee the animation plays exactly once per page load
- ease-out cubic easing (`1 - Math.pow(1 - progress, 3)`) chosen for natural deceleration that reads as live data
- Counter IIFE added as final isolated script block — no mixing with existing nav/cookie/FAQ/ROI scripts

## Deviations from Plan

None — plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness

- Phase 5 all four implementation plans complete (Plans 01–04): inline Calendly embed, floating sticky CTA, CTA copy variants, hero stat counter animation
- Awaiting human verification checkpoint (Task 2) to confirm all Phase 5 deliverables visually
- After verification approval, Phase 6 (Design Polish & Mobile) is ready to begin

---
*Phase: 05-booking-experience-ctas*
*Completed: 2026-04-28*
