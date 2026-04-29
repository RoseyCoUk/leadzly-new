---
phase: 08-structured-data
plan: 02
subsystem: seo
tags: [json-ld, structured-data, schema-org, faqpage, service]

# Dependency graph
requires:
  - phase: 08-structured-data/01
    provides: Organization + LocalBusiness JSON-LD block in index.html head (SCHEMA-01)
provides:
  - FAQPage JSON-LD block with 7 Question/Answer nodes in index.html head (SCHEMA-02)
  - Service JSON-LD block with @graph array of 4 Service nodes in index.html head (SCHEMA-03)
affects: [phase-09-performance]

# Tech tracking
tech-stack:
  added: []
  patterns: [FAQPage JSON-LD with mainEntity array; Service JSON-LD via @graph; HTML entities decoded to Unicode in JSON values]

key-files:
  created: []
  modified: [index.html]

key-decisions:
  - "FAQPage question names match visible button span text verbatim (after entity decoding)"
  - "All HTML entities decoded to Unicode before inserting into JSON: &ndash; to en-dash, &mdash; to em-dash, &rsquo; to right single quote, &ldquo;/&rdquo; to curly double quotes"
  - "Service block uses @graph array (not separate scripts) — groups all 4 services under one context"
  - "4th service (Sales Enablement) included because it is visible on the page — plan note confirmed this was intentional"
  - "Service descriptions match .service-card__desc text exactly — em-dashes are Unicode, not HTML entities"

patterns-established:
  - "FAQPage: mainEntity array with @type Question nodes, each with name + acceptedAnswer.text"
  - "Service: @graph array groups multiple Service nodes under one script block"
  - "JSON-LD: All text values are plain Unicode — no HTML entities permitted in JSON string values"

requirements-completed: [SCHEMA-02, SCHEMA-03]

# Metrics
duration: 8min
completed: 2026-04-29
---

# Phase 8 Plan 02: Structured Data Summary

**FAQPage JSON-LD (7 Q&A pairs) and Service JSON-LD (@graph of 4 services) inserted into index.html head, completing all three SCHEMA-01/02/03 requirements for Phase 8**

## Performance

- **Duration:** 8 min
- **Started:** 2026-04-29T00:35:00Z
- **Completed:** 2026-04-29T00:43:00Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments

- Inserted FAQPage JSON-LD block with 7 Question nodes — all HTML entities decoded to Unicode for valid JSON
- Inserted Service JSON-LD block with @graph array of 4 Service nodes matching all visible service cards on page
- All three JSON-LD blocks (SCHEMA-01, SCHEMA-02, SCHEMA-03) now present in index.html head — zero HTML entities in any block
- Phase 8 structured data complete: ready for Google Rich Results Test after deployment

## Task Commits

Each task was committed atomically:

1. **Task 1: Insert FAQPage JSON-LD block (7 Q&A pairs) into index.html head** - `f8e17ff` (feat)
2. **Task 2: Insert Service JSON-LD block (4 services) into index.html head** - `502d2a8` (feat)

**Plan metadata:** (docs commit follows)

## Files Created/Modified

- `index.html` - Added FAQPage block (lines ~151–215) and Service block (lines ~216–259), both before `</head>`

## Decisions Made

- FAQPage `name` fields match the visible `<span>` text inside each FAQ button verbatim (after entity decoding)
- Service `description` values match `.service-card__desc` paragraph text exactly — em-dashes are Unicode `—`, not `&mdash;`
- 4th Service node (Sales Enablement) included: the plan explicitly noted to include it because it is visible on the page
- `provider.name` set to `"Leadzly"` on all four Service nodes (not "Elevateo Co")
- Service block uses a single @graph array rather than separate script blocks — groups related types cleanly

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- SCHEMA-01, SCHEMA-02, SCHEMA-03 all satisfied — Phase 8 structured data complete
- All three JSON-LD blocks validated with no HTML entities (Node.js check passed)
- Ready for Google Rich Results Test at https://search.google.com/test/rich-results after deployment
- Phase 9 (Performance & Core Web Vitals) can proceed: font weight trim, SVG lazy-loading, CLS fixes, CSS minification

## Known Stubs

None — all three JSON-LD blocks are fully wired with real values from the live page. No placeholder text, no hardcoded empty values.

---
*Phase: 08-structured-data*
*Completed: 2026-04-29*
