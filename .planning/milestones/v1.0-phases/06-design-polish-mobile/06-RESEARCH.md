# Phase 6: Design Polish & Mobile — Research

**Researched:** 2026-04-28
**Domain:** CSS hover effects, IntersectionObserver scroll animation, responsive CSS grid breakpoints, mobile layout
**Confidence:** HIGH

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| DSGN-03 | All cards lift on hover (translateY(-4px) + box-shadow) and show a green border highlight; sections fade up on scroll via IntersectionObserver | Card class inventory complete; partial fade-in system already exists — needs translateY upgrade and section-level observer |
| DSGN-04 | All grids collapse to single column on mobile; process steps stack vertically without arrows; comparison table scrolls horizontally on small screens | Grid audit complete; how__connector already hidden on mobile; comparison__table-wrap already has overflow-x:auto — gaps identified |
</phase_requirements>

---

## Summary

Phase 6 is a pure CSS/JS polish layer on top of a completed page. The HTML and content are done; this phase adds interaction fidelity (hover lifts, scroll fade-up) and ensures the layout degrades correctly on 375px mobile viewports. No new sections, no new content — only CSS rules and minor JS additions.

A partial fade-in system already exists. The JS observer in `index.html` (lines 981–1008) handles opacity only — no translateY. The `.fade-in` CSS class in `page.css` (lines 93–115) already transitions opacity and includes a CSS scroll-driven animation fallback (`animation-timeline: view()`). Phase 6 must upgrade the fade-in to include vertical translate and must apply the observer to sections as well as cards.

Hover effects exist on `service-card` and `channel-card` but are partial — they only add `box-shadow` and slightly adjust `border-color`. They do not do `translateY(-4px)` or add the required green border highlight. All other card types (pain-card, proven__card, pricing-card) have no hover state at all. Phase 6 must add the full hover pattern to every card class.

Mobile is mostly functional but has two documented gaps: (1) the booking grid uses `grid-template-columns: 1fr 1.2fr` with a breakpoint only at `max-width: 767px` — needs verification at 375px; (2) the `how__step` process steps already stack vertically on mobile and `how__connector` arrows are already `display:none` by default — this requirement is already met.

**Primary recommendation:** Two plans — one CSS-only plan (hover effects + translateY on fade-in + mobile grid fixes), one JS plan (upgrade scroll observer to include translateY, add section-level fade-up) — with a combined human-verify checkpoint at the end.

---

## Project Constraints (from CLAUDE.md)

- HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- No third-party JS libraries (e.g. Swiper, GSAP, AOS)
- No new font imports
- Calendly via CDN widget script only
- No backend, no API calls
- Single-page (index.html) — all edits go to index.html, style.css, base.css, or page.css

---

## Standard Stack

### Core (what is already in use)
| Technology | Version | Purpose |
|------------|---------|---------|
| Vanilla CSS | N/A | All layout, animation, hover |
| IntersectionObserver API | Native browser | Scroll reveal — already in use for .fade-in |
| CSS custom properties | N/A | Design tokens (colours, shadows, spacing, transitions) |
| CSS Grid | N/A | Section and card grids |

### Relevant Design Tokens Already Defined (style.css)
| Token | Value | Used For |
|-------|-------|---------|
| `--shadow-md` | `0 4px 12px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04)` | Base card hover shadow |
| `--shadow-lg` | `0 12px 32px rgba(0,0,0,0.10), 0 4px 8px rgba(0,0,0,0.04)` | Lifted card shadow |
| `--color-primary` | `#16a34a` | Green border highlight colour |
| `--transition-interactive` | `180ms cubic-bezier(0.16, 1, 0.3, 1)` | Shared transition timing |

---

## Architecture Patterns

### Existing Fade-In System (MUST understand before modifying)

**Current JS (index.html lines 981–1008):**
- Selects all `.fade-in` elements
- Sets `fade-in--hidden` (opacity: 0) only on elements below the fold
- On intersection: removes `fade-in--hidden`, adds `fade-in--visible` with staggered delay (60ms per sibling index)
- Uses `threshold: 0.08, rootMargin: '0px 0px -20px 0px'`
- Calls `observer.unobserve()` after trigger — plays once

**Current CSS (page.css lines 93–115):**
```css
.fade-in {
  opacity: 1;
  transition: opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}
.fade-in--hidden { opacity: 0; }
.fade-in--visible { opacity: 1; }
```

**Problem for DSGN-03:** The requirement says "sections fade up" — implying `translateY` from below. The current system only fades opacity. The upgrade must add `translateY(20px)` to `--hidden` and `translateY(0)` to `--visible`, and the `transition` property must include `transform`.

**Critical constraint:** The CSS scroll-driven fallback (`animation-timeline: view()`) at lines 104–115 already handles opacity. If translateY is added, this fallback must also be updated or it will conflict. The safest approach: extend the `fadeReveal` keyframe to include `transform: translateY(20px) → translateY(0)`.

**`prefers-reduced-motion`:** base.css (lines 43–50) already disables all animations/transitions when reduced motion is preferred. No additional work needed — the upgrade will automatically be suppressed.

### Hover Pattern — Target State (DSGN-03)

The requirement specifies:
- `transform: translateY(-4px)` — lift
- Elevated `box-shadow` — use `--shadow-lg` (stronger than current `--shadow-md`)
- Visible green border highlight — `border-color: var(--color-primary)`

**Pattern (apply to every card class):**
```css
.card-class {
  transition: transform var(--transition-interactive),
              box-shadow var(--transition-interactive),
              border-color var(--transition-interactive);
}
.card-class:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--color-primary);
}
```

### Card Class Inventory (complete audit of index.html)

Every card class that needs the hover treatment:

| Card Class | Location | Current Hover State | Has `transition` already |
|------------|----------|---------------------|--------------------------|
| `.pain-card` | pain-points section | None | No |
| `.service-card` | services section | `box-shadow: --shadow-md` + mild border | Yes (partial — missing transform) |
| `.channel-card` | channels section | `box-shadow: --shadow-md` + mild border | Yes (partial — missing transform) |
| `.how__step` | how-it-works section | None | No |
| `.proven__card` | proven section | None | No |
| `.pricing-card` | pricing section | None (featured has static box-shadow) | No |
| `.roi__card` | ROI section | None | No |
| `.about__diff-item` | about section | None | No |

**Note on `.pricing-card--featured`:** It has `transform: scale(1.03)` on desktop (min-width: 768px). The hover lift must be additive: `translateY(-4px)` on top of `scale(1.03)` — use `transform: scale(1.03) translateY(-4px)`. On mobile where scale is not applied, just `translateY(-4px)`.

**Note on `.how__step`:** These are inside `.how__timeline` (flex row on desktop). Hover lift works fine. No connector-specific issues.

### Mobile Grid Requirements (DSGN-04)

**Target viewport:** 375px

**Audit of existing breakpoints:**

| Grid | Current Breakpoint | Mobile (375px) behaviour | Gap |
|------|-------------------|--------------------------|-----|
| `services__grid` | `min-width: 640px` → 2-col | Single column at 375px | None — already correct |
| `pain-points__grid` | `min-width: 640px` → 2-col, `min-width: 1024px` → 3-col | Single column at 375px | None — already correct |
| `channels__grid` | `min-width: 640px` → 2-col, `min-width: 1024px` → 4-col | Single column at 375px | None — already correct |
| `proven__grid` | `min-width: 640px` → 2-col | Single column at 375px | None — already correct |
| `pricing__grid` | `min-width: 768px` → 3-col | Single column at 375px | None — already correct |
| `roi__inputs` | `min-width: 768px` → 3-col | Single column at 375px | None — already correct |
| `roi__outputs` | `min-width: 640px` → 2-col | Single column at 375px | None — already correct |
| `booking__grid` | `max-width: 767px` → 1-col | Single column at 375px | None — already correct |
| `footer__grid` | `min-width: 768px` → 4-col | Single column at 375px | None — already correct |
| `about__diff` | `min-width: 640px` → 3-col | Single column at 375px | None — already correct |
| `comparison__table-wrap` | `overflow-x: auto` + `min-width: 560px` on table | Scrolls horizontally | None — already correct per DSGN-04 |
| `how__timeline` + connectors | `min-width: 768px` → row + connectors visible | Steps stack vertically, connectors hidden (`display:none` default) | None — already correct |

**Key finding:** The grids and process step layout for DSGN-04 are already substantially implemented. The comparison table already has horizontal scroll. The main gaps are:

1. **Section-level fade-up** (DSGN-03) — sections themselves (not just `.fade-in` children) do not animate on scroll
2. **Hover effects missing translateY** on all card classes
3. **Floating CTA on mobile** — currently `display: none` at `max-width: 639px` (page.css line 1631). BOOK-02 success criteria says "hides or repositions" — the `display: none` approach satisfies this. No change needed.

### Section Fade-Up Pattern (DSGN-03)

The requirement says "each section fades up into view on scroll". Two implementation options:

**Option A — Apply `.fade-in` to `<section>` elements directly**
Simple: add `fade-in` class to each `<section>` in HTML. The existing observer picks them up automatically. Upgrade CSS to include translateY.
Risk: sections are large — the observer threshold of `0.08` means the animation fires when only 8% of the section is visible. That works well for sections.

**Option B — Separate section observer with different thresholds**
More control (e.g. lower threshold for large sections), but more code.

**Recommendation:** Option A — simpler, reuses existing infrastructure, fewer JS changes. Add `fade-in` to each `<section>` tag, upgrade CSS translateY, done. The sibling-stagger logic uses `parent.querySelectorAll('.fade-in')` — sections will be their own parents' siblings, so no stagger conflict.

**However:** adding `fade-in--hidden` to sections that are above the fold (hero, trust-bar) would cause a flash. The existing JS already guards against this: it only sets `fade-in--hidden` on elements where `rect.top > window.innerHeight * 0.85`. Hero and trust-bar will be visible immediately.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Scroll animation library | AOS, ScrollReveal, GSAP ScrollTrigger | Native IntersectionObserver | Already in use; no extra deps per CLAUDE.md constraint |
| CSS transitions | JS-driven inline style animation | CSS `transition` + class toggle | Simpler, respects `prefers-reduced-motion` via base.css |
| Hover JS handlers | `mouseenter`/`mouseleave` JS | Pure CSS `:hover` pseudo-class | No JS needed; works without IntersectionObserver |
| Custom scroll-driven animation | Manual scroll event + rAF | `animation-timeline: view()` (already used as fallback) | Browser-native, no layout thrash |

---

## Common Pitfalls

### Pitfall 1: `will-change` overuse
**What goes wrong:** Adding `will-change: transform` to every card as a "performance optimisation" creates excessive GPU layers, increasing memory usage and causing composite layer explosions on low-end mobile.
**How to avoid:** Do not add `will-change`. The cards are simple elements — GPU promotion only helps for elements that animate continuously (like the floating CTA pulse ring). Apply `will-change: transform` only if profiling reveals jank.

### Pitfall 2: Hover `transform` conflicts with `.pricing-card--featured` scale
**What goes wrong:** `.pricing-card--featured` has `transform: scale(1.03)` on desktop. If the hover rule sets `transform: translateY(-4px)` alone, it overwrites the scale — the card jumps back to scale(1) on hover.
**How to avoid:** The hover rule for `.pricing-card--featured` must be `transform: scale(1.03) translateY(-4px)`. This means a separate hover selector:
```css
@media (min-width: 768px) {
  .pricing-card--featured:hover {
    transform: scale(1.03) translateY(-4px);
  }
}
```
The base hover rule `translateY(-4px)` handles mobile correctly (scale is not applied below 768px).

### Pitfall 3: Fade-in CSS scroll-driven vs. JS observer conflict
**What goes wrong:** The CSS `animation-timeline: view()` fallback (page.css lines 104–115) applies to all `.fade-in` elements via `@supports`. If JS also sets `fade-in--hidden` and `fade-in--visible`, both systems fight each other in supporting browsers.
**Current state:** This conflict already exists but the JS wins because `fade-in--visible` sets `opacity: 1` with a more specific selector than the `@supports` rule. When adding `translateY`, care must be taken that the same specificity logic applies.
**How to avoid:** Keep the `@supports` keyframe as the no-JS fallback only. The JS path (observer) is the primary path. If upgrading `fadeReveal` to include translateY, also set `animation-fill-mode: both` (already set). This is safe.

### Pitfall 4: Section fade-in causes layout shift (CLS)
**What goes wrong:** Setting `opacity: 0; transform: translateY(20px)` on `<section>` elements that are above the fold at load time causes a visible "pop" (flash of invisible content then snap-in) or a Cumulative Layout Shift if translateY moves content.
**How to avoid:** The existing JS guard (`rect.top > window.innerHeight * 0.85`) already prevents above-the-fold elements from being set to hidden. Only sections below 85% of viewport height get the hidden class. TranslateY does not cause CLS because it is a composited transform (no reflow).

### Pitfall 5: `overflow: hidden` on section clips hover lift
**What goes wrong:** If a parent section has `overflow: hidden`, the `translateY(-4px)` lift on cards is clipped at the top of the container.
**Current state:** None of the section styles in style.css or page.css use `overflow: hidden`. Not a current issue. Flag for awareness — do not add `overflow: hidden` to section backgrounds during this phase.

### Pitfall 6: Mobile touch — hover states sticky
**What goes wrong:** On touch devices, `:hover` pseudo-class can "stick" after a tap — the card stays in the lifted state.
**How to avoid:** This is acceptable browser behaviour for CSS-only hover. It does not break functionality. The requirement does not specify pointer-media behaviour. No mitigation needed per scope.

---

## Code Examples

### Card Hover — Standard Pattern
```css
/* Source: DSGN-03 requirement + existing transition token */
.pain-card {
  /* existing styles preserved */
  transition: transform var(--transition-interactive),
              box-shadow var(--transition-interactive),
              border-color var(--transition-interactive);
}
.pain-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--color-primary);
}
```

### Featured Pricing Card — Hover with Scale Preserved
```css
/* Base hover — works on mobile (no scale applied) */
.pricing-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-lg);
  border-color: var(--color-primary);
}
/* Desktop featured card — preserve scale(1.03) */
@media (min-width: 768px) {
  .pricing-card--featured:hover {
    transform: scale(1.03) translateY(-4px);
  }
}
```

### Fade-In CSS Upgrade (page.css)
```css
/* Upgrade existing rules — add translateY */
.fade-in {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1),
              transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}
.fade-in--hidden {
  opacity: 0;
  transform: translateY(20px);
}
.fade-in--visible {
  opacity: 1;
  transform: translateY(0);
}

/* CSS scroll-driven fallback upgrade */
@supports (animation-timeline: view()) {
  .fade-in {
    animation: fadeReveal linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 80%;
  }
  @keyframes fadeReveal {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0);    }
  }
}
```

### Adding fade-in to Section Elements (index.html)
```html
<!-- Add fade-in class to section tags (below-fold sections only strictly needed,
     but adding to all is safe — the JS guard handles above-fold suppression) -->
<section class="pain-points fade-in" id="pain-points">
<section class="services fade-in" id="services">
<section class="about fade-in" id="about">
<!-- etc. -->
```

### Floating CTA Mobile — Already Handled
```css
/* page.css line 1631 — already implemented, no change needed */
@media (max-width: 639px) {
  .floating-cta { display: none; }
}
```

---

## State of the Art

| Old Approach | Current Approach | Impact |
|--------------|------------------|--------|
| JS scroll event + `getBoundingClientRect` polling | `IntersectionObserver` | No layout thrash, passive by design |
| JS-driven `style.opacity = 0` | CSS class toggle + CSS transition | Respects `prefers-reduced-motion` automatically |
| `animation-timeline: view()` (Chrome 115+ only) | Used as progressive enhancement fallback | Already implemented in project |

---

## Open Questions

1. **Should section-level fade-in include the hero and trust-bar?**
   - What we know: JS guard prevents above-fold sections from getting `fade-in--hidden`
   - What's unclear: Whether adding `fade-in` to hero/trust-bar adds noise for no visual gain (they're always visible on load)
   - Recommendation: Skip hero and trust-bar — add `fade-in` only to sections below them. Less risk, same result.

2. **ROI card — is `roi__card` a card (needs hover) or a container (does not need hover)?**
   - What we know: It's a white rounded container (`border-radius: var(--radius-xl)`) with inputs and outputs inside
   - What's unclear: The requirement says "every card" — the ROI card is interactive, not a pure content card
   - Recommendation: Apply the hover lift to `roi__card` — it has a visible border and the lift effect reinforces interactivity.

---

## Environment Availability

Step 2.6: SKIPPED — Phase 6 is pure CSS/JS edits to existing static files. No external tools, services, runtimes, CLIs, or databases required beyond a text editor and browser.

---

## Validation Architecture

nyquist_validation is enabled in config.json.

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Manual browser testing (no automated test framework — static HTML site) |
| Config file | None |
| Quick run command | Open index.html in browser, resize to 375px, scroll page |
| Full suite command | Same — manual visual verification at 375px, 768px, and 1440px |

### Phase Requirements → Test Map

| Req ID | Behaviour | Test Type | Automated Command | File Exists? |
|--------|-----------|-----------|-------------------|-------------|
| DSGN-03 | Hovering any card produces translateY(-4px) + shadow + green border | manual | Open index.html → hover each card type | N/A |
| DSGN-03 | Each section fades up on scroll via IntersectionObserver | manual | Open index.html → scroll from top, observe animations | N/A |
| DSGN-04 | All grids single column at 375px | manual | DevTools → 375px viewport → scroll full page | N/A |
| DSGN-04 | Process steps stack vertically, arrows hidden at 375px | manual | DevTools → 375px → inspect how-it-works section | N/A |
| DSGN-04 | Comparison table scrolls horizontally at 375px | manual | DevTools → 375px → swipe comparison table | N/A |
| DSGN-04 | Floating CTA hidden on mobile | manual | DevTools → 375px → wait 3s → confirm no floating button | N/A |

### Sampling Rate
- **Per task commit:** Visual spot-check in browser at 375px and 1440px
- **Per wave merge:** Full scroll-through at 375px, 768px, and 1440px — check all hover effects and animations
- **Phase gate:** Human verify checkpoint passes before marking phase complete

### Wave 0 Gaps
None — no automated test infrastructure needed. This is a visual/interaction phase; all testing is manual browser verification.

---

## Plan Recommendation

Phase 6 naturally splits into **two plans**:

**06-01-PLAN: CSS Polish** — hover effects on all card classes + fade-in translateY upgrade in page.css
- Edit `page.css`: upgrade `.fade-in` CSS (add translateY), update `fadeReveal` keyframes
- Edit `page.css`: add hover rules for all 8 card classes (pain-card, service-card upgrade, channel-card upgrade, how__step, proven__card, pricing-card, pricing-card--featured, roi__card, about__diff-item)
- No HTML changes

**06-02-PLAN: Section Animations + Human Verify** — add `fade-in` class to section elements in HTML
- Edit `index.html`: add `class="... fade-in"` to below-fold sections (pain-points, services, about, channels, how, industries, proven, comparison, pricing, faq, roi, booking, cta-banner)
- Human verify checkpoint: full browser check at 375px, 768px, 1440px

This split ensures CSS regressions are isolated from HTML changes.

---

## Sources

### Primary (HIGH confidence)
- Direct code audit of index.html, style.css, base.css, page.css — all requirements verified against actual implementation
- MDN IntersectionObserver API — native browser API, no library dependency
- CSS `animation-timeline: view()` — already implemented in project (progressive enhancement)

### Secondary (MEDIUM confidence)
- `prefers-reduced-motion` suppression via base.css — verified in codebase, standard CSS technique
- `transform: scale() translateY()` stacking — standard CSS transform list behaviour

### Tertiary
- None

---

## Metadata

**Confidence breakdown:**
- Card hover inventory: HIGH — complete code audit of all card classes
- CSS translateY upgrade: HIGH — existing system well understood, upgrade is additive
- Mobile grid gaps: HIGH — all grids audited at 375px breakpoint, most already correct
- Featured pricing card pitfall: HIGH — code audit confirmed `transform: scale(1.03)` exists

**Research date:** 2026-04-28
**Valid until:** 2026-05-28 (stable — no external dependencies)
