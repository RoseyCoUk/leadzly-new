---
phase: 05-booking-experience-ctas
plan: "04"
subsystem: ui
tags: [intersection-observer, animation, vanilla-js, hero, counter, floating-cta, bug-fix]

# Dependency graph
requires:
  - phase: 05-03
    provides: CTA copy variants across hero, pricing, and ROI sections
provides:
  - Hero stat counter animation — three numbers (12, 94%, 8) count up from 0 on first scroll using IntersectionObserver + requestAnimationFrame ease-out cubic
  - Floating sticky CTA fixed in main branch — HTML, JS IIFE, and CSS now present on the deployed branch
affects:
  - 06-design-polish-mobile

# Tech tracking
tech-stack:
  added: []
  patterns:
    - IntersectionObserver one-shot pattern with animated flag + disconnect() guard
    - ease-out cubic easing via 1 - Math.pow(1 - progress, 3) in rAF step loop
    - data-count-to / data-count-suffix data attributes as declarative animation targets
    - CSS opacity + translateY transition toggled by JS setTimeout 3000ms for floating CTA reveal

key-files:
  created: []
  modified:
    - index.html
    - page.css

key-decisions:
  - "Hero stat counter uses animated boolean flag + observer.disconnect() to guarantee the animation plays exactly once per page load — re-scrolling does not replay"
  - "ease-out cubic (1 - Math.pow(1 - progress, 3)) chosen for deceleration effect that feels live and data-driven"
  - "Graceful fallback: if IntersectionObserver not supported, numbers stay at their final HTML values (no JS required for readability)"
  - "Counter IIFE added as final script block before </body> — isolated from existing nav/cookie/FAQ/ROI scripts"
  - "Floating CTA re-implemented directly on main branch — original 05-02 feature commits (eaaaca6, 2d01d4a) were on a separate worktree branch that was never merged to main"

patterns-established:
  - "data-count-to pattern: declarative HTML attributes drive JS animation targets — no hardcoded values in scripts"

requirements-completed:
  - QWIN-03

# Metrics
duration: ~10min (including bug investigation and fix)
completed: "2026-04-28"
---

# Phase 5 Plan 04: Hero Stat Counter Animation Summary

**Hero card numbers (12, 94%, 8) count up from 0 on first scroll-into-view; floating sticky CTA fixed and now live on main branch**

## Performance

- **Duration:** ~10 min (including worktree branch investigation and floating CTA fix)
- **Completed:** 2026-04-28
- **Tasks:** 2 of 2 complete (Task 1 auto; Task 2 human-verify — approved after fix)
- **Files modified:** 2 (index.html, page.css)

## Accomplishments

- Added `data-count-to` attributes to all three `.hero__card-stat-num` spans (12, 94%, 8) with `data-count-suffix="%"` on the percentage stat
- Implemented counter IIFE using IntersectionObserver at 0.5 threshold — fires once when hero card is 50% in view
- Animation runs 0 → target over 1200ms with ease-out cubic easing curve
- One-shot guard ensures animation plays exactly once per page load (animated flag + observer.disconnect())
- Graceful fallback: numbers display at final values if IntersectionObserver is unavailable (no JS dependency for readability)
- Fixed floating sticky CTA missing from main: added HTML (floating-cta div + IIFE), JS setTimeout 3000ms reveal, and full CSS (fixed position, opacity transition, pulse-ring animation, mobile hide)

## Task Commits

Each task was committed atomically:

1. **Task 1: Add data-count-to attributes to hero stat spans and add counter JS IIFE** - `faa8f95` (feat)
2. **Fix: Floating sticky CTA missing from main branch** - `09b6cf6` (fix)

## Files Created/Modified

- `index.html` — Added data-count-to/data-count-suffix attributes on three hero stat spans; counter IIFE script block; floating CTA div + reveal IIFE
- `page.css` — Added `.floating-cta` base, `.floating-cta--visible` modifier, pulse-ring keyframe, mobile hide rule

## Decisions Made

- Hero stat counter uses `animated` boolean flag + `observer.disconnect()` to guarantee the animation plays exactly once per page load
- ease-out cubic easing (`1 - Math.pow(1 - progress, 3)`) chosen for natural deceleration that reads as live data
- Counter IIFE added as final isolated script block — no mixing with existing nav/cookie/FAQ/ROI scripts
- Floating CTA dot uses white background with rgba white pulse ring (against primary-green button background) for contrast

## Deviations from Plan

### Auto-fixed Issues

**1. [Rule 1 - Bug] Floating sticky CTA missing from main branch**
- **Found during:** Task 2 human-verify checkpoint (user reported CTA not visible)
- **Issue:** Phase 05-02 feature commits (eaaaca6, 2d01d4a — HTML/JS and CSS) were committed to worktree branch `worktree-agent-a39a37cd` but were never merged into `main`. The docs commit (f45c156) was on main but the feature commits were not, so the floating CTA was absent from the live page.
- **Fix:** Re-implemented the floating CTA directly on main: added HTML div + JS IIFE to index.html and full CSS rules (position fixed, transition, pulse-ring animation, mobile display:none) to page.css
- **Files modified:** index.html, page.css
- **Commit:** `09b6cf6`

## Issues Encountered

- Worktree branch isolation — 05-02 feature commits never merged to main. Investigation required checking `git log --oneline --all`, `git branch --contains`, and `git show main:index.html` to diagnose. Root cause: the orchestrator merged the docs commit but not the feature commits from the prior worktree's branch.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness

- Phase 5 all four implementation plans complete and verified (Plans 01–04): inline Calendly embed, floating sticky CTA (now fixed on main), CTA copy variants, hero stat counter animation
- Human verification checkpoint approved — Phase 5 complete
- Phase 6 (Design Polish & Mobile) is ready to begin

## Self-Check: PASSED

- `index.html` contains floating-cta div and both IIFE script blocks
- `page.css` contains .floating-cta and .floating-cta--visible rules
- Commits faa8f95 (feat) and 09b6cf6 (fix) exist on main branch

---
*Phase: 05-booking-experience-ctas*
*Completed: 2026-04-28*
