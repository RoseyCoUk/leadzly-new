# Requirements — Leadzly Website Overhaul

## Milestone v1.2: E-E-A-T & AI Search Signals

**Goal:** Strengthen leadzly.co's authority and AI citability by adding real contact details, founder identity, additional schema, and AEO-optimised copy within the existing static page.

---

### Contact & Local Presence (NAP)

- [x] **NAP-01**: Footer displays company address (1 Hollycroft Avenue, Belfast, BT5 5JE, United Kingdom) as visible, readable text
- [x] **NAP-02**: Footer displays WhatsApp phone number (+1 307 400 9814) as a clickable `tel:` link replacing the current placeholder
- [x] **NAP-03**: Existing `LocalBusiness` JSON-LD block updated with full `streetAddress`, `addressLocality` (Belfast), `postalCode` (BT5 5JE), `addressCountry` (GB), and `telephone` fields

### Founder & E-E-A-T Signals (EEAT)

- [x] **EEAT-01**: About section names Allan Chan as founder with a visible LinkedIn profile link (`https://www.linkedin.com/in/allan-chan-865422216/`)
- [x] **EEAT-02**: `Person` JSON-LD block added to `<head>` for Allan Chan (name, jobTitle "Founder", url `https://leadzly.co`, sameAs LinkedIn URL)

### Additional Schema (SCHEMA)

- [x] **SCHEMA-04**: `HowTo` JSON-LD block added to `<head>` mapping the 3-step "How It Works" process (step names, descriptions, and URLs matching the visible section content)

### AEO / AI Search Optimisation (AEO)

- [ ] **AEO-01**: All 7 FAQ answers restructured to lead with a direct, self-contained answer sentence before elaborating — optimised for AI extraction and featured snippet eligibility
- [ ] **AEO-02**: Hero sub-headline and/or services section copy updated to include at least 2 specific, verifiable statistics (e.g. meetings booked per month, sectors served, response rate)
- [ ] **AEO-03**: About section paragraph rewritten with specific, citable claims — NI team, years of experience, or portfolio outcomes — replacing vague agency language

---

## Future Requirements

- Testimonials section — when real client testimonials are sourced (TRST-01/02 revival)
- Replace hero image with NI/UK-specific B2B photography (V2-05)
- Team photos in About section (V2-04, blocked on asset sourcing)
- Live Clutch review widget if API becomes available (V2-01)
- Active section highlighting in nav on scroll (V2-02)
- Blog / content hub — when content strategy is defined

## Out of Scope (v1.2)

- Blog or content pages — single-page only (existing constraint)
- Google Business Profile optimisation — operational task
- Backlink building — off-page, no code changes
- Testimonials — no real content available yet
- Wikipedia / Wikidata entity presence — operational task
- Google Search Console submission — operational task

---

## Traceability

| REQ-ID | Phase | Plan |
|--------|-------|------|
| NAP-01 | Phase 11 | — |
| NAP-02 | Phase 11 | — |
| NAP-03 | Phase 10 | — |
| EEAT-01 | Phase 11 | — |
| EEAT-02 | Phase 10 | — |
| SCHEMA-04 | Phase 10 | — |
| AEO-01 | Phase 12 | — |
| AEO-02 | Phase 12 | — |
| AEO-03 | Phase 12 | — |
