---
phase: 08-structured-data
verified: 2026-04-28T00:00:00Z
status: passed
score: 8/8 must-haves verified
re_verification: false
gaps: []
human_verification:
  - test: "Run all three JSON-LD blocks through Google's Rich Results Test"
    expected: "Tool reports valid Organization/LocalBusiness, FAQPage, and Service markup with zero errors for each"
    why_human: "Requires live URL or file upload to an external Google tool — cannot test programmatically without network access to the deployed site"
  - test: "Paste each JSON-LD block into https://validator.schema.org/"
    expected: "Zero errors reported for all three blocks individually"
    why_human: "External validator — not testable via grep/static analysis alone"
---

# Phase 8: Structured Data Verification Report

**Phase Goal:** leadzly.co emits machine-readable schema that qualifies it for Google rich results (FAQ snippets, business knowledge panel)
**Verified:** 2026-04-28
**Status:** PASSED
**Re-verification:** No — initial verification

---

## Goal Achievement

### Observable Truths

| # | Truth | Status | Evidence |
|---|-------|--------|----------|
| 1 | `index.html <head>` contains a `<script type="application/ld+json">` block for Organization + LocalBusiness | VERIFIED | Lines 121–150; `grep -c "application/ld+json" index.html` = 3; position 4893 < `</head>` at position 10564 |
| 2 | Block includes name, url, logo, description, areaServed (UK + NI), contactPoint, address, parentOrganization | VERIFIED | Node.js `JSON.parse()` deep check: all 8 required properties confirmed; no telephone, no email |
| 3 | The JSON is syntactically valid — no trailing commas, no unclosed brackets, no HTML entities inside JSON | VERIFIED | `JSON.parse()` succeeds on all 3 blocks; entity scan inside ld+json blocks returns zero `&[a-z]+;` matches |
| 4 | No contact detail (phone, email) appears in the schema that is not visible on the page | VERIFIED | `telephone=false`, `email=false` confirmed via property existence check on parsed block 1 |
| 5 | `index.html <head>` contains a FAQPage JSON-LD block covering all 7 FAQ questions and answers | VERIFIED | `mainEntity.length = 7` confirmed via Node.js parse; all 7 Question nodes have acceptedAnswer; `grep -c '"@type": "Question"' index.html` = 7 |
| 6 | `index.html <head>` contains a Service JSON-LD block with a @graph array of 4 service entries | VERIFIED | `@graph.length = 4` confirmed via Node.js parse; all 4 Service nodes present (Telesales Outsourcing, Meeting Booking, BDM Provision, Sales Enablement) |
| 7 | All FAQ answer text matches the visible page content with HTML entities decoded to Unicode | VERIFIED | Visible page uses `&ndash;`, `&rsquo;`, `&mdash;`, `&ldquo;`, `&rdquo;`; schema JSON contains decoded Unicode (–, ', —, ", "); zero raw entities inside any ld+json block |
| 8 | All three JSON-LD blocks are syntactically valid JSON | VERIFIED | All 3 blocks parse cleanly via `JSON.parse()` with no exceptions |

**Score:** 8/8 truths verified

---

### Required Artifacts

| Artifact | Expected | Status | Details |
|----------|----------|--------|---------|
| `index.html` | Organization + LocalBusiness JSON-LD block in `<head>` (SCHEMA-01) | VERIFIED | Lines 121–150; `@type: ["LocalBusiness","ProfessionalService"]`; all required fields present |
| `index.html` | FAQPage JSON-LD block with 7 Q&A entries (SCHEMA-02) | VERIFIED | Lines 151–215; 7 Question/Answer pairs confirmed via Node.js parse |
| `index.html` | Service JSON-LD @graph with 4 service nodes (SCHEMA-03) | VERIFIED | Lines 216–255; 4 Service nodes; descriptions are exact string matches to visible `.service-card__desc` text |

---

### Key Link Verification

| From | To | Via | Status | Details |
|------|----|-----|--------|---------|
| `index.html <head>` | `schema.org/LocalBusiness` | `@type: ["LocalBusiness", "ProfessionalService"]` | WIRED | Line 125 confirmed; areaServed UK+NI AdministrativeArea nodes confirmed |
| `index.html <head>` | `schema.org/FAQPage` | `@type: "FAQPage"`, mainEntity array of 7 Question nodes | WIRED | Line 155 confirmed; 7 Question nodes at lines 157–212 |
| `index.html <head>` | `schema.org/Service` | `@graph` array with 4 Service nodes | WIRED | `@graph` at line 220 confirmed; 4 Service nodes; `provider.name: "Leadzly"` on all |

---

### Data-Flow Trace (Level 4)

Not applicable. This phase produces static JSON-LD declarations embedded in HTML head. There is no runtime data fetching, state management, or dynamic rendering involved. All values are literal strings sourced verbatim from visible page content.

---

### Behavioral Spot-Checks

| Behavior | Command | Result | Status |
|----------|---------|--------|--------|
| 3 JSON-LD script blocks present | `grep -c "application/ld+json" index.html` | 3 | PASS |
| 7 Question nodes present | `grep -c '"@type": "Question"' index.html` | 7 | PASS |
| 7 acceptedAnswer nodes present | `grep -c "acceptedAnswer" index.html` | 7 | PASS |
| 4 Service nodes present | `grep -c '"@type": "Service"' index.html` | 4 | PASS |
| @graph array present (Service block) | `grep -c "@graph" index.html` | 1 | PASS |
| All blocks before `</head>` | Node.js position check: ld+json at 4893 < `</head>` at 10564 | Confirmed | PASS |
| All 3 blocks valid JSON | `JSON.parse()` on each extracted block | No errors | PASS |
| No HTML entities inside JSON blocks | Entity regex scan in ld+json blocks | 0 matches | PASS |
| No telephone/email in block 1 | Property existence check on parsed JSON | telephone=false, email=false | PASS |
| Service descriptions match visible page | String equality: schema desc vs `.service-card__desc` | All 4 exact matches | PASS |
| Commits exist in git history | `git log --oneline` | cb9bcea, f8e17ff, 502d2a8 all confirmed | PASS |

---

### Requirements Coverage

| Requirement | Source Plan | Description | Status | Evidence |
|-------------|-------------|-------------|--------|----------|
| SCHEMA-01 | 08-01-PLAN.md | Organization + LocalBusiness JSON-LD block in `<head>` (name, URL, logo, contactPoint, areaServed UK/NI) | SATISFIED | Lines 121–150; all 8 required properties; commit cb9bcea |
| SCHEMA-02 | 08-02-PLAN.md | FAQPage JSON-LD block mapping all 7 existing FAQ questions and answers | SATISFIED | Lines 151–215; 7 Q+A pairs; Unicode entity decoding confirmed; commit f8e17ff |
| SCHEMA-03 | 08-02-PLAN.md | Service JSON-LD block covering telesales, meeting booking, and BDM provision | SATISFIED | Lines 216–255; @graph of 4 Service nodes matching all visible service cards; commit 502d2a8 |

No orphaned requirements. All three Phase 8 IDs (SCHEMA-01, SCHEMA-02, SCHEMA-03) appear in plan frontmatter and are fully satisfied. REQUIREMENTS.md traceability table maps all three to Phase 8 and marks them checked.

---

### Anti-Patterns Found

| File | Line | Pattern | Severity | Impact |
|------|------|---------|----------|--------|
| — | — | None found | — | — |

No TODO/FIXME comments, no placeholder text, no empty returns, no HTML entities inside JSON blocks, no hardcoded stubs.

---

### Human Verification Required

#### 1. Google Rich Results Test

**Test:** After deployment, go to https://search.google.com/test/rich-results and test `https://leadzly.co/`
**Expected:** FAQPage schema is detected and flagged as eligible for FAQ rich snippets; the LocalBusiness/Organization schema passes with zero errors or warnings
**Why human:** Google's Rich Results Test runs a live fetch against their crawler infrastructure — cannot replicate with static file analysis

#### 2. Schema.org Validator

**Test:** Paste each of the three JSON-LD blocks (without `<script>` wrapper tags) individually into https://validator.schema.org/
**Expected:** Zero errors reported for Organization+LocalBusiness block, FAQPage block, and Service @graph block
**Why human:** External validator tool; not accessible programmatically during verification

---

### Gaps Summary

No gaps. All 8 observable truths are verified, all 3 artifacts pass all levels (exists, substantive, wired), all key links are confirmed, and all 3 requirement IDs (SCHEMA-01, SCHEMA-02, SCHEMA-03) are fully satisfied. The phase goal — emitting machine-readable schema qualifying for Google rich results — is achieved in the codebase.

The only outstanding items are the two human verification steps requiring external Google/schema.org tools, which are post-deployment operational checks rather than code gaps.

---

_Verified: 2026-04-28_
_Verifier: Claude (gsd-verifier)_
