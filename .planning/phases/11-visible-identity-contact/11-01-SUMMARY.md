---
phase: 11-visible-identity-contact
plan: 01
subsystem: ui
tags: [html, nap, eeat, seo, footer, about]

# Dependency graph
requires:
  - phase: 10-schema-authority-layer
    provides: LocalBusiness JSON-LD with full address and telephone already declared in structured data
provides:
  - Footer Connect column with real phone tel: link, postal address as <address> element, and LinkedIn href
  - About section founder byline paragraph linking to Allan Chan's LinkedIn profile
affects: [12-aeo-copy-optimisation]

# Tech tracking
tech-stack:
  added: []
  patterns: []

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "No new CSS added — address block and founder byline inherit existing footer/paragraph styles"
  - "Postal address wrapped in <address> semantic element for browser/crawler recognition"
  - "about__founder and about__founder-link classes applied for future CSS targeting without breaking existing styles"

patterns-established:
  - "NAP on-page text matches Phase 10 JSON-LD exactly — same address, same phone format (+13074009814)"

requirements-completed: [NAP-01, NAP-02, EEAT-01]

# Metrics
duration: 1min
completed: 2026-04-29
---

# Phase 11 Plan 01: Visible Identity & Contact Summary

**Footer NAP block (postal address + real phone) and About founder byline (Allan Chan + LinkedIn) added as visible on-page text matching Phase 10 JSON-LD declarations**

## Performance

- **Duration:** ~1 min
- **Started:** 2026-04-29T07:22:59Z
- **Completed:** 2026-04-29T07:23:52Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments

- Replaced placeholder `tel:+44000000000` with real number `tel:+13074009814` in footer Connect column
- Fixed footer LinkedIn link from `href="#"` to Allan Chan's actual LinkedIn profile URL
- Added `<address class="footer__address">` block with full postal address (1 Hollycroft Avenue, Belfast, BT5 5JE, United Kingdom) after the Connect column's `</ul>`
- Inserted `<p class="about__founder">` byline after the `about__text` paragraph, naming Allan Chan with a clickable LinkedIn link

## Task Commits

Each task was committed atomically:

1. **Task 1: Add address block and fix phone/LinkedIn in footer Connect column** - `208abbe` (feat)
2. **Task 2: Add Allan Chan founder byline to About section** - `9cd7cc0` (feat)

**Plan metadata:** TBD (docs: complete plan)

## Files Created/Modified

- `index.html` - Footer Connect column updated (phone, LinkedIn, address block) + About section founder byline added

## Decisions Made

- No new CSS added — address block and founder byline inherit existing footer and paragraph styles respectively (plan constraint)
- `<address>` semantic element used for postal address — correct HTML5 semantic for contact information, recognised by crawlers
- `about__founder` and `about__founder-link` class names applied for future CSS targeting capability without requiring immediate CSS changes

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Phase 12 (AEO Copy Optimisation) can now proceed — visible NAP and founder identity are in place
- Consistent NAP across JSON-LD (Phase 10) and visible page text (this plan) — structured data + body content alignment complete
- No blockers

---
*Phase: 11-visible-identity-contact*
*Completed: 2026-04-29*
