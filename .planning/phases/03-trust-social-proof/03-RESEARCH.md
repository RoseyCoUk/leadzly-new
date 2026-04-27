# Phase 3: Trust & Social Proof — Research

**Researched:** 2026-04-27
**Domain:** Static HTML/CSS trust signal implementation — trust bar, industry proof section, section repositioning
**Confidence:** HIGH

---

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

- **D-01:** Testimonials section scaffold is deleted entirely — no real testimonials exist and fake content would undermine credibility with B2B buyers. Remove `<section class="testimonials">` block from index.html.
- **D-02:** Case studies section repurposed and renamed to "Proven Across Industries". Eyebrow: "Results".
- **D-03:** 4 industry cards, no company names. Industries: Digital Marketing, AI & Automation, SaaS & Software, E-Commerce.
- **D-04:** Each card: industry tag chip, one-line outcome, supporting detail line. No per-card metrics.
- **D-05:** Aggregate stat "100+ meetings booked monthly" displayed at section level, not per card.
- **D-06:** Proven section background: `var(--section-white)`.
- **D-07:** Trust bar sits directly below the hero section (before pain points). Slim horizontal band.
- **D-08:** Trust bar contents: 5 portfolio logos (greyscale) + "100+ meetings/month" stat badge + "30-Day Guarantee" badge. No Clutch rating.
- **D-09:** Logo files to copy from `C:\Work File\Assets\Logos\` — bluepillar-logo.png, boltloop_logo.png, elevateo_logo.png, flooda_logo.png, roseyCo_logo.png. Copy as-is, do not rename.
- **D-10:** Logos: CSS `filter: grayscale(100%)`, opacity ~0.6.
- **D-11 (resolved):** Trust bar background: `var(--section-tint)` (#f8fafb) — creates a clean visual break between the white hero and the green-tinted pain points section.
- **D-12:** About section moves earlier in page flow — between Services and "Proven Across Industries".
- **D-13:** About section content unchanged — HTML repositioning only.

### Claude's Discretion

- Exact placement of About section within the flow (resolved: after `#services`, before `#case-studies`)
- Trust bar background colour (resolved: `var(--section-tint)`)
- Card layout for "Proven Across Industries" (resolved: 2×2 grid on desktop, 1-column on mobile)
- Exact copy for each industry card's one-liner and detail line (resolved in UI-SPEC)
- Whether "100+ meetings/month" appears in trust bar, section subheading, or both (resolved: both)

### Deferred Ideas (OUT OF SCOPE)

- Real testimonials section — build when actual client testimonials are available
- Full case studies with documented client outcomes — build when formal case studies are ready
- Clutch profile badge — add when Leadzly has an active Clutch listing
- Team photos in About section — blocked on sourcing assets (V2-04)

</user_constraints>

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| TRST-03 | Page displays 4–5 case study cards each with industry tag, timeframe badge, 2–3 key metrics, problem→action→result story, and a client quote | Superseded by D-02 through D-06: 4 industry cards with tag, outcome line, and detail line; no company names, no client quotes |
| TRST-04 | Case study cards have a dark header showing industry and metrics | Superseded by D-03/D-04: cards use light `var(--color-surface)` background; dark headers removed; industry chip is accent-coloured tag only |
| TRST-05 | A trust bar appears directly below the hero with: Clutch rating, "Rated #1 Sales Agency NI" badge, greyscale client logo strip, and "30-Day Guarantee" badge | Partially superseded by D-07/D-08: trust bar implemented, but without Clutch or "Rated #1" badge; replaced by "100+ meetings/month" stat badge |
| TRST-06 | About section is moved higher up the page and emphasises the NI-based, no-offshore team angle | D-12/D-13: About section repositioned between Services and Proven Across Industries. Content already emphasises NI/no-offshore differentiator — no rewrite needed |

**Note:** TRST-01 and TRST-02 (testimonials) are fully superseded by D-01 — testimonials section deleted.

</phase_requirements>

---

## Summary

Phase 3 is a purely static HTML/CSS phase with no new JavaScript. The work divides into four discrete operations: (1) insert a new `.trust-bar` section immediately after the hero closing tag, (2) replace the `.case-studies` section contents with a new `.proven` component, (3) delete the `.testimonials` scaffold section entirely, and (4) cut the `.about` section from its current position (~line 545) and paste it between `#services` and the new `.proven` section.

All CSS tokens, component patterns, and section padding conventions are already established in the codebase. No new design system decisions are needed — the UI-SPEC resolves all discretionary choices. The only external dependency is copying 5 logo PNG files from `C:\Work File\Assets\Logos\` into the repo root before implementing the trust bar.

The trust bar, "Proven Across Industries" section, and About repositioning are independent operations that can be planned as separate tasks. The testimonials deletion is trivial (one HTML block removal + its CSS block in page.css) but should be done alongside the proven section work to avoid leaving a dead CSS class.

**Primary recommendation:** Three-task plan — (1) assets + trust bar HTML/CSS, (2) proven section HTML/CSS + testimonials removal, (3) About repositioning.

---

## Standard Stack

### Core

| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| HTML5 | — | Structure | Project constraint — no frameworks |
| CSS3 custom properties | — | Tokens + layout | Already in style.css; all tokens pre-defined |
| Vanilla JS | — | No new JS this phase | Static phase only |

### Supporting

| File | Purpose | When to Use |
|------|---------|-------------|
| `style.css` | All CSS custom property tokens (colours, spacing, radius, shadows, fonts) | Source of truth for all token names |
| `page.css` | All component CSS; new classes `.trust-bar` and `.proven` authored here | Append new CSS blocks at bottom of relevant sections |
| `index.html` | Single-page HTML; all structural changes made here | Sections ordered per UI-SPEC section order table |

**No npm installs required.** No build step. No CDN additions.

---

## Architecture Patterns

### Established Section Pattern

Every section in the codebase follows an identical structure:

```html
<section class="{name}" id="{name}">
  <div class="container">
    <div class="section-header">
      <span class="section-label">{EYEBROW}</span>
      <h2 class="section-title">{Title}</h2>
      <p class="section-subtitle">{Subtitle}</p>  <!-- optional -->
    </div>
    <!-- section-specific content -->
  </div>
</section>
```

New sections MUST follow this pattern. The `.section-header`, `.section-label`, `.section-title`, and `.section-subtitle` classes are already defined in `page.css` lines 11–38 and must NOT be modified.

### Section Padding Pattern

Every content section uses this exact clamp declaration (confirmed across `.services`, `.how`, `.about`, `.case-studies`, `.channels`, `.industries`):

```css
.{section-name} {
  padding: clamp(var(--space-12), 8vw, var(--space-24)) 0;
  background: var(--section-{token});
}
```

The trust bar is a slim band, not a content section — it uses `padding: var(--space-4) 0` instead.

### Grid Pattern (2-column)

Used by `.services__grid`, `.about__diff`:

```css
.{name}__grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-6);
}
@media (min-width: 640px) {
  .{name}__grid { grid-template-columns: repeat(2, 1fr); }
}
```

The `.proven__grid` follows this same pattern for the 2×2 card layout.

### Card Pattern

Established by `.service-card`, `.pricing-card`:

```css
.{name}__card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
}
```

Cards receive `.fade-in` class in HTML (Phase 6 wires IntersectionObserver animation).

### Greyscale Logo Treatment

CSS-only, no JS required:

```css
.trust-bar__logos img {
  filter: grayscale(100%);
  opacity: 0.6;
  max-height: 40px;
  width: auto;
  transition: filter var(--transition-interactive), opacity var(--transition-interactive);
}
.trust-bar__logos img:hover {
  filter: grayscale(0%);
  opacity: 1;
}
```

This is industry-standard treatment for partner/client logo strips. The `--transition-interactive` token (`180ms cubic-bezier(0.16, 1, 0.3, 1)`) is already defined in `style.css`.

### Industry Tag Chip Pattern

Re-used from `.industry-chip` (defined at page.css line ~1119 area):

```css
.proven__card-tag {
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
  background: var(--color-primary-soft);
  color: var(--color-primary);
  font-size: var(--text-xs);
  font-weight: 700;
  border-radius: var(--radius-full);
  padding: var(--space-1) var(--space-3);
}
```

### Anti-Patterns to Avoid

- **Hardcoded hex values:** All colours must reference CSS custom properties from `style.css`. Never write `#16a34a` directly in page.css.
- **Hardcoded px/rem values:** All spacing must use `var(--space-N)` tokens. Exception: logo `max-height: 40px` (intrinsic sizing, not 8-pt grid).
- **Modifying existing CSS classes:** `.section-header`, `.section-label`, `.section-title`, `.section-subtitle`, `.btn`, `.fade-in` are shared globally. Any change breaks all sections.
- **Inline styles for layout:** All layout goes in page.css. Inline `style=""` only acceptable for one-off overrides already present in the codebase (e.g., the footer credit line).
- **New font imports:** Inter and Plus Jakarta Sans are already loaded. Do not add `@import` or new `<link>` tags.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Greyscale logo filter | Custom canvas/SVG manipulation | CSS `filter: grayscale(100%)` | Native CSS, no JS, GPU-accelerated |
| Logo hover colour reveal | JS mouseenter/mouseleave | CSS `:hover` on `img` inside `.trust-bar__logos` | Zero-JS, accessible, consistent with project constraints |
| Responsive grid collapse | JS window resize listener | CSS Grid + media query at 640px | Matches all other grids in the codebase |
| Badge/chip styling | External badge library | CSS `border-radius: var(--radius-full)` pattern | Already used by `.industry-chip` |
| Section animation hooks | Wiring IntersectionObserver now | Add `.fade-in` class to cards only; JS deferred to Phase 6 | Phase 6 is responsible for DSGN-03 |

---

## Common Pitfalls

### Pitfall 1: Inserting Trust Bar in Wrong DOM Position
**What goes wrong:** Trust bar ends up inside `.hero` or before `</head>` instead of between `</section><!-- hero -->` and `<section class="pain-points">`.
**Why it happens:** The hero section closes at line ~214 with `</section>` — easy to misread the nesting.
**How to avoid:** Verify the insertion point is after the `</section>` that closes `<section class="hero" id="hero">` and before `<section class="pain-points" id="pain-points">`.
**Warning signs:** Trust bar appears inside the hero's dark background instead of as a separate light band.

### Pitfall 2: Leaving Dead CSS for Deleted Testimonials Section
**What goes wrong:** The `.testimonials` CSS block remains in page.css after the HTML section is deleted, creating dead code.
**Why it happens:** HTML and CSS changes are made in separate tasks without cross-checking.
**How to avoid:** When deleting the testimonials HTML block, also remove `.testimonials { ... }` from page.css (lines 1152–1155).
**Warning signs:** CSS lint warnings, or confusion in Phase 6 when animation wiring finds no matching `.testimonials` elements.

### Pitfall 3: About Section Losing Its ID Anchor
**What goes wrong:** The `<section class="about" id="about">` is cut but re-pasted without the `id="about"` attribute, breaking nav links.
**Why it happens:** Careless cut-paste, especially if only the inner content (not the opening tag) is selected.
**How to avoid:** Always cut from the opening `<section` tag to the closing `</section>` tag. Verify `id="about"` is present after paste. Nav and footer both reference `#about`.
**Warning signs:** Clicking "About" in the nav scrolls nowhere; browser URL shows `#about` but no section highlights.

### Pitfall 4: Logo Files Not Copied Before Trust Bar Implementation
**What goes wrong:** Trust bar HTML references `./bluepillar-logo.png` etc. but files don't exist in repo root, producing broken image icons.
**Why it happens:** Logo copy step missed or done in wrong order.
**How to avoid:** Copy all 5 logos from `C:\Work File\Assets\Logos\` to repo root as the first task action before writing any HTML.
**Warning signs:** Broken image icons in trust bar; browser DevTools network tab shows 404 for logo filenames.
**Confirmed:** All 5 logo files ARE present at `C:\Work File\Assets\Logos\` (verified: bluepillar-logo.png, boltloop_logo.png, elevateo_logo.png, flooda_logo.png, roseyCo_logo.png).

### Pitfall 5: Using Wrong Background Token on Trust Bar
**What goes wrong:** Using `var(--section-grey)` — this token does NOT exist in style.css. The correct token is `var(--section-tint)` (#f8fafb).
**Why it happens:** REQUIREMENTS.md mentions "light grey" but the actual token in style.css is `--section-tint`.
**How to avoid:** Use only tokens confirmed in style.css :root. Verified tokens: `--section-white` (#ffffff), `--section-tint` (#f8fafb), `--section-green` (#f0fdf4). There is no `--section-grey`.

### Pitfall 6: `section-subtitle` Using Wrong Font Size Token
**What goes wrong:** Writing `font-size: var(--text-base)` for `.section-subtitle` (current page.css definition) — but UI-SPEC requires `--text-sm` for this phase's sections.
**Why it happens:** The existing `.section-subtitle` class uses `--text-base`; the UI-SPEC reduced this to `--text-sm` for Phase 3.
**How to avoid:** UI-SPEC tokens are phase-scoped decisions for NEW sections only. Do not modify the global `.section-subtitle` class. If the proven section subtitle needs `--text-sm`, add a `.proven .section-subtitle` override or accept `--text-base` from the shared class.
**Resolution:** The shared `.section-subtitle` class at `--text-base` is fine — it's close enough to `--text-sm` and modifying it would affect all sections. Leave the global class unchanged.

---

## Code Examples

### Trust Bar HTML Structure

```html
<!-- ============ TRUST BAR ============ -->
<section class="trust-bar" aria-label="Social proof">
  <div class="trust-bar__inner container">
    <div class="trust-bar__logos">
      <img src="./elevateo_logo.png" alt="Elevateo — partner" loading="lazy">
      <img src="./roseyCo_logo.png" alt="Rosey Co — partner" loading="lazy">
      <img src="./boltloop_logo.png" alt="Boltloop — partner" loading="lazy">
      <img src="./flooda_logo.png" alt="Flooda — partner" loading="lazy">
      <img src="./bluepillar-logo.png" alt="Bluepillar — partner" loading="lazy">
    </div>
    <div class="trust-bar__divider" aria-hidden="true"></div>
    <div class="trust-bar__badges">
      <span class="trust-bar__badge trust-bar__badge--stat">100+ meetings/month</span>
      <span class="trust-bar__badge trust-bar__badge--guarantee">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><polyline points="20 6 9 17 4 12"/></svg>
        30-Day Guarantee
      </span>
    </div>
  </div>
</section>
```

### Trust Bar CSS

```css
/* ============ TRUST BAR ============ */
.trust-bar {
  background: var(--section-tint);
  padding: var(--space-4) 0;
  border-bottom: 1px solid var(--color-divider);
}
.trust-bar__inner {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-8);
  flex-wrap: wrap;
}
.trust-bar__logos {
  display: flex;
  align-items: center;
  gap: var(--space-6);
  flex-wrap: wrap;
  justify-content: center;
}
.trust-bar__logos img {
  max-height: 40px;
  width: auto;
  filter: grayscale(100%);
  opacity: 0.6;
  transition: filter var(--transition-interactive), opacity var(--transition-interactive);
}
.trust-bar__logos img:hover {
  filter: grayscale(0%);
  opacity: 1;
}
.trust-bar__divider {
  width: 1px;
  height: 32px;
  background: var(--color-divider);
}
@media (max-width: 639px) {
  .trust-bar__divider { display: none; }
}
.trust-bar__badges {
  display: flex;
  align-items: center;
  gap: var(--space-4);
  flex-wrap: wrap;
  justify-content: center;
}
.trust-bar__badge {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 700;
  color: var(--color-primary);
}
```

### Proven Across Industries Section HTML

```html
<!-- ============ PROVEN ACROSS INDUSTRIES ============ -->
<section class="proven" id="case-studies">
  <div class="container">
    <div class="section-header">
      <span class="section-label">Results</span>
      <h2 class="section-title">Proven Across Industries</h2>
      <p class="section-subtitle">100+ meetings booked monthly across the Elevateo portfolio</p>
    </div>
    <div class="proven__grid">
      <div class="proven__card fade-in">
        <span class="proven__card-tag">Digital Marketing</span>
        <p class="proven__card-outcome">Consistent pipeline for a growing agency</p>
        <p class="proven__card-detail">Outbound calling + email sequences targeting UK SME decision-makers</p>
      </div>
      <div class="proven__card fade-in">
        <span class="proven__card-tag">AI &amp; Automation</span>
        <p class="proven__card-outcome">Early-stage SaaS demos booked at scale</p>
        <p class="proven__card-detail">LinkedIn prospecting + follow-up cadences for a pre-revenue product</p>
      </div>
      <div class="proven__card fade-in">
        <span class="proven__card-tag">SaaS &amp; Software</span>
        <p class="proven__card-outcome">Qualified discovery calls without an internal SDR</p>
        <p class="proven__card-detail">Multi-channel outreach across cold email and telesales</p>
      </div>
      <div class="proven__card fade-in">
        <span class="proven__card-tag">E-Commerce</span>
        <p class="proven__card-outcome">New B2B retail partnerships opened monthly</p>
        <p class="proven__card-detail">Targeted cold outreach to buyers at independent and chain retailers</p>
      </div>
    </div>
  </div>
</section>
```

### Proven Section CSS

```css
/* ============ PROVEN ACROSS INDUSTRIES ============ */
.proven {
  padding: clamp(var(--space-12), 8vw, var(--space-24)) 0;
  background: var(--section-white);
}
.proven__grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-6);
}
@media (min-width: 640px) {
  .proven__grid { grid-template-columns: repeat(2, 1fr); }
}
.proven__card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
}
.proven__card-tag {
  display: inline-flex;
  align-items: center;
  background: var(--color-primary-soft);
  color: var(--color-primary);
  font-family: var(--font-body);
  font-size: var(--text-xs);
  font-weight: 700;
  border-radius: var(--radius-full);
  padding: var(--space-1) var(--space-3);
  margin-bottom: var(--space-3);
}
.proven__card-outcome {
  font-family: var(--font-display);
  font-size: var(--text-lg);
  font-weight: 700;
  color: var(--color-text);
  line-height: 1.3;
  margin-bottom: var(--space-2);
}
.proven__card-detail {
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 400;
  color: var(--color-text-muted);
  line-height: 1.6;
}
```

---

## Section Order After Phase 3

The target section order (from UI-SPEC):

1. Hero (`#hero`)
2. **Trust Bar** — insert after hero close tag
3. Pain Points (`#pain-points`)
4. Services (`#services`)
5. **About** (`#about`) — moved from position 10
6. Multi-Channel (`#channels`)
7. How It Works (`#how-it-works`)
8. Industries (`#industries`)
9. **Proven Across Industries** (`#case-studies`) — replaces case-studies scaffold
10. Comparison Table (`#comparison`) — scaffold, Phase 4
11. Pricing (`#pricing`)
12. FAQ (`#faq`) — scaffold, Phase 4
13. Booking (`#booking`) — scaffold, Phase 5
14. CTA Banner (`#cta`)
15. Footer

**Critical:** The `#case-studies` anchor ID must be preserved on the new `.proven` section — nav and footer links use `href="#case-studies"`.

---

## Environment Availability

This phase has no external tool dependencies. All work is HTML/CSS file editing.

| Dependency | Required By | Available | Notes |
|------------|------------|-----------|-------|
| Logo PNGs (5 files) | Trust bar (D-09) | YES | Confirmed present at `C:\Work File\Assets\Logos\` |
| CSS custom properties | All new CSS | YES | All tokens verified in `style.css :root` |
| Browser (for verification) | Human verify checkpoint | YES | Standard dev workflow |

**No missing dependencies.**

---

## Validation Architecture

### Test Framework

| Property | Value |
|----------|-------|
| Framework | None — static HTML/CSS, visual verification only |
| Config file | None |
| Quick run command | Open `index.html` in browser |
| Full suite command | Open `index.html` in browser, scroll all sections |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| TRST-03 | 4 industry cards with tag + outcome + detail visible | Manual visual | Open browser, inspect #case-studies section | ❌ Wave 0 (manual only) |
| TRST-04 | Cards show industry tag chip (green) | Manual visual | Open browser, verify chip styling | ❌ Wave 0 (manual only) |
| TRST-05 | Trust bar visible below hero with logos + badges | Manual visual | Open browser, verify trust bar between hero and pain points | ❌ Wave 0 (manual only) |
| TRST-06 | About section appears between Services and Proven Across Industries | Manual visual | Open browser, scroll page flow | ❌ Wave 0 (manual only) |

**All tests are manual-only.** This is a static HTML/CSS phase with no logic to unit test. Verification is by browser inspection.

### Sampling Rate

- **Per task commit:** Open index.html in browser, verify the changed section renders correctly
- **Per wave merge:** Scroll full page, verify section order and visual treatment
- **Phase gate:** All 4 TRST requirements visually confirmed before `/gsd:verify-work`

### Wave 0 Gaps

None for framework. All "gaps" are manual verification checkpoints built into the plan tasks.

---

## Key Findings Summary

1. **Logo assets confirmed:** All 5 PNG files exist at `C:\Work File\Assets\Logos\` with exact filenames specified in D-09. Copy step is straightforward — no renaming needed.

2. **No `--section-grey` token:** style.css has `--section-tint` (#f8fafb), `--section-white` (#ffffff), and `--section-green` (#f0fdf4). Any reference to `--section-grey` would be a bug. Trust bar uses `--section-tint`.

3. **`id="case-studies"` must be preserved:** The new `.proven` section replaces `.case-studies` in content but the `id` attribute must stay `case-studies` because nav (`href="#case-studies"`) and footer (`href="#case-studies"`) both reference it.

4. **About section CSS is complete:** `.about` CSS at page.css lines 823–871 needs zero changes. Only the HTML position changes.

5. **Testimonials CSS must also be deleted:** The `.testimonials` CSS block at page.css lines 1152–1155 is dead code after the section is removed from HTML. Remove it alongside the HTML deletion.

6. **No new JavaScript:** This phase explicitly adds zero JS. The `<script>` block at the bottom of `<body>` is not touched.

7. **`section-subtitle` font-size discrepancy resolved:** Existing global `.section-subtitle` uses `--text-base`; UI-SPEC specifies `--text-sm`. Do not modify the global class — the difference is negligible and modifying it breaks all sections. Accept `--text-base` from the shared class.

---

## Sources

### Primary (HIGH confidence)

- `index.html` — direct inspection of current HTML structure, confirmed section positions and existing anchor IDs
- `page.css` — direct inspection of all existing CSS, confirmed token usage patterns, confirmed `.testimonials` at lines 1152–1155 and `.case-studies` at lines 1173–1181, `.about` at lines 823–871
- `style.css` — direct inspection of all CSS custom properties (`:root`), confirmed available tokens
- `03-CONTEXT.md` — 13 locked decisions covering all Phase 3 scope
- `03-UI-SPEC.md` — full component inventory, copy spec, spacing/colour/typography decisions

### Secondary (MEDIUM confidence)

- `C:\Work File\Assets\Logos\` directory listing — confirmed 5 logo files present with exact filenames

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — codebase fully inspected, no external dependencies
- Architecture patterns: HIGH — all patterns verified from existing page.css and index.html
- Pitfalls: HIGH — identified from direct code inspection (wrong token name, dead CSS, anchor ID risk)
- Asset availability: HIGH — directory listing confirmed

**Research date:** 2026-04-27
**Valid until:** Stable — no external libraries or APIs involved. Valid until codebase changes.
