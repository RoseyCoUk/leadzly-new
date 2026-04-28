# Phase 5: Booking Experience & CTAs — Research

**Researched:** 2026-04-27
**Domain:** Calendly inline embed, vanilla JS animation patterns (IntersectionObserver, requestAnimationFrame), floating fixed UI components, CTA copy strategy
**Confidence:** HIGH — all findings are derived from direct codebase inspection, the existing UI spec, and well-established vanilla JS APIs. No external library research required.

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| BOOK-01 | Inline Calendly embed section — two-column layout, left sells the call, right is live widget | UI spec fully specifies HTML structure, grid, and Calendly script loading. Exact markup confirmed against existing `.booking` scaffold in index.html. |
| BOOK-02 | Floating sticky CTA — bottom-right, pulsing green dot, animates in after 3 s, hides on mobile | UI spec provides complete CSS and JS. Fits established IIFE pattern from phases 3–4. |
| BOOK-03 | At least 4 distinct CTA copy variants across page | Current hero/pricing CTA copy audited in index.html — exact edit targets identified. |
| QWIN-03 | Hero stat numbers count up from zero on scroll-into-view (IntersectionObserver + rAF) | Pattern matches existing scroll-reveal IIFE already in index.html. UI spec provides complete JS IIFE. |
</phase_requirements>

---

## Summary

Phase 5 has four distinct deliverables: (1) replacing the booking section scaffold with a real Calendly inline embed, (2) adding a floating sticky CTA button, (3) updating CTA button copy across the page, and (4) animating hero stat numbers on scroll.

All deliverables are self-contained and low-risk. The codebase is a single `index.html` with `style.css`, `base.css`, and `page.css`. Every deliverable maps to a narrow, well-understood edit target. The UI spec (05-UI-SPEC.md) provides complete HTML, CSS, and JS for every component — no design decisions remain open for the executor.

The Calendly embed is the only external dependency. Calendly provides both a CSS file and a JS widget file from their own CDN. Both are loaded via standard `<link>` and `<script>` tags — no API key, no auth, no npm. The inline embed uses a plain `<div>` with `data-url` — Calendly's widget.js initialises it automatically on load.

**Primary recommendation:** Execute this phase as four sequential plans — one plan per requirement. Each plan is independently testable and carries no risk of breaking adjacent sections.

---

## Standard Stack

### Core

| Library / API | Version | Purpose | Why Standard |
|---------------|---------|---------|--------------|
| Calendly widget.js | CDN (auto-latest) | Inline calendar embed | First-party Calendly CDN script, no alternative |
| Calendly widget.css | CDN (auto-latest) | Calendly iframe styles | Required companion stylesheet |
| IntersectionObserver | Native browser API | Trigger counter animation on scroll | Already used in existing scroll-reveal IIFE in index.html |
| requestAnimationFrame | Native browser API | Drive 60fps counter increment | Standard animation loop API — no library required |

### No New npm / Build Tooling

The CLAUDE.md constraint is absolute: vanilla HTML/CSS/JS only, no npm, no frameworks. All deliverables comply fully.

**Calendly script loading (one-time addition to `<head>` or before `</body>`):**
```html
<link href="https://assets.calendly.com/assets/external/widget.css" rel="stylesheet">
<script src="https://assets.calendly.com/assets/external/widget.js" type="text/javascript" async></script>
```

---

## Architecture Patterns

### Established Codebase Conventions (must be followed)

| Convention | Detail |
|------------|--------|
| JS isolation | Every JS feature is an IIFE in a separate `<script>` block before `</body>` |
| CSS location | All new CSS goes in `page.css` only — never `style.css` or `base.css` |
| CSS naming | BEM-lite: `.block__element` and `.block--modifier` |
| No inline styles | Only allowed for Calendly embed `min-width`/`height` (Calendly requirement) |
| Section padding | `padding: clamp(var(--space-12), 8vw, var(--space-24)) 0` — must match all other sections |
| Design tokens | Use CSS custom properties from `style.css :root` — no new hex values |

### Pattern 1: IIFE Script Block (established — must replicate)

**What:** Each JS feature is a self-contained IIFE appended as a new `<script>` block before `</body>`.
**When to use:** All new JS in this phase — floating CTA timer, stat counter.
**Example (from existing codebase, phase 4 FAQ):**
```js
// Already in index.html
(function() {
  var items = document.querySelectorAll('.faq__item');
  items.forEach(function(item) {
    var btn = item.querySelector('.faq__question');
    // ...
  });
})();
```

### Pattern 2: IntersectionObserver + one-shot animated flag (established — QWIN-03 reuses)

**What:** Observer fires once, sets `animated = true`, disconnects, runs animation loop with `requestAnimationFrame`.
**When to use:** Any "animate once on first scroll into view" interaction.

The existing scroll-reveal IIFE already demonstrates this pattern. QWIN-03 adds a new observer that watches `.hero__card` and on first intersection runs the counter animation defined in the UI spec.

### Pattern 3: CSS transition via class toggle (established — floating CTA)

**What:** Element starts with `opacity: 0; transform: translateY(16px); pointer-events: none`. JS adds a `--visible` modifier class after a `setTimeout`, which transitions to `opacity: 1; transform: translateY(0); pointer-events: auto`.
**When to use:** Delayed entrance animations — floating CTA (3 s delay).

This is identical to the cookie banner pattern already in index.html (see `.cookie-banner--visible`).

### Pattern 4: CSS `@keyframes` pulse ring (new in this phase)

**What:** A `::before` pseudo-element on the pulse dot scales from `1` to `1.5` while fading from `opacity 0.6` to `0` in a 2 s infinite loop.
**When to use:** The floating CTA dot only — signals live availability.

### Recommended Edit Sequence (per plan)

```
Plan 1 — BOOK-01  Booking section: HTML + CSS
Plan 2 — BOOK-02  Floating sticky CTA: HTML + CSS + JS IIFE
Plan 3 — BOOK-03  CTA copy edits: hero + 3 pricing buttons
Plan 4 — QWIN-03  Hero stat counter: data attributes + JS IIFE
         + human verify checkpoint (all Phase 5 changes)
```

### Anti-Patterns to Avoid

- **Editing style.css or base.css:** All new CSS goes in `page.css` only. These files are design system tokens and global resets.
- **Adding Calendly script twice:** Check `index.html` before adding — only one instance of the widget script and CSS link should exist.
- **Using `auto` as `max-height` transition target:** CSS `transition` cannot animate to `auto`. Use a fixed pixel ceiling (established pattern from FAQ accordion).
- **Calendly `height` too short:** Calendly's inline widget needs at least 630px to render the month view without a scrollbar. The spec mandates `700px` desktop / `650px` mobile.
- **Floating CTA z-index conflict with cookie banner:** Cookie banner is `z-index: 9999`. Floating CTA must also be `z-index: 9999` — they do not overlap (banner is fixed bottom full-width, CTA is fixed bottom-right with defined width). No conflict.
- **Forgetting `pointer-events: none` on hidden state:** Without this, the invisible floating CTA would intercept clicks in the bottom-right corner for the first 3 seconds.
- **Stat counter replay on re-scroll:** Use `animated = true` flag + `observer.disconnect()` after first intersection so the count only plays once.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Calendar booking UI | Custom date picker / form | Calendly inline embed | Calendly handles timezone, availability, confirmation emails, CRM sync — weeks of work |
| Easing functions | Custom math | CSS `cubic-bezier` or JS `1 - Math.pow(1 - progress, 3)` | Proven ease-out curve, single line |
| Scroll position detection | `scroll` event listener + debounce | IntersectionObserver | Already used in codebase; no scroll event jank, automatic cleanup via `disconnect()` |

**Key insight:** In this phase, every "hard problem" is already solved — Calendly handles booking, the browser handles intersection detection and animation frames, and the UI spec provides every line of code.

---

## Existing Edit Targets (confirmed via codebase audit)

### BOOK-01 — Calendly Embed

**Target:** `index.html` line 826
```html
<!-- Current: -->
<p class="scaffold-placeholder">Booking calendar coming in Phase 5.</p>
```
Replace with the two-column `.booking__grid` structure from the UI spec.

**Calendly script:** Not yet present in `index.html` — must be added. Check both `<head>` and end-of-`<body>` before inserting.

**Booking section background:** Already `background: var(--section-green)` set in `page.css` line 1300–1302. Do not change.

### BOOK-03 — CTA Copy Edits (exact locations)

| Location | Line | Current Copy | New Copy |
|----------|------|--------------|----------|
| Hero primary CTA | line 155 | "Book a Strategy Call" | "Book a Free Strategy Call" |
| Hero secondary CTA | line 156 | "How It Works" | "See If We're a Fit" (also update `href` from `#services` to `#booking` or keep as-is — see Open Questions) |
| Pricing Starter CTA | line 654 | "Get Started" | "Start Filling Your Calendar" |
| Pricing Growth CTA | line 669 | "Get Started" | "Start Filling Your Calendar" |
| Pricing Enterprise CTA | line 683 | "Book a Call" | "Start Filling Your Calendar" |
| ROI section | none yet | no CTA button exists | add "Get Your Meetings Forecast" CTA linking to #booking |

Note: UI spec says the hero secondary CTA should be "See If We're a Fit" (`.btn--outline btn--lg`). Currently the hero has one primary CTA ("Book a Strategy Call") and one outline CTA ("How It Works"). The outline CTA copy changes to "See If We're a Fit". The `href` could reasonably stay as `#services` or change to `#booking` — see Open Questions.

### QWIN-03 — Hero Stat Spans (data attribute additions)

| Element | Line | Current | Add |
|---------|------|---------|-----|
| `.hero__card-stat-num` #1 | line 193 | `>12<` | `data-count-to="12"` |
| `.hero__card-stat-num` #2 | line 197 | `>94%<` | `data-count-to="94" data-count-suffix="%"` |
| `.hero__card-stat-num` #3 | line 201 | `>8<` | `data-count-to="8"` |

---

## Common Pitfalls

### Pitfall 1: Calendly widget not initialising

**What goes wrong:** The Calendly inline widget `<div>` renders but stays blank.
**Why it happens:** The `widget.js` script loaded after the DOM div is created — this is fine with `async`. However if the script is placed before the div in DOM order AND async isn't set, the script fires before the div exists.
**How to avoid:** Load `widget.js` with the `async` attribute. Place the `<div class="calendly-inline-widget">` inside the booking section (which is rendered before `</body>`). This is already the pattern the UI spec prescribes.
**Warning signs:** Empty white box where the calendar should be; no console errors.

### Pitfall 2: Calendly height too short causes scroll inside iframe

**What goes wrong:** User sees a scrollbar inside the Calendly iframe, must scroll inside the embed to access the calendar.
**Why it happens:** Calendly's month-view UI requires approximately 650–700px. At shorter heights, Calendly adds internal overflow.
**How to avoid:** Use `height: 700px` desktop, `height: 650px` mobile (< 768px). Do not reduce below 630px.
**Warning signs:** Calendar shows but requires internal scrolling on desktop.

### Pitfall 3: Floating CTA obscures cookie banner on first visit

**What goes wrong:** Cookie banner slides up from bottom. Floating CTA appears 3 s later overlapping bottom-right of banner.
**Why it happens:** Cookie banner is full-width bottom; floating CTA is bottom-right. They occupy the same screen region on first visit.
**How to avoid:** The floating CTA has a fixed `bottom: 24px; right: 24px` position — it overlaps the right side of the cookie banner. On mobile, the floating CTA is `display: none` so no conflict. On desktop, the cookie banner is `flex-wrap: wrap` and the CTA button sits at the right — the floating CTA appears above the banner (both are `z-index: 9999`). No action required — review visually during human verify checkpoint.
**Warning signs:** Cookie banner accept button becomes unclickable due to floating CTA overlap.

### Pitfall 4: Counter animation replays on back navigation

**What goes wrong:** User navigates away and back; stats count up again each time.
**Why it happens:** `animated` flag and observer are re-created on each page load in a standard page (not SPA). This is actually fine — it is a page reload, not a SPA navigation. The spec says "first scroll" which means first scroll per page load.
**How to avoid:** No action required. The `animated = true` flag prevents replay within a single page session.
**Warning signs:** Only a concern if the page is later converted to a SPA.

### Pitfall 5: Hero secondary CTA href conflicts with section order

**What goes wrong:** "See If We're a Fit" CTA scrolls user to an unexpected location.
**Why it happens:** The current href is `#services`. If changed to `#booking`, it scrolls past all content — may not be ideal for a new visitor.
**How to avoid:** Keep `href="#booking"` — the intent of "See If We're a Fit" is to send users to the booking section to self-qualify. This is the conversion-first choice. Confirm in plan.

---

## Code Examples

### Calendly Inline Embed (from UI spec / Calendly docs)

```html
<!-- In <head> or before </body> — one instance only -->
<link href="https://assets.calendly.com/assets/external/widget.css" rel="stylesheet">
<script src="https://assets.calendly.com/assets/external/widget.js" type="text/javascript" async></script>

<!-- In booking section -->
<div class="calendly-inline-widget"
     data-url="https://calendly.com/allan-chan-leadzly"
     style="min-width:320px;height:700px;"></div>
```

### Floating CTA — CSS state machine

```css
/* Hidden state */
.floating-cta {
  position: fixed;
  bottom: 24px;
  right: 24px;
  z-index: 9999;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.4s ease, transform 0.4s ease;
  pointer-events: none;
}
/* Visible state (JS adds this class after 3s) */
.floating-cta--visible {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
/* Hide on mobile */
@media (max-width: 639px) {
  .floating-cta { display: none; }
}
```

### Floating CTA — JS IIFE

```js
(function() {
  setTimeout(function() {
    var btn = document.getElementById('floating-cta');
    if (btn) btn.classList.add('floating-cta--visible');
  }, 3000);
})();
```

### Stat Counter — JS IIFE

```js
(function() {
  var card = document.querySelector('.hero__card');
  if (!card || !('IntersectionObserver' in window)) return;
  var animated = false;
  var observer = new IntersectionObserver(function(entries) {
    if (animated || !entries[0].isIntersecting) return;
    animated = true;
    observer.disconnect();
    var nums = card.querySelectorAll('[data-count-to]');
    nums.forEach(function(el) {
      var target = parseInt(el.dataset.countTo, 10);
      var suffix = el.dataset.countSuffix || '';
      var duration = 1200;
      var start = null;
      function step(ts) {
        if (!start) start = ts;
        var progress = Math.min((ts - start) / duration, 1);
        var eased = 1 - Math.pow(1 - progress, 3);
        el.textContent = Math.round(eased * target) + suffix;
        if (progress < 1) requestAnimationFrame(step);
      }
      requestAnimationFrame(step);
    });
  }, { threshold: 0.5 });
  observer.observe(card);
})();
```

---

## Runtime State Inventory

Not applicable — this is a greenfield feature phase (new HTML/CSS/JS additions). No existing data stores, services, or registered state reference the concepts being added.

---

## Environment Availability

Step 2.6: SKIPPED — this phase is purely HTML/CSS/JS additions. No external CLI tools, runtimes, databases, or services beyond a standard browser and text editor are required. Calendly CDN is accessed at runtime by the user's browser, not at build time.

---

## Validation Architecture

`workflow.nyquist_validation` is `true` in `.planning/config.json` — section included.

### Test Framework

| Property | Value |
|----------|-------|
| Framework | None — static HTML/CSS/JS site with no automated test suite |
| Config file | Not applicable |
| Quick run command | Open `index.html` in browser + manual checklist |
| Full suite command | Manual cross-browser check (Chrome, Firefox, Safari mobile) |

No automated test infrastructure exists and none is appropriate for a static page with external CDN dependencies (Calendly embed). All validation is manual browser testing.

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| BOOK-01 | Two-column booking layout; Calendly widget loads and is bookable | Manual browser | Open index.html, scroll to booking, confirm calendar renders | N/A |
| BOOK-02 | Floating CTA invisible for first 3 s, then slides in; pulsing dot visible; hidden on mobile | Manual browser | Load page, watch bottom-right, resize to 375px | N/A |
| BOOK-03 | At least 4 distinct CTA copy variants present across page | Manual browser / grep | `grep -o "Book a Free Strategy Call\|Get Your Meetings Forecast\|See If We're a Fit\|Start Filling Your Calendar" index.html` | N/A |
| QWIN-03 | Hero stats count up from 0 when scrolled into view; no replay on re-scroll | Manual browser | Load page, scroll to hero card, observe animation | N/A |

### Sampling Rate

- **Per task commit:** Open `index.html` in Chrome — confirm the specific deliverable renders correctly.
- **Per wave merge:** Full manual pass — scroll entire page, test floating CTA timing, test stat counter, confirm Calendly renders.
- **Phase gate:** Full manual pass green before `/gsd:verify-work` — including mobile viewport check (375px) for floating CTA hide rule.

### Wave 0 Gaps

None — no automated test framework gaps exist because this project intentionally has no test framework. All testing is manual browser verification documented in the human verify checkpoint (plan 4).

---

## Project Constraints (from CLAUDE.md)

| Constraint | Implication for Phase 5 |
|------------|------------------------|
| HTML/CSS/vanilla JS only — no build system, no npm, no frameworks | All deliverables must be plain HTML tags, CSS in page.css, and script IIFEs in index.html |
| Calendly inline embed uses Calendly widget script from CDN | Confirmed approach — load from `https://assets.calendly.com/assets/external/widget.js` |
| No new font imports | Inter and Plus Jakarta Sans already loaded — no changes |
| No backend | Calendly handles all booking server-side — no server code needed |
| All client-side only | Stat counter animation uses rAF + IntersectionObserver — no external library |
| Logo assets: use existing SVGs | Not relevant to this phase (no logos added) |

---

## Open Questions

1. **Hero secondary CTA href: `#services` vs `#booking`**
   - What we know: Current href is `#services`. UI spec copy is "See If We're a Fit".
   - What's unclear: Does "See If We're a Fit" imply self-qualification by reading services, or booking a call?
   - Recommendation: Change to `href="#booking"` — the copy implies a conversion action (fit assessment via a call), not an informational scroll. The booking section is where visitors take that action.

2. **ROI section CTA — where in the HTML**
   - What we know: UI spec calls for a "Get Your Meetings Forecast" CTA in the ROI section. The existing ROI section HTML (lines 775–815) has no CTA button — it ends with a disclaimer paragraph.
   - What's unclear: The exact position and styling for this CTA is not specified beyond "`.btn--primary`".
   - Recommendation: Add a `.btn--primary` anchor linking to `#booking` after the `.roi__disclaimer` paragraph. Full-width on mobile, auto width centred on desktop — consistent with other section CTAs. The plan should specify this precisely.

3. **Calendly script placement — `<head>` vs before `</body>`**
   - What we know: The UI spec says "in `<head>` or before `</body>`". The `async` attribute means placement does not affect load blocking.
   - What's unclear: No preference stated.
   - Recommendation: Place in `<head>` alongside the existing stylesheets — keeps all external resource hints together, consistent with the Google Fonts preconnect links already there. The `async` attribute prevents render blocking.

---

## Sources

### Primary (HIGH confidence)
- Direct codebase inspection — `index.html`, `page.css`, `style.css`, `base.css`, `.planning/phases/05-booking-experience-ctas/05-UI-SPEC.md`
- Calendly official embed documentation pattern (standard `data-url` div + widget.js) — well-established, unchanged for years
- MDN Web APIs: IntersectionObserver, requestAnimationFrame — native browser APIs, no versioning concern

### Secondary (MEDIUM confidence)
- Calendly CDN URLs — verified against known Calendly embed documentation format; `async` attribute usage is standard practice

### Tertiary (LOW confidence)
- None — all findings are directly from codebase or well-established browser APIs

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — Calendly CDN is the only external dependency; everything else is native browser APIs already used in the codebase
- Architecture: HIGH — all patterns are already established in index.html; this phase replicates them, not invents them
- Pitfalls: HIGH — all identified via direct codebase inspection (cookie banner z-index, Calendly height, IIFE pattern)

**Research date:** 2026-04-27
**Valid until:** 2026-06-01 (Calendly CDN URLs are stable; CSS token / JS pattern findings are timeless within this codebase)
