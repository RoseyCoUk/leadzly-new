# Roadmap: Leadzly Website Overhaul

## Milestones

- ✅ **v1.0 Full Website Overhaul** — Phases 1–6 (shipped 2026-04-28)
- ✅ **v1.1 SEO & Performance** — Phases 7–9 (shipped 2026-04-29)
- 📋 **v1.2 E-E-A-T & AI Search Signals** — Phases 10–12 (planned)

## Phases

<details>
<summary>✅ v1.0 Full Website Overhaul (Phases 1–6) — SHIPPED 2026-04-28</summary>

- [x] Phase 1: Foundation & Hygiene (2/2 plans) — completed 2026-04-24
- [x] Phase 2: Page Restructure & Scaffolding (2/2 plans) — completed 2026-04-27
- [x] Phase 3: Trust & Social Proof (3/3 plans) — completed 2026-04-27
- [x] Phase 4: Conversion Sections (4/4 plans) — completed 2026-04-28
- [x] Phase 5: Booking Experience & CTAs (4/4 plans) — completed 2026-04-28
- [x] Phase 6: Design Polish & Mobile (2/2 plans) — completed 2026-04-28

Full details: [.planning/milestones/v1.0-ROADMAP.md](./milestones/v1.0-ROADMAP.md)

</details>

<details>
<summary>✅ v1.1 SEO & Performance (Phases 7–9) — SHIPPED 2026-04-29</summary>

- [x] Phase 7: Meta Hygiene (2/2 plans) — completed 2026-04-28
- [x] Phase 8: Structured Data (2/2 plans) — completed 2026-04-29
- [x] Phase 9: Performance & Core Web Vitals (2/2 plans) — completed 2026-04-29

Full details: [.planning/milestones/v1.1-ROADMAP.md](./milestones/v1.1-ROADMAP.md)

</details>

### 📋 v1.2 E-E-A-T & AI Search Signals (Phases 10–12)

- [ ] **Phase 10: Schema Authority Layer** - Add three new JSON-LD blocks (Person, HowTo, updated LocalBusiness) to index.html head
- [ ] **Phase 11: Visible Identity & Contact** - Surface founder name, LinkedIn, and full NAP contact details as visible on-page elements
- [ ] **Phase 12: AEO Copy Optimisation** - Restructure FAQ answers and sharpen hero/About copy with direct sentences and citable statistics

---

## Phase Details

### Phase 10: Schema Authority Layer
**Goal**: Machines can identify Leadzly's location, contact details, founder identity, and process from structured data alone
**Depends on**: Phase 9 (existing JSON-LD blocks in head as base)
**Requirements**: NAP-03, EEAT-02, SCHEMA-04
**Success Criteria** (what must be TRUE):
  1. Google's Rich Results Test confirms LocalBusiness schema includes streetAddress, postalCode, addressLocality, addressCountry, and telephone fields
  2. A Person entity for Allan Chan is present in `<head>` with jobTitle, url, and sameAs (LinkedIn) fields
  3. A HowTo block is present in `<head>` with 3 steps whose names and descriptions match the visible "How It Works" section content
  4. All three new JSON-LD blocks validate without errors in schema.org validator
**Plans**: TBD

### Phase 11: Visible Identity & Contact
**Goal**: Visitors and search crawlers can see Leadzly's physical address, phone number, and named founder directly on the page
**Depends on**: Phase 10 (schema already correct before visible elements added)
**Requirements**: NAP-01, NAP-02, EEAT-01
**Success Criteria** (what must be TRUE):
  1. Footer displays the full postal address (1 Hollycroft Avenue, Belfast, BT5 5JE, United Kingdom) as readable text
  2. Footer phone number is a working `tel:` link showing +1 307 400 9814, replacing any placeholder text
  3. About section names Allan Chan as founder with a visible, clickable LinkedIn link pointing to the correct profile URL
**Plans**: TBD
**UI hint**: yes

### Phase 12: AEO Copy Optimisation
**Goal**: Every FAQ answer opens with a self-contained answer, and hero/About copy contains specific verifiable claims that AI systems can extract and cite
**Depends on**: Phase 11 (visible section content finalised before copy pass)
**Requirements**: AEO-01, AEO-02, AEO-03
**Success Criteria** (what must be TRUE):
  1. All 7 FAQ answers begin with a direct, standalone answer sentence before any elaboration — readable in isolation without context
  2. Hero sub-headline or services copy contains at least 2 specific statistics (e.g. meetings booked per month, sector count, or response rate) with concrete numbers
  3. About section paragraph contains at least 2 specific, verifiable claims (NI team composition, years of experience, or named client outcomes) with no vague agency language
  4. page.css link in index.html shows `?v=15` if any CSS-touching changes were made during this phase
**Plans**: TBD

---

## Progress

| Phase | Milestone | Plans Complete | Status | Completed |
|-------|-----------|----------------|--------|-----------|
| 1. Foundation & Hygiene | v1.0 | 2/2 | Complete | 2026-04-24 |
| 2. Page Restructure & Scaffolding | v1.0 | 2/2 | Complete | 2026-04-27 |
| 3. Trust & Social Proof | v1.0 | 3/3 | Complete | 2026-04-27 |
| 4. Conversion Sections | v1.0 | 4/4 | Complete | 2026-04-28 |
| 5. Booking Experience & CTAs | v1.0 | 4/4 | Complete | 2026-04-28 |
| 6. Design Polish & Mobile | v1.0 | 2/2 | Complete | 2026-04-28 |
| 7. Meta Hygiene | v1.1 | 2/2 | Complete | 2026-04-28 |
| 8. Structured Data | v1.1 | 2/2 | Complete | 2026-04-29 |
| 9. Performance & Core Web Vitals | v1.1 | 2/2 | Complete | 2026-04-29 |
| 10. Schema Authority Layer | v1.2 | 0/? | Not started | - |
| 11. Visible Identity & Contact | v1.2 | 0/? | Not started | - |
| 12. AEO Copy Optimisation | v1.2 | 0/? | Not started | - |

---
*v1.0 shipped: 2026-04-28 | v1.1 shipped: 2026-04-29 | v1.2 planning in progress*
