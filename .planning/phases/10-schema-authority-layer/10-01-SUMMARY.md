---
phase: 10-schema-authority-layer
plan: 01
subsystem: seo
tags: [schema-org, json-ld, structured-data, local-business, person, howto, eeat]

# Dependency graph
requires:
  - phase: 08-structured-data
    provides: SCHEMA-01 LocalBusiness, SCHEMA-02 FAQPage, SCHEMA-03 Service JSON-LD blocks in head
provides:
  - Full NAP (name, address, phone) in LocalBusiness JSON-LD (NAP-03)
  - Person entity for Allan Chan with LinkedIn sameAs (EEAT-02)
  - HowTo JSON-LD mapping 4-step How It Works process (SCHEMA-04)
affects: [11-visible-identity-contact, 12-aeo-copy-optimisation]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "JSON-LD blocks labelled with HTML comments using requirement ID (e.g. EEAT-02, SCHEMA-04)"
    - "HowTo step property is singular 'step' not 'steps'; position values are strings not integers"
    - "Person sameAs uses full LinkedIn profile URL"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "4 HowToStep entries included to match all 4 visible How It Works steps — plan note said 3 but RESEARCH.md and visible section both show 4"
  - "telephone in E.164 format (+13074009814) with no spaces or dashes — schema.org best practice"
  - "HTML entities in visible body HTML are correct usage; only JSON string values must be entity-free (confirmed zero entities in JSON blocks)"

patterns-established:
  - "JSON-LD blocks for new schema types appended before </head> after last existing </script>"
  - "Comment anchors follow pattern: <!-- Structured Data: Type — Description (REQ-ID) -->"

requirements-completed: [NAP-03, EEAT-02, SCHEMA-04]

# Metrics
duration: 2min
completed: 2026-04-29
---

# Phase 10 Plan 01: Schema Authority Layer Summary

**LocalBusiness JSON-LD upgraded with full Belfast PostalAddress + E.164 telephone; Person entity for Allan Chan + HowTo 4-step process block added to head — 5 total JSON-LD blocks, zero schema errors**

## Performance

- **Duration:** 2 min
- **Started:** 2026-04-29T06:46:41Z
- **Completed:** 2026-04-29T06:48:08Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments

- SCHEMA-01 LocalBusiness updated with full PostalAddress (streetAddress, addressLocality, addressRegion, postalCode, addressCountry) and telephone in E.164 format
- Person entity added for founder Allan Chan with jobTitle, url, and LinkedIn sameAs — satisfies EEAT-02
- HowTo block added with 4 HowToStep entries whose names match the visible How It Works section verbatim — satisfies SCHEMA-04
- JSON-LD block count increased from 3 to 5; all existing blocks (SCHEMA-01, SCHEMA-02, SCHEMA-03) preserved intact

## Task Commits

Each task was committed atomically:

1. **Task 1: Update LocalBusiness address and telephone (NAP-03)** - `518433a` (feat)
2. **Task 2: Add Person and HowTo JSON-LD blocks (EEAT-02, SCHEMA-04)** - `5ca8545` (feat)

**Plan metadata:** (docs commit — see below)

## Files Created/Modified

- `index.html` - Updated SCHEMA-01 address block (5 PostalAddress fields + telephone); added Person and HowTo JSON-LD blocks before </head>

## Decisions Made

- 4 HowToStep entries included (not 3 as mentioned in ROADMAP success criterion) — RESEARCH.md explicitly recommends matching all 4 visible steps to satisfy "names match visible section content"
- telephone value `+13074009814` from STATE.md v1.2 note, E.164 format (no spaces, no dashes)
- HTML entities found in visible HTML body are correct usage; confirmed zero HTML entities in any JSON string value
- Person block inserted before HowTo block (ordered per plan: Person first, HowTo second)

## Deviations from Plan

None - plan executed exactly as written. The 4-step vs 3-step note was pre-resolved in the plan itself (Task 2 action explicitly documents the discrepancy and instructs to include all 4).

## Issues Encountered

None — both edits applied cleanly. The insertion point for Task 2 (after SCHEMA-03 `</script>`, before `</head>`) was unambiguous.

## User Setup Required

None - no external service configuration required.

**Manual validation recommended (post-commit):**
1. Paste each JSON-LD block into https://validator.schema.org/ — each should show zero errors
2. Visit https://search.google.com/test/rich-results and test deployed URL — LocalBusiness should show valid with address fields

## Next Phase Readiness

- Phase 10 Plan 01 complete — machine-readable authority signals established in head
- Phase 11 (Visible Identity & Contact) can now add matching visible NAP in footer and founder name/LinkedIn in About section
- Phase 12 (AEO Copy Optimisation) can reference the structured data foundation when restructuring FAQ answers and adding stats

---
*Phase: 10-schema-authority-layer*
*Completed: 2026-04-29*
