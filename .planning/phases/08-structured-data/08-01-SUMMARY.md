---
phase: 08-structured-data
plan: 01
subsystem: seo
tags: [json-ld, structured-data, schema-org, local-business, organization]

# Dependency graph
requires:
  - phase: 07-meta-hygiene
    provides: canonical URL, robots meta, Twitter Cards, robots.txt, sitemap.xml
provides:
  - Organization + LocalBusiness JSON-LD block in index.html <head>
  - SCHEMA-01 requirement satisfied
affects: [08-02, 08-03, phase-09-performance]

# Tech tracking
tech-stack:
  added: []
  patterns: [JSON-LD script blocks in <head> before </head>; separate blocks per schema type]

key-files:
  created: []
  modified: [index.html]

key-decisions:
  - "contactPoint uses Calendly URL (not email or phone) — no visible contact info on page, schema-content mismatch avoided"
  - "address uses only addressRegion and addressCountry — no street address on page, fabrication avoided"
  - "areaServed: United Kingdom + Northern Ireland — matches page claims, Republic of Ireland excluded"
  - "parentOrganization: Elevateo Co — matches about section visible text"
  - "No telephone or email in schema — neither appears in visible page UI (anti-pattern avoided per research)"

patterns-established:
  - "JSON-LD: Separate <script type=application/ld+json> blocks per schema type (not combined @graph)"
  - "JSON-LD: Inserted immediately before </head>, after Calendly CDN scripts"
  - "JSON-LD: Plain Unicode strings only — no HTML entities in JSON values"

requirements-completed: [SCHEMA-01]

# Metrics
duration: 5min
completed: 2026-04-29
---

# Phase 8 Plan 01: Structured Data Summary

**Organization + LocalBusiness JSON-LD block with @type ["LocalBusiness","ProfessionalService"], areaServed UK/NI, and Calendly contactPoint inserted into index.html head**

## Performance

- **Duration:** 5 min
- **Started:** 2026-04-29T00:26:00Z
- **Completed:** 2026-04-29T00:31:22Z
- **Tasks:** 1
- **Files modified:** 1

## Accomplishments

- Inserted single `<script type="application/ld+json">` block into index.html `<head>` before `</head>`
- Block covers all SCHEMA-01 required properties: name, url, logo, description, areaServed, contactPoint, address, parentOrganization
- No schema-content mismatches: no phone, email, or street address added (none visible on page)
- JSON is syntactically valid with no HTML entities, no trailing commas

## Task Commits

Each task was committed atomically:

1. **Task 1: Insert Organization + LocalBusiness JSON-LD block into index.html head** - `cb9bcea` (feat)

**Plan metadata:** (docs commit follows)

## Files Created/Modified

- `index.html` - Added 30-line Organization + LocalBusiness JSON-LD block at lines 36–65, before `</head>` at line 66

## Decisions Made

- `contactPoint.url` set to Calendly booking link (`https://calendly.com/allan-chan-leadzly`) — the only contact method on the visible page; no phone or email is displayed in the UI
- `address` uses only `addressRegion: "Northern Ireland"` and `addressCountry: "GB"` — no street address exists on page
- `areaServed` uses United Kingdom and Northern Ireland as `AdministrativeArea` nodes — matches hero eyebrow and about section wording; Republic of Ireland excluded (page does not explicitly claim IE clients)
- `@type` array `["LocalBusiness", "ProfessionalService"]` — LocalBusiness is the most specific applicable type; ProfessionalService adds service-business signal

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- SCHEMA-01 complete; ready for SCHEMA-02 (FAQPage JSON-LD, plan 02)
- Plan 02 will extract the 7 FAQ Q&A pairs from index.html and encode as FAQPage schema
- Plan 03 will add Service JSON-LD for the three service offerings (telesales, meeting booking, BDM provision)
- All data values for plans 02 and 03 are already extracted verbatim in 08-RESEARCH.md

## Known Stubs

None — JSON-LD block is fully wired with real values from the live page. No placeholder text or hardcoded empty values.

---
*Phase: 08-structured-data*
*Completed: 2026-04-29*
