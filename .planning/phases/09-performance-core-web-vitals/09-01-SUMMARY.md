---
phase: 09-performance-core-web-vitals
plan: 01
subsystem: ui
tags: [performance, core-web-vitals, google-fonts, svg, cls, lazy-loading, css]

# Dependency graph
requires:
  - phase: 08-structured-data
    provides: Completed JSON-LD blocks in index.html head — base HTML ready for performance work
provides:
  - Google Fonts URL trimmed to 7 weight variants (PJS 400/600/700/800, Inter 400/600/700)
  - outreach-sequence.svg and pipeline-board.svg as standalone external files
  - index.html referencing both SVGs via lazy-loaded img tags with explicit dimensions
  - Calendly container pre-reserves 700px desktop / 650px mobile via min-height
affects: [10-css-minification, future-performance-audits]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "External SVG files served as lazy-loaded img tags (not inline) for off-screen decorative graphics"
    - "Google Fonts URL uses only loaded weight variants — no unused weights requested"
    - "Calendly embed container uses min-height (not height) to pre-reserve space — allows widget expansion"

key-files:
  created:
    - outreach-sequence.svg
    - pipeline-board.svg
  modified:
    - index.html
    - page.css

key-decisions:
  - "Font weight 500 → 600 for all Inter and PJS rules — nearest loaded weight above 500, avoids browser synthetic bold fallback"
  - "SVG img tags use alt='' aria-hidden=true — decorative graphics, suppresses screen reader announcement"
  - "min-height not height on Calendly container — allows widget to expand if Calendly injects taller iframe"
  - "CSS selectors updated from .how__visual svg / .channels__visual svg to img — identical visual rules retained"

patterns-established:
  - "Decorative graphics externalised as SVG files, lazy-loaded via img tag with explicit width/height to prevent CLS"

requirements-completed: [PERF-01, PERF-02, PERF-03]

# Metrics
duration: 15min
completed: 2026-04-29
---

# Phase 09 Plan 01: Performance — Font Trim, SVG Lazy-Loading, Calendly CLS Fix Summary

**Google Fonts trimmed to 7 weight variants, both inline SVG mockups externalised as lazy-loaded img files (~320 lines removed from HTML), and Calendly container pre-reserves 700px/650px to eliminate layout shift**

## Performance

- **Duration:** ~15 min
- **Started:** 2026-04-29T01:39:29Z
- **Completed:** 2026-04-29T01:46:34Z
- **Tasks:** 3
- **Files modified:** 4 (index.html, page.css, outreach-sequence.svg created, pipeline-board.svg created)

## Accomplishments

- Removed Inter weight 500 and PJS weight 500 from Google Fonts URL — browser now downloads 7 font variants instead of 9; updated 5 CSS rules from font-weight: 500 to 600 (nearest loaded weight)
- Extracted both inline SVG mockups (~320 lines) into standalone files at project root; replaced with `<img loading="lazy" width="..." height="...">` tags — reduces HTML parse payload and defers off-screen resource fetching
- Added `min-height: 700px` to `.booking__calendar-wrap` (desktop) and `min-height: 650px` inside `@media (max-width: 767px)` — browser pre-allocates space before Calendly iframe loads, eliminating measurable CLS in booking section

## Task Commits

Each task was committed atomically:

1. **Task 1: Trim Google Fonts URL and update font-weight 500 CSS rules** - `6815e64` (feat)
2. **Task 2: Externalise inline SVGs and replace with lazy-loaded img tags** - `9d05c36` (feat)
3. **Task 3: Add Calendly container min-height to prevent layout shift** - `1780e83` (feat)

## Files Created/Modified

- `index.html` - Updated Google Fonts link (weight 500 removed); replaced two inline SVG blocks with lazy-loaded img tags
- `page.css` - Updated 5 font-weight: 500 rules to 600; updated .how__visual and .channels__visual selectors from svg to img; added min-height to .booking__calendar-wrap
- `outreach-sequence.svg` - New standalone SVG file (viewBox 0 0 480 520), outreach sequence UI mockup
- `pipeline-board.svg` - New standalone SVG file (viewBox 0 0 640 500), pipeline CRM board mockup

## Decisions Made

- Font weight 500 upgraded to 600 (not downgraded to 400) — 600 is the nearest loaded weight above 500, preserving intended visual emphasis rather than losing it
- SVG img tags use `alt="" aria-hidden="true"` — decorative graphics convey no information to screen reader users
- `min-height` used on Calendly container (not `height`) — allows Calendly iframe to grow if future widget versions render taller content
- CSS selector change from `.how__visual svg` to `.how__visual img` kept inside the existing `@media (min-width: 900px)` block — visual show/hide logic unchanged

## Deviations from Plan

None — plan executed exactly as written. The `.how__visual svg` rule was confirmed to be inside a `@media (min-width: 900px)` block (not at root scope as implied by plan line references), but the selector change was applied correctly within that block.

## Issues Encountered

- The `.how__visual svg` CSS rule was nested inside `@media (min-width: 900px)` rather than at root scope — the plan's line number references (630–636) were accurate but the media query context required editing `  .how__visual svg {` (indented) not `.how__visual svg {` (root). Handled automatically by reading the file before editing.

## User Setup Required

None — no external service configuration required.

## Next Phase Readiness

- PERF-01, PERF-02, PERF-03 complete — three of five performance requirements done
- Remaining: PERF-04 (floating CTA CLS fix) and PERF-05 (page.css minification)
- Both SVG files deployed alongside index.html via Vercel static asset serving — no configuration needed

---
*Phase: 09-performance-core-web-vitals*
*Completed: 2026-04-29*
