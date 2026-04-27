# Phase 3: Trust & Social Proof - Context

**Gathered:** 2026-04-27
**Status:** Ready for planning

<domain>
## Phase Boundary

Add credibility signals that displace scepticism in UK/NI B2B buyers — a trust bar below the hero, a "Proven Across Industries" section (replacing the planned case studies), and an elevated About section. The testimonials section is removed entirely rather than built with placeholder or fake content.

**Scope anchor:** This phase does NOT add interactions, animations, or hover effects (Phase 6). It delivers static HTML/CSS content sections only.

</domain>

<decisions>
## Implementation Decisions

### Testimonials Section — Removed
- **D-01:** The testimonials section scaffold is deleted, not built out. No real testimonials exist and fake ones would undermine credibility with sceptical B2B buyers (same reasoning as Phase 2 fake social proof removal). The scaffold placeholder `<p class="scaffold-placeholder">` and surrounding HTML are removed entirely.

### Case Studies → "Proven Across Industries"
- **D-02:** The case studies section is repurposed and renamed. New section title: **"Proven Across Industries"**. Eyebrow label: "Results".
- **D-03:** 4 industry cards — one per Elevateo portfolio vertical. No company names on the cards. Industries: **Digital Marketing**, **AI & Automation**, **SaaS & Software**, **E-Commerce**.
- **D-04:** Each card shows: industry label (as a tag/chip), one-line outcome description, and a supporting detail line. No metrics beyond the aggregate stat.
- **D-05:** Aggregate stat — **"100+ meetings booked monthly"** — displayed as a section headline stat or subheading, not per-card. This is the real number from running outbound across the Elevateo portfolio.
- **D-06:** Section background: white (`var(--section-white)`) — consistent with current case-studies background.

### Trust Bar (TRST-05)
- **D-07:** Trust bar sits directly below the hero section (before pain points). Slim horizontal band.
- **D-08:** Contents: 5 portfolio company logos (greyscale) + **"100+ meetings/month"** stat badge + **"30-Day Guarantee"** badge. No Clutch rating (no confirmed profile).
- **D-09:** Logo files to copy from `C:\Work File\Assets\Logos\` into repo: `bluepillar-logo.png`, `boltloop_logo.png`, `elevateo_logo.png`, `flooda_logo.png`, `roseyCo_logo.png`. Copy as-is — do not rename.
- **D-10:** Logos displayed greyscale (CSS `filter: grayscale(100%)`) with opacity ~0.6, standard treatment for client/partner logo strips.
- **D-11:** Background: light grey (`var(--section-grey)`) or white — whichever contrasts cleanly with the hero below and pain points above. Claude's discretion.

### About Section — Repositioned (TRST-06)
- **D-12:** About section moves earlier in the page flow. Exact placement is Claude's discretion — somewhere between services and case studies feels natural, but planner can decide based on flow.
- **D-13:** About section content already emphasises the NI-based, no-offshore differentiator — no content rewrite needed. Only repositioning in the HTML.

### Claude's Discretion
- Exact placement of About section within the flow
- Trust bar background colour (light grey vs white — whichever works visually)
- Card layout for "Proven Across Industries" (2×2 grid vs 4-column row)
- Exact copy for each industry card's one-liner and detail line
- Whether "100+ meetings/month" appears in the trust bar, the section subheading, or both

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Existing Code
- `index.html` — Full HTML structure; testimonials scaffold at line ~463, case-studies section at line ~449, about section at line ~545
- `page.css` — All existing section CSS including `.testimonials`, `.case-studies`, `.about` blocks
- `style.css` — CSS custom properties (colour tokens, spacing scale, shadows)
- `base.css` — Base reset and typography

### Logo Assets
- `C:\Work File\Assets\Logos\bluepillar-logo.png` — Bluepillar (E-Commerce division)
- `C:\Work File\Assets\Logos\boltloop_logo.png` — Boltloop (AI & Automation division)
- `C:\Work File\Assets\Logos\elevateo_logo.png` — Elevateo (parent company)
- `C:\Work File\Assets\Logos\flooda_logo.png` — Flooda (SaaS/Software division)
- `C:\Work File\Assets\Logos\roseyCo_logo.png` — Rosey Co (Marketing division)

### Requirements
- `.planning/REQUIREMENTS.md` §TRST-03, §TRST-04, §TRST-05, §TRST-06 — acceptance criteria (TRST-01 and TRST-02 are superseded by D-01)

No external ADRs or specs — all decisions captured in this context file.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `.section-header` + `.section-label` + `.section-title` pattern — already used across all sections; "Proven Across Industries" must follow this same structure
- `.btn.btn--primary` — reuse for any CTAs within the new section
- `var(--section-white)`, `var(--section-grey)`, `var(--section-tint)` — background tokens; use these, not hardcoded hex
- Existing `.about` CSS at `page.css:823` — no changes needed, section just moves in HTML

### Established Patterns
- All JS is vanilla inline `<script>` block at bottom of `<body>` — any JS for this phase (none anticipated) follows this pattern
- Card components use `fade-in` class for scroll animation hooks (Phase 6 wires the JS)
- SVG icons inline in HTML — no external icon library

### Integration Points
- `index.html` line ~213 — hero section closes here; trust bar inserts immediately after
- `index.html` line ~449 — case-studies section; replace contents with "Proven Across Industries"
- `index.html` line ~463 — testimonials section; delete this entire section block
- `index.html` line ~545 — about section; cut from here, paste earlier in the flow

</code_context>

<specifics>
## Specific Ideas

- "100+ meetings booked monthly" is a real aggregate number from running outbound across the Elevateo portfolio — it can appear in the trust bar and/or as a section stat without attribution to specific companies
- The 4 industry card verticals map to real Elevateo divisions: Digital Marketing (Rosey Co), AI & Automation (Boltloop), SaaS & Software (Flooda), E-Commerce (Bluepillar) — content can reflect what those divisions actually do without naming them
- Logo greyscale treatment is intentional — keeps trust bar visually clean and avoids colour clashes between 5 different brand colours

</specifics>

<deferred>
## Deferred Ideas

- Real testimonials section — build when actual client testimonials are available (future phase)
- Full case studies with documented client outcomes — build when formal case studies are ready (future phase)
- Clutch profile badge — add when Leadzly has an active Clutch listing
- Team photos in About section — blocked on sourcing assets (noted in REQUIREMENTS.md V2-04)

</deferred>

---

*Phase: 03-trust-social-proof*
*Context gathered: 2026-04-27*
