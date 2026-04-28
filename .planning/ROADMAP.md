# ROADMAP: Leadzly Website Overhaul

**Milestone:** M1 — Full Website Overhaul
**Goal:** Convert sceptical UK/NI B2B decision-makers into booked strategy calls
**Requirements:** 27 v1 requirements mapped across 6 phases

---

## Phases

| # | Phase | Goal | Requirements | Plans |
|---|-------|------|--------------|-------|
| 1 | Foundation & Hygiene | Site is clean, crawlable, and navigable before any structural work | NAV-01, NAV-02, NAV-03, QWIN-01, QWIN-02, QWIN-04 | 2 plans |
| 2 | Page Restructure & Scaffolding | 1/2 | In Progress|  |
| 3 | Trust & Social Proof | 3/3 | Complete   | 2026-04-28 |
| 4 | Conversion Sections | 4/4 | Complete   | 2026-04-28 |
| 5 | Booking Experience & CTAs | 4/4 | Complete   | 2026-04-28 |
| 6 | Design Polish & Mobile | 2/2 | Complete   | 2026-04-28 |

---

## Phase Details

### Phase 1: Foundation & Hygiene

**Goal:** The site has correct meta data, GDPR consent, a favicon, secure external links, and a properly behaved nav — all before any structural changes that would disrupt testing.
**Depends on:** Nothing (first phase)
**Requirements:** NAV-01, NAV-02, NAV-03, QWIN-01, QWIN-02, QWIN-04
**UI hint**: yes

**Success Criteria:**
1. The nav sticks to the top on scroll and gains a visible blur/shadow effect on all screen sizes
2. A working hamburger menu appears on mobile and opens/closes the nav links
3. Sharing the page URL on Slack, WhatsApp, or LinkedIn renders the correct OG title, description, and image
4. A custom favicon appears in the browser tab and bookmark bar
5. A GDPR cookie consent banner appears on first visit and does not reappear after the user accepts

**Plans:** 2 plans
- [ ] 01-PLAN-meta-favicon.md — Add missing OG tags (og:image, og:url) + favicon link tags; verify pre-implemented NAV requirements
- [ ] 01-PLAN-gdpr-banner.md — GDPR cookie consent banner with real GA4 gating via localStorage

---

### Phase 2: Page Restructure & Scaffolding

**Goal:** The full intended section order exists in the HTML with the correct structural markup and background alternation in place, ready for content to be filled in.
**Depends on:** Phase 1
**Requirements:** PAGE-01, PAGE-02, PAGE-03, PAGE-04, DSGN-02
**UI hint**: yes

**Success Criteria:**
1. Scrolling the page top-to-bottom reveals sections in this exact order: hero, trust bar, pain points, services, multi-channel, process, industries, case studies, testimonials, comparison table, pricing, FAQ, booking, footer CTA, footer
2. A "Does this sound familiar?" pain points section with 6 empathetic problem cards is visible on the page
3. A dark navy multi-channel section with 4 channel cards (Telesales, Cold Email, LinkedIn, Follow-Up) is visible
4. An industries section with a 12-chip grid (icon + label) is visible
5. Sections visibly alternate between white, light grey, dark navy, and green-tinted backgrounds as the user scrolls

**Plans:** 1/2 plans executed
- [x] 02-01-PLAN.md — HTML restructure: reorder existing sections, insert pain points / multi-channel / industries, add scaffold sections for case studies, testimonials, comparison, FAQ, booking
- [x] 02-02-PLAN.md — CSS: background alternation (DSGN-02) + full styles for pain-points, channels, industries grids and cards

---

### Phase 3: Trust & Social Proof

**Goal:** Visitors see a trust bar below the hero with partner logos and a 30-day guarantee, a "Proven Across Industries" section with 4 real industry cards and an aggregate stat, and an About section elevated earlier in the page flow — enough social proof to displace scepticism without fabricating testimonials or case studies.
**Depends on:** Phase 2
**Requirements:** TRST-03, TRST-04, TRST-05, TRST-06 (TRST-01 and TRST-02 superseded by D-01 — testimonials section deleted, not built)
**UI hint**: yes

**Success Criteria:**
1. A trust bar directly below the hero shows 5 greyscale partner logos, a "100+ meetings/month" stat badge, and a "30-Day Guarantee" badge
2. A "Proven Across Industries" section at #case-studies shows 4 industry cards (Digital Marketing, AI & Automation, SaaS & Software, E-Commerce), each with a green tag chip, bold outcome line, and supporting detail line
3. The About section appears between Services and Multi-Channel — earlier in the page than its original position after pricing
4. No testimonials scaffold section exists on the page

**Plans:** 3/3 plans complete
- [x] 03-01-PLAN.md — Logo assets copy + trust bar HTML/CSS (TRST-05)
- [x] 03-02-PLAN.md — Proven Across Industries section HTML/CSS + testimonials deletion (TRST-03, TRST-04)
- [x] 03-03-PLAN.md — About section repositioning + human verify checkpoint (TRST-06)

---

### Phase 4: Conversion Sections

**Goal:** Visitors encounter a side-by-side comparison of Leadzly against alternatives, can self-qualify via FAQ, can calculate their own ROI, and experience consistent typographic hierarchy throughout.
**Depends on:** Phase 2
**Requirements:** SECT-01, SECT-02, SECT-03, DSGN-01
**UI hint**: yes

**Success Criteria:**
1. A comparison table on a dark navy background compares Leadzly vs In-House vs Other Agencies across 8 rows, with the Leadzly column visibly highlighted in green
2. A FAQ accordion with 7 questions opens and closes one question at a time (others collapse), centred at max-width 720px
3. An ROI calculator accepts 3 inputs (average deal value, current meetings/month, target meetings/month) and outputs projected additional monthly revenue and annual pipeline — all calculated in-browser with no server call
4. Every section on the page has a consistent eyebrow label (small caps, green) above the main heading, and the hero h1 is bold at ≥3.5rem

**Plans:** 4/4 plans complete
- [x] 04-01-PLAN.md — DSGN-01: hero h1 clamp fix (2.4rem → 3.5rem) + .section-label weight fix (600 → 700)
- [x] 04-02-PLAN.md — SECT-01: comparison table HTML + CSS (dark navy background, 8 rows, Leadzly column green)
- [x] 04-03-PLAN.md — SECT-02: FAQ accordion HTML + CSS + JS IIFE (7 questions, one-at-a-time, max-height animation)
- [x] 04-04-PLAN.md — SECT-03: ROI calculator HTML + CSS + JS IIFE + human verify checkpoint (all Phase 4 changes) (completed 2026-04-28)

---

### Phase 5: Booking Experience & CTAs

**Goal:** Visitors can book a strategy call without leaving the page, a persistent floating CTA is always visible, varied CTA copy appears in every major section, and hero stat numbers animate on scroll.
**Depends on:** Phase 3
**Requirements:** BOOK-01, BOOK-02, BOOK-03, QWIN-03
**UI hint**: yes

**Success Criteria:**
1. A two-column booking section shows bullet benefits on the left and a live Calendly calendar widget on the right — a visitor can select a slot without leaving leadzly.co
2. A floating sticky button in the bottom-right corner with a pulsing green dot animates into view 3 seconds after page load
3. At least 4 distinct CTA variants appear across the page ("Book a Free Strategy Call", "Get Your Meetings Forecast", "See If We're a Fit", "Start Filling Your Calendar")
4. Hero stat numbers visibly count up from zero when scrolled into view for the first time

**Plans:** 4/4 plans complete
- [x] 05-01-PLAN.md — BOOK-01: Calendly inline embed — two-column booking grid HTML + CSS + Calendly CDN resources
- [x] 05-02-PLAN.md — BOOK-02: Floating sticky CTA — HTML + CSS (fixed position, pulse ring, mobile hide) + JS IIFE (3s delay)
- [x] 05-03-PLAN.md — BOOK-03: CTA copy variants — hero + pricing copy edits + ROI section CTA addition
- [x] 05-04-PLAN.md — QWIN-03: Hero stat counter — data attributes + JS IIFE (IntersectionObserver + rAF) + human verify checkpoint (all Phase 5 changes) (completed 2026-04-28)

---

### Phase 6: Design Polish & Mobile

**Goal:** Every card, section, and grid is visually polished with hover effects, scroll animations, and a fully responsive mobile layout — the site feels intentional and trustworthy on any device.
**Depends on:** Phases 4, 5
**Requirements:** DSGN-03, DSGN-04
**UI hint**: yes

**Success Criteria:**
1. Hovering any card produces a lift effect (translateY(-4px) + elevated box-shadow) and a visible green border highlight
2. Each section fades up into view on scroll via IntersectionObserver — no jarring instant appearance
3. On a 375px mobile viewport, all grids collapse to single column, process steps stack vertically without arrows visible, and the comparison table scrolls horizontally
4. The floating sticky CTA button does not obscure content on mobile (hides or repositions when overlapping)

**Plans:** 2/2 plans complete
- [x] 06-01-PLAN.md — DSGN-03 + DSGN-04: card hover lift effects (all 8 card classes) + fade-in translateY upgrade in page.css
- [x] 06-02-PLAN.md — DSGN-03: add fade-in class to 13 section elements in index.html + human verify checkpoint

---

## Progress Table

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation & Hygiene | 0/2 | Not started | — |
| 2. Page Restructure & Scaffolding | 2/2 | Complete | 2026-04-27 |
| 3. Trust & Social Proof | 3/3 | Complete | 2026-04-27 |
| 4. Conversion Sections | 0/4 | Not started | — |
| 5. Booking Experience & CTAs | 0/4 | Not started | — |
| 6. Design Polish & Mobile | 0/2 | Not started | — |

---

## Coverage

| Requirement | Phase | Status |
|-------------|-------|--------|
| NAV-01 | Phase 1 | Pending |
| NAV-02 | Phase 1 | Pending |
| NAV-03 | Phase 1 | Pending |
| QWIN-01 | Phase 1 | Pending |
| QWIN-02 | Phase 1 | Pending |
| QWIN-04 | Phase 1 | Pending |
| PAGE-01 | Phase 2 | Pending |
| PAGE-02 | Phase 2 | Pending |
| PAGE-03 | Phase 2 | Pending |
| PAGE-04 | Phase 2 | Pending |
| DSGN-02 | Phase 2 | Pending |
| TRST-01 | Phase 3 | Superseded by D-01 (testimonials deleted) |
| TRST-02 | Phase 3 | Superseded by D-01 (testimonials deleted) |
| TRST-03 | Phase 3 | Pending |
| TRST-04 | Phase 3 | Pending |
| TRST-05 | Phase 3 | Pending |
| TRST-06 | Phase 3 | Pending |
| SECT-01 | Phase 4 | Pending |
| SECT-02 | Phase 4 | Pending |
| SECT-03 | Phase 4 | Pending |
| DSGN-01 | Phase 4 | Pending |
| BOOK-01 | Phase 5 | Complete |
| BOOK-02 | Phase 5 | Complete |
| BOOK-03 | Phase 5 | Complete |
| QWIN-03 | Phase 5 | Complete |
| DSGN-03 | Phase 6 | Pending |
| DSGN-04 | Phase 6 | Pending |

**Coverage: 27/27 v1 requirements mapped**

---
*Roadmap created: 2026-04-24*
*Last updated: 2026-04-28 — Phase 6 planned: 2 plans (06-01-PLAN.md, 06-02-PLAN.md) covering DSGN-03 and DSGN-04.*
