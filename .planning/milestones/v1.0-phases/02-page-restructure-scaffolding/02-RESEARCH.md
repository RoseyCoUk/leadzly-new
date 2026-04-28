# Phase 2: Page Restructure & Scaffolding — Research

**Researched:** 2026-04-24
**Domain:** Static HTML section ordering, CSS background alternation, semantic markup for B2B landing page sections
**Confidence:** HIGH — all findings are based on direct code inspection of the existing codebase plus established HTML/CSS patterns with no external dependencies required

---

## Summary

Phase 2 is a structural HTML editing task on a static `index.html` page. There is no framework, no build tool, and no package installation involved. The work divides into two categories: (1) reordering and augmenting existing sections, and (2) adding four brand-new sections (pain points, multi-channel, industries, and placeholder slots for case studies/testimonials/comparison/FAQ/booking/footer CTA).

The existing codebase is well-structured. A CSS design system in `style.css` provides all the tokens needed — colours, spacing, type scale, shadows. The `page.css` file contains all section styles. The pattern established across all existing sections is consistent: a `<section class="[name]" id="[anchor]">` wrapper, a `.container` inner div, a `.section-header` with `.section-label` + `.section-title`, and a content grid. New sections must follow this exact pattern.

The primary risk is incorrect section ordering in the HTML — the target order from PAGE-01 is the source of truth and must be followed precisely. The secondary risk is background alternation: dark navy sections must set their own text colours explicitly (white text on dark) because the design system tokens default to light-mode text colours.

**Primary recommendation:** Edit `index.html` to reorder and insert sections in a single focused pass, adding corresponding CSS classes to `page.css`. Do not touch `style.css` (the design system is complete). Do not add any JavaScript in this phase — all new sections are static HTML/CSS only.

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| PAGE-01 | Page sections ordered: hero → trust bar → pain points → services → multi-channel → process → industries → case studies → testimonials → comparison table → pricing → FAQ → booking → footer CTA → footer | Section audit below confirms current order and what must move/be inserted |
| PAGE-02 | Pain points section: headline "Does this sound familiar?" + 6 empathetic problem cards | New section — markup and content defined in this research |
| PAGE-03 | Multi-channel section on dark navy background with 4 channel cards | New section — dark navy pattern from existing `.cta-banner` and `[data-theme="dark"]` shows correct approach |
| PAGE-04 | Industries section: 12-chip grid with icon + label | New section — CSS chip/tag pattern already established in `.result-card__tag` |
| DSGN-02 | Sections alternate between white, light grey (#fafafa), dark navy (#0f172a), and green-tinted (#f0fdf4) backgrounds | Background colour map defined in this research |
</phase_requirements>

---

## Current Section Inventory (what exists in index.html today)

| HTML Order | Section | id | Current Background | CSS Class |
|------------|---------|----|--------------------|-----------|
| 1 | Hero | `#hero` | Dark navy (image + overlay) | `.hero` |
| 2 | Trust Bar | `#trust` | White (no bg set) | `.trust` |
| 3 | Services | `#services` | White (no bg set) | `.services` |
| 4 | How It Works (Process) | `#how-it-works` | Light grey (`var(--color-surface)`) | `.how` |
| 5 | Results (Case Studies placeholder) | `#results` | White (no bg set) | `.results` |
| 6 | Pricing | `#pricing` | Light grey (`var(--color-surface)`) | `.pricing` |
| 7 | About | `#about` | White (no bg set) | `.about` |
| 8 | CTA Banner (footer CTA) | `#cta` | Green gradient | `.cta-banner` |
| — | Footer | `#footer` | Dark navy | `.footer` |

**Sections that do NOT exist yet (must be created):**
- Pain Points (`#pain-points`) — PAGE-02
- Multi-Channel Breakdown (`#channels`) — PAGE-03
- Industries (`#industries`) — PAGE-04
- Case Studies (placeholder shell) — Phase 3 adds content, Phase 2 adds the `<section>` wrapper
- Testimonials (placeholder shell) — Phase 3 adds content, Phase 2 adds the `<section>` wrapper
- Comparison Table (placeholder shell) — Phase 4 adds content, Phase 2 adds the `<section>` wrapper
- FAQ (placeholder shell) — Phase 4 adds content, Phase 2 adds the `<section>` wrapper
- Booking (placeholder shell) — Phase 5 adds content, Phase 2 adds the `<section>` wrapper

---

## Target Section Order (PAGE-01)

The final HTML order inside `<main>` must be:

```
1.  #hero              (exists — no change)
2.  #trust             (exists — stays in place)
3.  #pain-points       (NEW — PAGE-02)
4.  #services          (exists — move after pain points)
5.  #channels          (NEW — PAGE-03, dark navy)
6.  #how-it-works      (exists — move after channels)
7.  #industries        (NEW — PAGE-04)
8.  #case-studies      (scaffold only — Phase 3 fills content)
9.  #testimonials      (scaffold only — Phase 3 fills content)
10. #comparison        (scaffold only — Phase 4 fills content)
11. #pricing           (exists — move here)
12. #faq               (scaffold only — Phase 4 fills content)
13. #booking           (scaffold only — Phase 5 fills content)
14. #cta               (exists — stays)
Footer
```

**Sections that need to move (HTML reorder only, no content change):**
- `#services` — currently 3rd, needs to be 4th (after pain points)
- `#how-it-works` — currently 4th, needs to be 6th (after channels)
- `#results` — rename to `#case-studies`, move to 8th slot (was 5th)
- `#pricing` — currently 6th, needs to be 11th
- `#about` — currently 7th. **Note:** TRST-06 says About moves higher — but that is a Phase 3 requirement. Phase 2 should keep About near its current position or omit it from the ordered list. Safe approach: retain About between pricing and FAQ as a placeholder; Phase 3 will relocate it.

---

## Background Alternation Map (DSGN-02)

The required alternation pattern across all 15 sections:

| # | Section | Background | CSS Implementation |
|---|---------|------------|-------------------|
| 1 | Hero | Dark navy | Already done (`.hero__overlay` + bg image) |
| 2 | Trust Bar | White | Remove any bg or set `background: var(--color-bg)` |
| 3 | Pain Points | Light grey `#fafafa` | `background: var(--color-surface)` |
| 4 | Services | White | `background: var(--color-bg)` (currently no bg — correct) |
| 5 | Multi-Channel | Dark navy `#0f172a` | `background: var(--color-dark)` + white text |
| 6 | Process (How It Works) | Light grey | Already `background: var(--color-surface)` — correct |
| 7 | Industries | Green-tinted `#f0fdf4` | `background: var(--color-primary-soft)` |
| 8 | Case Studies | White | `background: var(--color-bg)` |
| 9 | Testimonials | Light grey | `background: var(--color-surface)` |
| 10 | Comparison Table | Dark navy | `background: var(--color-dark)` + white text |
| 11 | Pricing | Light grey | Already `background: var(--color-surface)` — correct |
| 12 | FAQ | White | `background: var(--color-bg)` |
| 13 | Booking | Green-tinted | `background: var(--color-primary-soft)` |
| 14 | Footer CTA | Green gradient | Already done (`.cta-banner`) |
| Footer | Footer | Dark navy | Already done (`.footer`) |

**Key token mapping (from `style.css`):**
- White: `var(--color-bg)` = `#ffffff`
- Light grey: `var(--color-surface)` = `#f9fafb` (close enough to `#fafafa`)
- Dark navy: `var(--color-dark)` = `#0f172a`
- Green-tinted: `var(--color-primary-soft)` = `#f0fdf4`

---

## Standard Stack

No new libraries. This phase is pure HTML/CSS edits.

| Tool | Version | Purpose |
|------|---------|---------|
| HTML5 semantic elements | — | `<section>`, `<article>`, proper landmark roles |
| CSS custom properties | — | All from existing `style.css` — no new tokens needed |
| Inline SVG | — | Icons inside channel cards and industry chips |

**Installation:** None required.

---

## Architecture Patterns

### Established Section Pattern (copy exactly for all new sections)

Every section in the existing codebase follows this structure — new sections must match:

```html
<!-- Source: index.html — services section pattern -->
<section class="[section-name]" id="[anchor]">
  <div class="container">
    <div class="section-header">
      <span class="section-label">[Eyebrow label in green]</span>
      <h2 class="section-title">[Main heading]</h2>
      <!-- Optional: <p class="section-subtitle">...</p> -->
    </div>
    <!-- Section-specific content grid -->
  </div>
</section>
```

CSS for each new section follows the established section padding pattern:

```css
/* Source: page.css — services section padding pattern */
.section-name {
  padding: clamp(var(--space-12), 8vw, var(--space-24)) 0;
  /* background set according to alternation map */
}
```

### Dark Navy Section Pattern

The multi-channel section and the comparison table (Phase 4) sit on dark navy. The existing `.cta-banner` shows how to handle dark sections — but the multi-channel section needs explicit text colour overrides since the design system tokens default to light-mode values:

```css
/* Pattern derived from existing .cta-banner and .footer */
.channels {
  padding: clamp(var(--space-12), 8vw, var(--space-24)) 0;
  background: var(--color-dark);
}
.channels .section-label {
  color: var(--color-primary); /* green — readable on dark */
}
.channels .section-title {
  color: #ffffff; /* explicit white — do not rely on --color-text */
}
.channels .section-subtitle {
  color: rgba(255, 255, 255, 0.7);
}
```

### Green-Tinted Section Pattern

For industries and booking sections:

```css
.industries {
  padding: clamp(var(--space-12), 8vw, var(--space-24)) 0;
  background: var(--color-primary-soft); /* #f0fdf4 */
}
```

### Card Grid Pattern

All existing card grids use CSS Grid with a 1-column mobile base expanding to multi-column at breakpoints. The service-card pattern is the canonical example:

```css
/* Source: page.css — services__grid pattern */
.new-section__grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-6);
}
@media (min-width: 640px) {
  .new-section__grid { grid-template-columns: repeat(2, 1fr); }
}
@media (min-width: 1024px) {
  .new-section__grid { grid-template-columns: repeat(3, 1fr); }
}
```

For a 6-card grid (pain points), use repeat(2, 1fr) at 640px and repeat(3, 1fr) at 1024px.
For a 4-card grid (channels), use repeat(2, 1fr) at 640px and repeat(4, 1fr) at 1024px.

### Industry Chip Pattern

The existing `.result-card__tag` pill pattern can be extended for chips:

```css
/* Source: page.css — result-card__tag */
.industry-chip {
  display: inline-flex;
  align-items: center;
  gap: var(--space-2);
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 600;
  color: var(--color-primary);
  background: var(--color-primary-highlight);
  padding: var(--space-3) var(--space-5);
  border-radius: var(--radius-full);
  border: 1px solid oklch(from var(--color-primary) l c h / 0.15);
}
```

The 12-chip industries grid should use `display: flex; flex-wrap: wrap; gap: var(--space-3)` rather than CSS grid, since chips are variable width and should flow naturally.

### Placeholder Scaffold Pattern

For sections added in Phase 2 but filled with content in later phases (case studies, testimonials, comparison, FAQ, booking), add minimal placeholder HTML:

```html
<!-- ============ CASE STUDIES (scaffold — content in Phase 3) ============ -->
<section class="case-studies" id="case-studies">
  <div class="container">
    <div class="section-header">
      <span class="section-label">Case Studies</span>
      <h2 class="section-title">Results That Speak for Themselves</h2>
    </div>
    <!-- Cards added in Phase 3 -->
  </div>
</section>
```

This approach keeps the section in the correct DOM position and gives Phase 3/4/5 a clean insertion point without any reordering.

### fade-in Class Convention

All content cards in the existing codebase carry `class="... fade-in"` for the IntersectionObserver reveal animation. New cards must include `fade-in` in their class list. The IntersectionObserver JS is already wired up in the inline `<script>` block and targets `document.querySelectorAll('.fade-in')` — no JS changes are needed.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Section backgrounds on dark navy | Custom colour hex strings | `var(--color-dark)` token from `style.css` | Stays consistent with dark mode toggle and design system |
| Responsive grids | Custom media queries per section | Follow established `640px` / `1024px` breakpoints from `page.css` | All existing grids use these exact breakpoints — consistency |
| Green chip styling | New design | Extend `.result-card__tag` pattern | Already established and visually consistent |
| Fade-in animation | New JS | Add `class="fade-in"` to cards | IntersectionObserver already wired up globally |
| Icon SVGs | External library | Inline SVG paths (consistent with all existing icons) | No new dependencies; existing icons are all inline SVGs at 24x24 with `stroke="currentColor"` |

---

## Pain Points Section Content (PAGE-02)

The 6 problem cards for "Does this sound familiar?":

| # | Problem | Headline | Body copy direction |
|---|---------|----------|---------------------|
| 1 | Empty pipeline | "Your pipeline looks thinner every week" | Closers waiting for leads that never arrive |
| 2 | Closers stuck prospecting | "Your best salespeople are stuck cold calling" | They should be closing, not sourcing |
| 3 | SDR hire cost/risk | "Hiring an SDR feels like a £40k+ gamble" | 6-month ramp, no guarantee, all the risk |
| 4 | Unqualified leads | "You're drowning in meetings that go nowhere" | Bad fit prospects clog your calendar |
| 5 | Burned by agency | "You've tried an agency before and been let down" | Vanity metrics, no meetings, gone by month 3 |
| 6 | No outbound visibility | "You have no idea what outbound even costs you" | No tracking, no attribution, no predictability |

Each card should be empathetic in tone — validating the pain, not lecturing. Use a warning/empathy icon (SVG), the pain headline as an `<h3>`, and 1–2 sentences of copy.

---

## Multi-Channel Section Content (PAGE-03)

4 channel cards on dark navy:

| Channel | Icon suggestion | Description |
|---------|----------------|-------------|
| Telesales | Phone SVG | Human-to-human cold calling. Direct, personal, high-conversion. |
| Cold Email | Envelope SVG | Targeted email sequences that cut through noise. |
| LinkedIn | Network/person SVG | Social selling that warms prospects before the call. |
| Follow-Up | Refresh/arrow SVG | Systematic multi-touch follow-up so no lead goes cold. |

On dark navy, card backgrounds should use `var(--color-dark-surface)` (`#1e293b`) with `var(--color-dark-border)` (`#334155`) borders, matching the footer's inner contrast pattern.

---

## Industries Section Content (PAGE-04)

12 chips, each with a small inline SVG icon + text label:

| Industry | Icon direction |
|----------|---------------|
| SaaS | Cloud/code |
| Recruitment | People |
| E-commerce | Shopping bag |
| Professional Services | Briefcase |
| Financial Services | Chart/coin |
| Construction | Building/hard hat |
| Healthcare | Heart/cross |
| Logistics | Truck/package |
| Education | Book/graduation cap |
| Marketing Agencies | Megaphone |
| Managed IT | Server/shield |
| Energy | Lightning bolt |

Use 16x16 SVG icons inline with `stroke="currentColor"` at 1.5 stroke-width to match the rest of the icon set.

---

## Common Pitfalls

### Pitfall 1: Text Colour on Dark Navy Sections
**What goes wrong:** Adding `background: var(--color-dark)` to a section without overriding text colours — paragraph text becomes invisible because `--color-text` is `#1a1a2e` (dark text for light backgrounds).
**Why it happens:** The design system is light-mode first; CSS custom properties cascade but don't auto-invert for dark sections.
**How to avoid:** For every dark navy section, explicitly set `.section-title { color: #ffffff }`, `.section-subtitle { color: rgba(255,255,255,0.7) }`, and card body text to `rgba(255,255,255,0.8)`. Do NOT rely on `var(--color-text-inverse)` as a shorthand — it only equals `#ffffff` in light mode.
**Warning signs:** Section looks fine in light mode but body text disappears on dark bg.

### Pitfall 2: Wrong Section Order After Edits
**What goes wrong:** HTML sections get placed in the wrong position during a multi-section edit, especially when moving existing sections and inserting new ones simultaneously.
**Why it happens:** Large HTML edits across 400+ lines lose spatial context.
**How to avoid:** Work in two passes: (1) insert all new sections in their final positions using placeholder comment markers first, then (2) move existing sections to match the target order. Verify by scanning all `<section` opening tags from top to bottom after each pass.
**Warning signs:** `#services` appears before `#pain-points`, or `#pricing` appears before `#testimonials`.

### Pitfall 3: Breaking the fade-in Observer
**What goes wrong:** New cards are added without `class="fade-in"`, meaning they don't animate — OR they are added with `fade-in` but are inside a wrapper the observer doesn't reach.
**Why it happens:** The IntersectionObserver in the existing `<script>` block queries `document.querySelectorAll('.fade-in')` once on page load. Cards added correctly will be picked up.
**How to avoid:** Add `fade-in` to every `.service-card`-equivalent element in new sections. No JS changes needed.
**Warning signs:** New cards appear instantly (no fade animation) or flash on page load without the subtle delay.

### Pitfall 4: Duplicate anchor IDs
**What goes wrong:** The existing `#results` section becomes `#case-studies` but nav links still point to `#results`.
**Why it happens:** Nav links reference the old anchor.
**How to avoid:** When renaming `id="results"` to `id="case-studies"`, simultaneously update the nav `<a href="#results">Results</a>` link and the footer column link.
**Warning signs:** Clicking "Results" in the nav does nothing (hash not found).

### Pitfall 5: Scaffold Sections with Visible Empty Space
**What goes wrong:** Placeholder sections (case studies, testimonials, etc.) render with padding but no content — creating large blank gaps on the page during Phase 2.
**Why it happens:** The section has padding but no content between the `.section-header` and the closing `</section>`.
**How to avoid:** Include a minimum placeholder in each scaffold section: the `.section-header` is sufficient. Optionally add a `<!-- Content added in Phase X -->` comment inside. Do NOT add padding to a section that has only a heading — the heading itself provides enough visual anchor. Alternatively, add a brief "coming soon" placeholder paragraph styled as `color: var(--color-text-faint)` to make scaffolding visually intentional rather than broken.

---

## Architecture: Recommended Edit Order

To minimise merge conflicts and make each edit atomic:

1. **Pass 1 — HTML reorder and insert:** Edit `index.html` to achieve the correct section order. Move existing sections, add new full sections (pain points, multi-channel, industries), and add scaffold sections (case studies, testimonials, comparison, FAQ, booking). Update nav link anchors if needed.

2. **Pass 2 — Background CSS:** Add background declarations to new sections in `page.css`. Fix text colour overrides for dark navy sections.

3. **Pass 3 — New section CSS:** Add full CSS for `.pain-points`, `.channels`, `.industries` grids and cards to `page.css`.

This maps cleanly to 3 plans: HTML restructure → background/colour CSS → new section CSS.

---

## State of the Art

| Old Approach | Current Approach | Impact for This Phase |
|--------------|------------------|----------------------|
| Separate CSS file per section | Single `page.css` for all sections | Append all new section CSS to `page.css` — one file to edit |
| `display: table` for multi-column layouts | CSS Grid with `grid-template-columns` | All new grids use CSS Grid, not floats or tables |
| `px` units for spacing | `clamp()` + CSS custom property spacing scale | Use `var(--space-N)` tokens, not hardcoded pixel values |
| Fixed font sizes | Fluid type scale via `clamp()` | Use `var(--text-N)` tokens — do not hardcode `font-size` |

---

## Environment Availability

Step 2.6: SKIPPED — this phase is purely HTML/CSS edits with no external tools, CLIs, runtimes, or services required. No environment check needed.

---

## Validation Architecture

`nyquist_validation` is enabled. However, this project is a static HTML/CSS site with no test framework configured or required. Validation for Phase 2 is visual/manual.

### Test Framework

| Property | Value |
|----------|-------|
| Framework | None — static HTML/CSS, no automated test runner |
| Config file | N/A |
| Quick run command | Open `index.html` in browser |
| Full suite command | Scroll top-to-bottom, verify section order, verify backgrounds, verify new sections visible |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| PAGE-01 | Sections appear in correct order on scroll | Visual/manual | Open `index.html`, scroll top to bottom | N/A — no test file |
| PAGE-02 | Pain points section visible with 6 cards | Visual/manual | Inspect DOM — `document.querySelectorAll('#pain-points .pain-card').length === 6` in DevTools | N/A |
| PAGE-03 | Multi-channel section visible on dark navy | Visual/manual | Check `#channels` background computed style | N/A |
| PAGE-04 | Industries section visible with 12 chips | Visual/manual | `document.querySelectorAll('#industries .industry-chip').length === 12` in DevTools | N/A |
| DSGN-02 | Background alternation visible on scroll | Visual/manual | Scroll page, observe section backgrounds alternate | N/A |

### Wave 0 Gaps

None — no automated test framework is applicable for this phase. All verification is browser visual inspection. The planner should include explicit "open in browser and verify" steps in plan tasks.

---

## Open Questions

1. **About section position in Phase 2**
   - What we know: TRST-06 (Phase 3) says to move About higher. Phase 2 success criteria do not mention About.
   - What's unclear: Should About be included in the Phase 2 section order at all? The PAGE-01 target order does not list it — it only specifies the 15 sections.
   - Recommendation: Retain `#about` in the HTML between `#pricing` and `#faq` as a quiet placeholder. Phase 3 will relocate it. This avoids disrupting it twice.

2. **`#results` rename to `#case-studies`**
   - What we know: The existing section is `id="results"` with class `.results`. Phase 2 requires a case studies section in slot 8.
   - What's unclear: Should we rename the existing section in-place, or treat it as a scaffold and replace entirely?
   - Recommendation: Rename the existing section to `id="case-studies"` and `class="case-studies"` to match future Phase 3 patterns. The existing result cards are placeholder content only — Phase 3 replaces them with proper case study cards. Update nav link `href="#results"` to `href="#case-studies"`.

3. **Nav links update**
   - What we know: The nav currently has: Services, How It Works, Results, Pricing, About.
   - What's unclear: Should the nav be updated in Phase 2 to reflect new section anchors?
   - Recommendation: Yes — update "Results" to "Case Studies" (`href="#case-studies"`). New sections (pain points, channels, industries) do not need nav links — the nav is for primary navigation, not every section.

---

## Sources

### Primary (HIGH confidence)
- Direct read of `index.html` (all existing sections, their IDs, classes, current order)
- Direct read of `style.css` (all CSS custom property tokens used in recommendations)
- Direct read of `page.css` (all section padding patterns, card patterns, grid breakpoints)
- Direct read of `base.css` (reset, utility classes including `.fade-in`)
- `.planning/REQUIREMENTS.md` — PAGE-01 through PAGE-04, DSGN-02 verbatim requirements
- `.planning/ROADMAP.md` — phase success criteria
- `.planning/phases/01-foundation-hygiene/01-CONTEXT.md` — established conventions (IIFE JS pattern, design token usage)

### Secondary (MEDIUM confidence)
- N/A — all research grounded in direct codebase inspection

### Tertiary (LOW confidence)
- N/A

---

## Metadata

**Confidence breakdown:**
- Current section inventory: HIGH — read directly from index.html
- Target section order: HIGH — read directly from REQUIREMENTS.md PAGE-01
- Background alternation map: HIGH — derived from DSGN-02 requirement + design token values in style.css
- New section content (pain points, channels, industries): HIGH — content verbatim from PAGE-02, PAGE-03, PAGE-04 requirements
- CSS patterns for new sections: HIGH — derived from existing page.css conventions

**Research date:** 2026-04-24
**Valid until:** Stable — no external dependencies; valid until codebase changes
