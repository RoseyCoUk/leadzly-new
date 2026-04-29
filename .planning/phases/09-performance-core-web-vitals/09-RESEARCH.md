# Phase 9: Performance & Core Web Vitals — Research

**Researched:** 2026-04-28
**Domain:** Static HTML/CSS performance optimisation — Core Web Vitals, Google Fonts API, CLS prevention, CSS minification
**Confidence:** HIGH (all findings verified against live codebase and official sources)

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| PERF-01 | Google Fonts request trimmed to weights 400, 600, 700 only | Font API v2 syntax verified; current URL audited; weight 500 and 800 found in use — see Weight Audit below |
| PERF-02 | Below-fold decorative SVG mockups lazy-loaded — not blocking initial HTML parse | SVGs are inline `<svg>` elements; `loading="lazy"` does not apply; must convert to external `.svg` file + `<img loading="lazy">` |
| PERF-03 | Calendly embed container given explicit `min-height` — no layout shift on load | `.booking__calendar-wrap` has no `min-height`; widget height is only on child `div` inline style |
| PERF-04 | Floating CTA inject causes zero CLS | Floating CTA was **removed** in commit `909aaf2` — no floating CTA exists in current HTML; see PERF-04 analysis below |
| PERF-05 | `page.css` minified — served as minified file, version-bumped | 1693-line unminified file; manual minification via online tool; version bump already understood (query string pattern in use) |
</phase_requirements>

---

## Summary

Phase 9 is a pure optimisation pass on a static HTML/CSS/vanilla-JS site with no build pipeline. The five requirements address font bandwidth, render-blocking resource deferral, layout stability from third-party widgets and injected UI, and serving a compressed CSS file.

The most nuanced finding is **PERF-02**: the pipeline board and outreach sequence mockups are inline `<svg>` elements, not `<img>` elements. The HTML `loading="lazy"` attribute only works on `<img>`, `<iframe>`, and `<video>` (Chrome also supports `<iframe loading="lazy">`). Inline SVGs are part of the DOM at parse time and cannot be lazily loaded by the browser natively. The correct approach is to save each SVG mockup as a standalone `.svg` file and reference it with `<img src="..." loading="lazy" width="..." height="...">`.

**PERF-04 is a special case**: the floating sticky CTA that existed during Phase 5 was removed in a later commit (`909aaf2 remove floating sticky CTA (BOOK-02)`). The current `index.html` has no floating CTA element. The requirement is effectively satisfied by absence. The plan should note this, verify it explicitly, and mark the task as a confirm-and-close rather than an implementation task.

**Primary recommendation:** Implement PERF-01 (font URL trim), PERF-02 (SVG externalise + lazy-load), PERF-03 (Calendly wrapper min-height), PERF-04 (confirm no floating CTA exists), and PERF-05 (manual CSS minification + version bump) in a single focused pass across index.html, page.css, and two new .svg files.

---

## Codebase Audit (Pre-Implementation Facts)

These are facts extracted directly from the live files, not hypotheses.

### Current Google Fonts URL (index.html line 48)

```html
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

**Weights loaded:**
- Plus Jakarta Sans: 400, 500, 600, 700, 800
- Inter: 400, 500, 600, 700

### Font Weight Audit — Actual CSS Usage

Searched `page.css` for all `font-weight` values:

| Weight | Occurrences in page.css | Occurrences in style.css/base.css |
|--------|------------------------|-----------------------------------|
| 400 | Implicit (body default) | — |
| 500 | 5 occurrences | 0 |
| 600 | Multiple | 0 |
| 700 | Multiple | 0 |
| 800 | 7 occurrences | 0 |

**Implication for PERF-01:** The requirement says "weights 400, 600, and 700 only". If this is applied literally to both Inter and Plus Jakarta Sans, seven uses of `font-weight: 800` and five uses of `font-weight: 500` in page.css must be replaced with valid alternatives (typically 700 for 800 → nearest loaded weight, and 600 for 500 → nearest loaded weight). The planner must include a CSS weight replacement sub-task.

Alternatively, the requirement may intend "Inter weights only" — Inter is currently loaded at 400, 500, 600, 700. Trimming Inter to 400, 600, 700 removes weight 500 from the Inter download. Plus Jakarta Sans (display font) uses 800 heavily and should be retained.

**Recommended interpretation:** Trim Inter to 400;600;700 (removes one Inter variant). Trim Plus Jakarta Sans to 400;600;700;800 (removes weight 500 from PJS, which has no confirmed uses). This satisfies the spirit of PERF-01 (remove unused weight variants) without breaking the visual design.

**Cross-check needed in plan:** Grep page.css for `font-family.*Inter` + `font-weight.*500` to confirm weight-500 Inter is truly unused vs just inherited.

### Inline SVG Mockups — Exact Locations

| Element | CSS class | HTML lines | Approx viewBox dimensions |
|---------|-----------|-----------|--------------------------|
| Outreach Sequence mockup | `.channels__visual` wrapper | ~551–669 | 480×520 |
| Pipeline Board mockup | `.how__visual` wrapper | ~709–878 | 640×500 |

Both are **desktop-only** (hidden via `display:none` on mobile via CSS media queries). Neither carries `loading="lazy"`.

**Both SVGs are large** — the Pipeline Board SVG spans ~170 lines of markup. Externalising them will meaningfully reduce DOM parse time and initial HTML payload.

### Calendly Embed — Current State (index.html lines 1307–1309)

```html
<div class="booking__calendar-wrap">
  <div class="calendly-inline-widget"
       data-url="https://calendly.com/allan-chan-elevateoco/30min"
       style="min-width:320px;height:700px;"></div>
</div>
```

CSS for `.booking__calendar-wrap` (page.css line 1676):

```css
.booking__calendar-wrap {
  border-radius: var(--radius-xl);
  overflow: hidden;
}
```

**No `min-height` on `.booking__calendar-wrap`**. The height is only on the child `.calendly-inline-widget` via inline style. Before the Calendly script loads and sets the widget height, the container has zero intrinsic height, causing layout shift.

Mobile override (page.css line 1684):
```css
.booking__calendar-wrap .calendly-inline-widget {
  height: 650px !important;
}
```

### Floating CTA — Current State

**Absent from HTML and CSS.** The floating sticky CTA was removed in commit `909aaf2 remove floating sticky CTA (BOOK-02)`. No element with a floating/fixed CTA pattern exists in `index.html`, `page.css`, `style.css`, or `base.css`.

PERF-04 is satisfied by absence. The plan task should confirm this via grep and mark it closed.

### page.css Size

- **Lines:** 1,693
- **Minified target:** Estimated 40–60% size reduction (comments, whitespace, blank lines)

---

## Standard Stack

This project uses no build tools. All operations are manual.

### Core
| Technique | Version | Purpose | Why Standard |
|-----------|---------|---------|--------------|
| Google Fonts CSS2 API | current | Variable-weight font loading | Only way to load subset of weights from Google Fonts CDN |
| `<img loading="lazy">` | HTML spec (all modern browsers) | Defer off-screen image/SVG loading | Native browser API, zero JS required |
| CSS `min-height` | CSS spec | Pre-reserve space for dynamic content | Prevents layout shift before third-party widget loads |
| `position: fixed` (CLS context) | CSS spec | Fixed elements excluded from CLS reflow scoring | Layout Instability API spec confirmed: fixed elements don't shift sibling content |

### Supporting
| Tool | Purpose | When to Use |
|------|---------|-------------|
| cleancss.com | Manual CSS minification | Paste page.css, get minified output, save as page.min.css |
| cssnano (online) | Alternative minifier | If cleancss.com output has issues |
| Chrome DevTools > Network tab | Verify font weights fetched | Confirm only 3 weight files download after URL change |
| Lighthouse (Chrome DevTools) | Score verification | Run on deployed version to confirm 90+ performance score |

### Installation
```bash
# No npm install needed — pure HTML/CSS/JS static site
# No build tools, no package manager
```

---

## Architecture Patterns

### Pattern 1: Google Fonts CSS2 Weight Specification

**What:** The `wght@` parameter accepts semicolon-separated weight values in ascending order.

**Correct syntax for trimmed URL:**
```html
<!-- Before (8 weight variants across 2 families) -->
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

<!-- After (6 weight variants — removes Inter 500 and PJS 500) -->
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
```

**Note:** Weight values MUST be in ascending order. The `display=swap` parameter should be retained — it tells the browser to use a fallback font while the web font loads, preventing invisible text (good for LCP).

### Pattern 2: Externalise Inline SVG + Lazy Load

**What:** Save the inline SVG markup as a standalone `.svg` file, then reference it with `<img>`.

**Step 1 — Create external SVG files:**
```
pipeline-board.svg       (contents: the <svg viewBox="0 0 640 500"...> element)
outreach-sequence.svg    (contents: the <svg viewBox="0 0 480 520"...> element)
```

**Step 2 — Replace inline SVG with img tag:**
```html
<!-- Before -->
<div class="how__visual">
  <svg viewBox="0 0 640 500" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" role="presentation">
    <!-- ~170 lines of SVG markup -->
  </svg>
</div>

<!-- After -->
<div class="how__visual">
  <img src="./pipeline-board.svg"
       alt=""
       aria-hidden="true"
       loading="lazy"
       width="640"
       height="500">
</div>
```

**Why `alt=""`?** These are decorative/presentational graphics (`aria-hidden="true"` already on the original SVG). Empty alt suppresses screen reader announcement. Width/height attributes prevent CLS from the image itself (browser reserves space before image loads).

**CSS impact:** The existing `.how__visual svg` and `.channels__visual svg` rules use `width: 100%; height: auto`. After conversion, rename or add `.how__visual img` and `.channels__visual img` rules with the same styles. The `filter: drop-shadow` and `border-radius` rules still apply.

### Pattern 3: Calendly CLS Prevention via min-height

**What:** Add `min-height` to the wrapper element that matches the widget's loaded height.

```css
/* In page.css — add to .booking__calendar-wrap block */
.booking__calendar-wrap {
  border-radius: var(--radius-xl);
  overflow: hidden;
  min-height: 700px; /* matches height:700px on .calendly-inline-widget */
}

@media (max-width: 767px) {
  .booking__calendar-wrap {
    min-height: 650px; /* matches height:650px mobile override */
  }
}
```

**Why this prevents CLS:** The browser renders `.booking__calendar-wrap` with 700px height before the Calendly iframe loads. No layout shift occurs because no height transition happens — the space is pre-allocated.

### Pattern 4: CSS Minification (Manual — No Build Tool)

**Process:**
1. Open cleancss.com
2. Paste entire contents of `page.css`
3. Download/copy minified output
4. Save as `page.css` (overwrite in place — do not rename to `page.min.css` as index.html already references `./page.css`)
5. Increment version query string in `index.html`:

```html
<!-- Before -->
<link rel="stylesheet" href="./page.css?v=13">

<!-- After -->
<link rel="stylesheet" href="./page.css?v=14">
```

**Why overwrite in place:** `index.html` references `./page.css` — not `./page.min.css`. Renaming would require an index.html change. The version query string (`?v=14`) is the cache-busting mechanism. Keeping the same filename is simpler.

**What minification removes:** Comments, whitespace, blank lines, redundant semicolons. It does NOT alter selectors or property values. The visual output is identical.

### Anti-Patterns to Avoid

- **Do not add `loading="lazy"` to inline `<svg>` elements.** The attribute is ignored by browsers on inline SVG. The element is already in the DOM at parse time.
- **Do not remove `display=swap` from the Google Fonts URL.** This prevents FOIT (Flash of Invisible Text) which hurts LCP.
- **Do not use `height` instead of `min-height` for the Calendly wrapper.** The Calendly script may inject a taller iframe for some calendar states. `min-height` is safer.
- **Do not rename page.css to page.min.css.** index.html links to `./page.css` — overwrite in place.
- **Do not add Calendly min-height only in CSS without the mobile media query.** The mobile height is 650px, not 700px.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Font weight subsetting | Custom font-face loading JS | Google Fonts `wght@400;600;700` URL parameter | CDN handles subsetting automatically per weight |
| SVG lazy loading | IntersectionObserver + JS SVG injection | `<img src="x.svg" loading="lazy">` | Native browser API, zero JS, works with CSS rules |
| CSS minification | Manual whitespace/comment removal by hand | Online minifier (cleancss.com) | Minifier handles edge cases; hand editing 1,693 lines risks syntax breakage |
| Cache busting | Filename hash | Query string version bump (`?v=14`) | Consistent with existing pattern established in Phases 1–8 |

---

## PERF-04 Analysis: Floating CTA

The floating sticky CTA was implemented in Phase 5 (05-02-PLAN) and subsequently removed in a quick commit (`909aaf2 remove floating sticky CTA (BOOK-02)`) prior to Phase 7.

**Current state:** No floating CTA exists anywhere in `index.html`, `page.css`, `style.css`, or `base.css`.

**PERF-04 requirement:** "Floating CTA inject causes zero CLS (container pre-reserved or transition adjusted)"

**Recommended plan approach:** One task — confirm the element is absent via grep, document that PERF-04 is satisfied by absence, and mark the requirement met. No implementation needed.

**If the floating CTA is to be re-added in future:** The correct CLS-safe pattern is:
- Use `position: fixed` (removes element from normal flow — other content does not reflow)
- Reveal via CSS `opacity` + `transform` transition only (not `display:none → block`, which causes reflow)
- Do not use JS-injected `position: static/relative` elements
- The Layout Instability API specification confirms: "scrolling with a `position: fixed` element doesn't produce a layout shift"

---

## Common Pitfalls

### Pitfall 1: SVG loading="lazy" on Inline SVG Does Nothing
**What goes wrong:** Developer adds `loading="lazy"` to an inline `<svg>` tag expecting deferred loading.
**Why it happens:** `loading="lazy"` is an HTML attribute only valid on `<img>`, `<iframe>`, and `<input type="image">`. Inline SVGs are DOM elements, already parsed when the HTML is processed.
**How to avoid:** Convert to `<img src="file.svg" loading="lazy">`. The external file is fetched lazily by the browser.
**Warning signs:** Lighthouse still reports the SVG in the critical request chain after adding the attribute.

### Pitfall 2: Google Fonts Weight Order Must Be Ascending
**What goes wrong:** `wght@700;400;600` causes the Google Fonts API to return an error or unexpected CSS.
**Why it happens:** The CSS2 API requires weight values in ascending order.
**How to avoid:** Always write weights as `wght@400;600;700` (low to high).
**Warning signs:** Network request to fonts.googleapis.com returns a 400 error or empty CSS.

### Pitfall 3: Calendly min-height Missing on Mobile
**What goes wrong:** CLS is fixed on desktop (700px wrapper) but still present on mobile.
**Why it happens:** Developer adds `min-height: 700px` to `.booking__calendar-wrap` but forgets the mobile override sets the widget to 650px.
**How to avoid:** Add both rules — `min-height: 700px` on the base rule and `min-height: 650px` inside the `@media (max-width: 767px)` block.
**Warning signs:** Lighthouse CLS measured on mobile simulated viewport still shows shift in booking section.

### Pitfall 4: CSS Minifier Breaks Custom Property References
**What goes wrong:** Minifier collapses `var(--color-primary)` into something invalid.
**Why it happens:** Some aggressive minifiers attempt to resolve CSS custom properties. CleanCSS in "default" mode does not.
**How to avoid:** Use CleanCSS at default (not "advanced") optimisation level. Verify a sample of `var(--...)` references appear intact in the output.
**Warning signs:** Page renders with no colour after deploying minified CSS — custom property references removed.

### Pitfall 5: SVG img Tag Missing Width/Height Attributes
**What goes wrong:** Converting inline SVG to `<img>` without `width`/`height` attributes causes its own CLS, because the browser doesn't know the image aspect ratio before the file loads.
**Why it happens:** The original inline SVG has intrinsic dimensions via `viewBox`, but an `<img>` tag without explicit dimensions has unknown size until the response headers arrive.
**How to avoid:** Always include `width` and `height` attributes matching the SVG viewBox dimensions on the `<img>` tag.

---

## Code Examples

### PERF-01: Trimmed Google Fonts URL

```html
<!-- Source: Google Fonts CSS2 API — developers.google.com/fonts/docs/css2 -->
<!-- Removes Inter:500 and Plus Jakarta Sans:500 (5 total unused weight files) -->
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
```

### PERF-02: img Tag for External SVG (Pipeline Board)

```html
<!-- Replaces ~170-line inline SVG in .how__visual -->
<img src="./pipeline-board.svg"
     alt=""
     aria-hidden="true"
     loading="lazy"
     width="640"
     height="500">
```

CSS update needed in page.css:

```css
/* Change: .how__visual svg → .how__visual img */
@media (min-width: 900px) {
  .how__visual img {
    width: 100%;
    max-width: 960px;
    height: auto;
    filter: drop-shadow(0 6px 24px rgba(0,0,0,0.10));
    border-radius: 12px;
  }
}
```

### PERF-03: Calendly Wrapper min-height

```css
/* Add to existing .booking__calendar-wrap block in page.css */
.booking__calendar-wrap {
  border-radius: var(--radius-xl);
  overflow: hidden;
  min-height: 700px; /* pre-reserves space before Calendly iframe loads */
}
@media (max-width: 767px) {
  .booking__calendar-wrap {
    min-height: 650px;
  }
}
```

### PERF-05: Version Bump in index.html

```html
<!-- Increment from v=13 to v=14 after minification -->
<link rel="stylesheet" href="./page.css?v=14">
```

---

## Environment Availability

Step 2.6: SKIPPED — Phase 9 is pure HTML/CSS file edits + manual use of a web-based minifier. No CLI tools, runtimes, databases, or external services need to be installed.

The one external dependency is the CleanCSS online tool (cleancss.com), which is accessible via browser and requires no installation.

---

## Validation Architecture

### Test Framework
| Property | Value |
|----------|-------|
| Framework | Manual + Lighthouse CLI / Chrome DevTools Lighthouse panel |
| Config file | none — no automated test suite for static HTML |
| Quick run command | Open Chrome DevTools → Lighthouse → Performance tab → Analyse |
| Full suite command | Lighthouse audit on deployed Vercel URL (measures real network) |

No automated test framework exists for this project. Validation is visual/manual + Lighthouse.

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| PERF-01 | Only 3 Inter weights and 4 PJS weights fetched | manual | Chrome DevTools Network tab: filter `fonts.gstatic.com` requests | ❌ Wave 0 N/A — manual only |
| PERF-02 | SVG mockups absent from initial render waterfall | manual | Chrome DevTools Network tab: confirm pipeline-board.svg and outreach-sequence.svg load after initial parse | ❌ Wave 0 N/A — manual only |
| PERF-03 | No layout shift in booking section on page load | manual | Chrome DevTools Performance → Layout Shift regions; or Lighthouse CLS score | ❌ Wave 0 N/A — manual only |
| PERF-04 | Floating CTA absent — zero CLS contribution | grep verify | `grep -n "floating\|float-cta" index.html` returns no matches | ❌ Wave 0 N/A — manual verify |
| PERF-05 | page.css served minified with version bump | manual | Open DevTools Sources → page.css → verify single line or minimal whitespace; check query string | ❌ Wave 0 N/A — manual only |

### Sampling Rate
- **Per task:** Visual check in browser after each file change
- **Per wave merge:** Full Lighthouse run on local file (via `python -m http.server` or VS Code Live Server equivalent)
- **Phase gate:** Lighthouse Performance score 90+ on deployed Vercel URL before `/gsd:verify-work`

### Wave 0 Gaps
None — no automated test infrastructure is required for this phase. All validation is manual (Lighthouse + DevTools). Wave 0 for this phase is zero-gap.

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Google Fonts API v1 (`?family=Inter:400,600`) | CSS2 API (`?family=Inter:wght@400;600;700&display=swap`) | 2018 | CSS2 supports variable fonts and `display=swap` for FOIT prevention |
| `<img loading="lazy">` not supported | `loading="lazy"` native in all evergreen browsers | 2020 (Chrome 77, Firefox 75, Safari 15.4) | No JS needed for lazy loading images |
| CLS measured per page load | CLS now averaged over longest 5-second window session | 2021 (Chrome 96) | Isolated shifts matter less; sustained shifts from scroll-triggered content matter more |

---

## Open Questions

1. **Weight 500 in page.css**
   - What we know: `font-weight: 500` appears 5 times in page.css. The Inter 500 weight file is currently downloaded.
   - What's unclear: Whether those rules use `--font-body` (Inter) or `--font-display` (Plus Jakarta Sans). If they're Inter 500, removing Inter 500 from the font URL will cause browser fallback to weight 400 — visually similar but not identical.
   - Recommendation: Planner should include a grep step to identify which elements use weight 500 + Inter. If purely display-font elements, removing Inter 500 is safe. If body elements, change CSS weight to 400 or 600.

2. **page.css minification — overwrite vs rename**
   - What we know: current link is `./page.css?v=13`. Version bump is the existing cache-bust pattern.
   - What's unclear: Whether the project has any tooling that regenerates page.css (it does not — manually maintained).
   - Recommendation: Overwrite page.css in place, increment query string to `?v=14`. Do not rename file.

---

## Sources

### Primary (HIGH confidence)
- Live codebase audit — `index.html`, `page.css`, `style.css`, `base.css` read directly
- [Google Fonts CSS2 API docs](https://developers.google.com/fonts/docs/css2) — weight syntax, `display=swap`
- [WICG Layout Instability README](https://github.com/WICG/layout-instability/blob/main/README.md) — `position:fixed` exclusion from CLS

### Secondary (MEDIUM confidence)
- [web.dev — Optimize CLS](https://web.dev/articles/optimize-cls) — general CLS prevention patterns
- [web.dev — CLS definition](https://web.dev/articles/cls) — metric definition
- Git log `909aaf2` — confirms floating CTA removed before Phase 7

### Tertiary (LOW confidence)
- [andreaverlicchi.eu — SVG performance](https://www.andreaverlicchi.eu/blog/loading_svg_images_icons_performance_considerations/) — confirmed externalize + img approach
- [natclark.com — CLS fixes](https://natclark.com/how-to-fix-cumulative-layout-shift-cls-in-2025/) — Calendly min-height pattern corroborated

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — all patterns verified against official specs and live codebase
- Architecture: HIGH — code examples derived from actual file contents, not generic patterns
- Pitfalls: HIGH — identified from direct inspection of the gap between current code and requirements
- PERF-04 (floating CTA): HIGH — confirmed absent via git log + full-file grep

**Research date:** 2026-04-28
**Valid until:** 2026-07-28 (stable APIs — Google Fonts CSS2, HTML loading attribute, CSS min-height)
