# Phase 4: Conversion Sections — Research

**Researched:** 2026-04-27
**Domain:** Vanilla HTML/CSS/JS — comparison table, FAQ accordion, ROI calculator, typographic audit
**Confidence:** HIGH

---

## Summary

Phase 4 adds three interactive conversion sections to a static HTML/CSS/vanilla JS page, plus a typography consistency pass. All decisions are fully locked in the UI-SPEC — this research role is primarily to document what the executor needs to know about working safely in the existing codebase, pitfalls for each pattern, and the exact edit targets.

The page already has scaffold HTML for the comparison table (`.comparison`) and FAQ (`.faq`) sections — both contain `<p class="scaffold-placeholder">` to replace. The ROI calculator section (`#roi`) does not yet exist and must be inserted as new HTML between `.faq` and `.booking`. The hero h1 font-size clamp minimum needs a single value change from `2.4rem` to `3.5rem`. The `.section-label` weight needs updating from `600` to `700`.

**Primary recommendation:** Work in four discrete plans: (1) comparison table HTML+CSS, (2) FAQ accordion HTML+CSS+JS, (3) ROI calculator HTML+CSS+JS, (4) typographic audit — hero h1 fix + eyebrow weight fix + eyebrow presence audit across all sections.

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| SECT-01 | Comparison table on dark navy, 8 rows, Leadzly column highlighted green | Scaffold exists at `.comparison#comparison`; replace placeholder; full spec in UI-SPEC §Comparison Table |
| SECT-02 | FAQ accordion, 7 questions, one-at-a-time, max-width 720px | Scaffold exists at `.faq#faq`; replace placeholder; full spec in UI-SPEC §FAQ Accordion |
| SECT-03 | ROI calculator, 3 inputs, 2 outputs, client-side JS only | New section — no scaffold; insert between `.faq` and `.booking`; full spec in UI-SPEC §ROI Calculator |
| DSGN-01 | Hero h1 bold ≥3.5rem; eyebrow label on every section | Single CSS value change for h1; weight fix for `.section-label`; HTML audit for missing eyebrow spans |
</phase_requirements>

---

## Project Constraints (from CLAUDE.md)

- **Tech stack:** HTML/CSS/vanilla JS only — no build system, no npm, no frameworks
- **No new font imports** — Inter and Plus Jakarta Sans already loaded via Google Fonts
- **Logo assets:** Use existing SVG/PNG files only
- **No backend:** All JS is client-side only — no server calls, no fetch for calculator
- **JS delivery:** New `<script>` block before `</body>` in `index.html`, or extracted to `page.js`. Keep as IIFE to avoid global scope pollution. No libraries.
- **CSS:** New classes go in `page.css` only — do not modify `style.css` or `base.css` design tokens
- **Design tokens:** All spacing, color, typography MUST use CSS custom property tokens from `style.css :root` — no hardcoded hex or px values

---

## Standard Stack

### Core (this project)

| File | Purpose | Phase 4 role |
|------|---------|--------------|
| `index.html` | Single-page HTML | Add ROI section HTML; replace scaffold placeholders; add `<script>` block |
| `page.css` | Component styles | Add comparison, FAQ, ROI, and eyebrow CSS classes |
| `style.css` | Design token `:root` | Read-only — tokens referenced, not modified |
| `base.css` | Reset + global base | Read-only — provides focus ring, font base |

### No supporting libraries

Per project constraints and UI-SPEC §Registry Safety: no external scripts added in Phase 4. All JS is inline vanilla.

---

## Architecture Patterns

### Existing Section Pattern (MUST match)

Every section follows this structure:

```html
<section class="[name]" id="[name]">
  <div class="container">
    <div class="section-header">
      <span class="section-label">[EYEBROW LABEL]</span>
      <h2 class="section-title">[Title]</h2>
      <p class="section-subtitle">[Subtitle]</p>
    </div>
    <!-- content -->
  </div>
</section>
```

CSS for every section uses:
```css
.[name] {
  padding: clamp(var(--space-12), 8vw, var(--space-24)) 0;
  background: var(--[token]);
}
```

All three new/updated sections MUST use this exact clamp pattern. Deviating breaks visual rhythm.

### IIFE Pattern for JS (existing convention)

All existing scripts in `index.html` use IIFEs:
```js
(function() {
  // scoped logic
})();
```

New FAQ and ROI calculator scripts MUST follow the same pattern. Add them as new IIFE blocks before `</body>`, after the existing script block. Do not merge into the existing script block — keep each feature isolated.

### CSS Class Namespace Convention

New classes are prefixed by section name:
- Comparison: `.comparison__table`, `.comparison__table-wrap`, `.comparison__th--leadzly`, `.comparison__cell--leadzly`, `.comparison__row`, `.comparison__label`
- FAQ: `.faq__list`, `.faq__item`, `.faq__item--active`, `.faq__question`, `.faq__answer`, `.faq__icon`
- ROI: `.roi__card`, `.roi__inputs`, `.roi__field`, `.roi__label`, `.roi__input`, `.roi__hint`, `.roi__outputs`, `.roi__output`, `.roi__output-label`, `.roi__output-value`, `.roi__output-sub`, `.roi__disclaimer`

---

## Specific Implementation Guidance

### SECT-01: Comparison Table

**Edit target in `index.html`:** Replace the `<p class="scaffold-placeholder">Comparison table coming in Phase 4.</p>` inside `.comparison > .container` with the full `<div class="comparison__table-wrap">` + `<table>` structure.

**Edit target in `page.css`:** The `.comparison` rule at line 1152 already sets `padding` and `background: var(--section-tint)`. The background MUST be changed to `var(--color-dark)` as per SECT-01 requirement and UI-SPEC. New CSS classes (`.comparison__table-wrap`, `.comparison__table`, etc.) go after the existing `.comparison` rule.

**Key CSS facts from UI-SPEC:**
- Table wrapper: `overflow-x: auto; -webkit-overflow-scrolling: touch`
- Table: `min-width: 560px; width: 100%; border-collapse: collapse`
- Leadzly column header (`th.comparison__th--leadzly`): `background: var(--color-primary-highlight); color: var(--color-primary); font-weight: 700`
- Leadzly body cells (`td.comparison__cell--leadzly`): `background: var(--color-primary-soft); color: var(--color-primary); font-weight: 700`
- Row alternation: odd rows `var(--color-dark)`, even rows `var(--color-dark-surface)`
- All text inside section: `var(--color-text-inverse)` (#ffffff), except Leadzly column which uses `var(--color-primary)`
- Muted text (non-Leadzly non-header cells): `rgba(255,255,255,0.65)`
- Section title and subtitle on dark background: need `color: var(--color-text-inverse)` override (the global `.section-title` uses `var(--color-text)` which is dark)

**Pitfall — section-title colour on dark background:** The global `.section-title` rule sets `color: var(--color-text)` (#111827). Inside `.comparison` (dark navy background), this makes the heading unreadable. Override locally: `.comparison .section-title { color: var(--color-text-inverse); }` and `.comparison .section-subtitle { color: rgba(255,255,255,0.75); }`.

**Inline SVG icons:** The UI-SPEC specifies inline SVG for ✓ and ✗ icons — do NOT use HTML entities alone. Use:
- Checkmark: `<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>` — wrap in a `<span>` with `color: var(--color-primary)` in the Leadzly column
- X mark: `<svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>` — wrap in `<span>` with `color: rgba(255,255,255,0.4)`
- Partial (`~` text): plain text character, color `rgba(255,255,255,0.65)`

**8 table rows content** (from UI-SPEC — executor should copy exactly):

| Row | Leadzly | In-House | Other Agencies |
|-----|---------|----------|----------------|
| Setup time | Ready in 30 days | 3–6 months to hire & ramp | Varies (often slow) |
| Monthly cost | From £1,500/mo | £40,000+/yr salary + tools | Often opaque pricing |
| Meetings guaranteed | ✓ Yes — guaranteed | ✗ No guarantee | ✗ No guarantee |
| UK-based team | ✓ Northern Ireland team | ✓ If you hire locally | ✗ Typically offshore |
| Multi-channel outreach | ✓ Calls, email, LinkedIn | ✗ Depends on hire | ~ Email only (usually) |
| Reporting & visibility | ✓ Weekly transparent reports | ✗ Self-managed | ~ Varies by agency |
| No long-term lock-in | ✓ Month-to-month | N/A | ✗ Usually 6–12 mo contracts |
| Performance guarantee | ✓ 30-day money back | ✗ None | ✗ None |

---

### SECT-02: FAQ Accordion

**Edit target in `index.html`:** Replace `<p class="scaffold-placeholder">FAQ accordion coming in Phase 4.</p>` inside `.faq > .container` with the `<div class="faq__list">` + 7 `<div class="faq__item">` blocks.

**ARIA pattern for accordion:** Each `<button>` must have:
- `aria-expanded="false"` (toggled to `"true"` when open)
- `aria-controls="faq-answer-[n]"` pointing to the answer div ID
The answer `<div>` must have:
- `id="faq-answer-[n]"`
- `role="region"`
- `aria-labelledby="faq-btn-[n]"`

**Height animation pattern (max-height trick):**
```css
.faq__answer {
  overflow: hidden;
  max-height: 0;
  transition: max-height 300ms cubic-bezier(0.16, 1, 0.3, 1);
}
.faq__item--active .faq__answer {
  max-height: 400px; /* generous upper bound */
}
```
The max-height value must be larger than any realistic answer height. 400px is sufficient for 3–5 lines of text. Do not use `height: auto` in a CSS transition — it does not animate.

**JS accordion logic:**
```js
(function() {
  var items = document.querySelectorAll('.faq__item');
  items.forEach(function(item) {
    var btn = item.querySelector('.faq__question');
    if (!btn) return;
    btn.addEventListener('click', function() {
      var isActive = item.classList.contains('faq__item--active');
      // collapse all
      items.forEach(function(i) {
        i.classList.remove('faq__item--active');
        var b = i.querySelector('.faq__question');
        if (b) b.setAttribute('aria-expanded', 'false');
      });
      // open clicked (unless it was already open)
      if (!isActive) {
        item.classList.add('faq__item--active');
        btn.setAttribute('aria-expanded', 'true');
      }
    });
  });
})();
```

**Chevron rotation:**
```css
.faq__icon {
  transition: transform 200ms ease;
  flex-shrink: 0;
}
.faq__item--active .faq__icon {
  transform: rotate(180deg);
}
```

**Touch target:** Button padding of `var(--space-4) 0` (16px top + 16px bottom + ~14px text = ~46px) meets the 44px minimum touch target requirement.

**7 FAQ questions** — exact copy from UI-SPEC:
1. How quickly can you start generating meetings?
2. What does "qualified meeting" actually mean?
3. Do I have to sign a long-term contract?
4. What industries do you work with?
5. How is Leadzly different from a typical cold-calling agency?
6. What information do you need to get started?
7. What happens if you don't hit the meeting targets?

Full answer copy is in UI-SPEC §FAQ Accordion — copy verbatim.

---

### SECT-03: ROI Calculator

**Edit target in `index.html`:** INSERT a new `<section class="roi" id="roi">` block immediately before the `.booking` section. There is no scaffold to replace — this is a net-new HTML block.

**Edit target in `page.css`:** Add `.roi` and all child class rules. No existing scaffold CSS exists for `.roi`.

**Section background:** Use `var(--section-tint)` (#f8fafb). The alternating rhythm around it is: proven (white) → comparison (dark navy) → pricing (surface) → FAQ (white) → **ROI (tint)** → booking (green). This maintains visual rhythm without two consecutive white sections.

**Calculation logic (exact formula from UI-SPEC):**
```js
(function() {
  var dealInput = document.getElementById('roi-deal-value');
  var currentInput = document.getElementById('roi-current-meetings');
  var targetInput = document.getElementById('roi-target-meetings');
  var monthlyOutput = document.getElementById('roi-monthly');
  var annualOutput = document.getElementById('roi-annual');

  var CONVERSION_RATE = 0.10;

  function calculate() {
    var deal = parseFloat(dealInput.value) || 0;
    var current = parseFloat(currentInput.value) || 0;
    var target = parseFloat(targetInput.value) || 0;
    var additional = Math.max(0, target - current);
    var monthly = additional * deal * CONVERSION_RATE;
    var annual = monthly * 12;
    monthlyOutput.textContent = '£' + monthly.toLocaleString('en-GB');
    annualOutput.textContent = '£' + annual.toLocaleString('en-GB');
  }

  [dealInput, currentInput, targetInput].forEach(function(el) {
    if (el) el.addEventListener('input', calculate);
  });

  calculate(); // initialise on load
})();
```

**Default values for initial load display:**
- `roi-deal-value`: 5000
- `roi-current-meetings`: 4
- `roi-target-meetings`: 20
- Initial monthly output: £8,000 (= (20-4) × 5000 × 0.10)
- Initial annual output: £96,000 (£8,000 × 12)

Note: The UI-SPEC shows £80,000/£960,000 as example outputs. That reflects a different default (16 additional × 5000 × 0.10 = 8000 — actually still £8,000). The UI-SPEC example values may be from earlier drafts; the calculation formula is definitive. Executor should verify the math with the actual defaults before finalising.

**Input grid:** 3 columns on ≥768px, 1 column on mobile:
```css
.roi__inputs {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-6);
}
@media (min-width: 768px) {
  .roi__inputs { grid-template-columns: repeat(3, 1fr); }
}
```

**Output grid:** 2 columns on ≥640px, 1 column on mobile:
```css
.roi__outputs {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--space-6);
  margin-top: var(--space-8);
}
@media (min-width: 640px) {
  .roi__outputs { grid-template-columns: repeat(2, 1fr); }
}
```

---

### DSGN-01: Typography Audit

**Two CSS edits in `page.css`:**

1. Hero h1 font-size (line 274 in current `page.css`):
   - Find: `font-size: clamp(2.4rem, 3.5vw + 0.5rem, 4.25rem);`
   - Replace: `font-size: clamp(3.5rem, 3.5vw + 0.5rem, 4.75rem);`
   - (Font-weight is already 800 — satisfies "bold at ≥3.5rem")

2. Section-label weight (line 19 in current `page.css`):
   - Find: `font-weight: 600;`
   - Replace: `font-weight: 700;`

**HTML eyebrow audit** — verify every `<div class="section-header">` has a `<span class="section-label">`. Current state from reading `index.html`:

| Section | Has `.section-label`? | Required eyebrow text |
|---------|----------------------|----------------------|
| Hero | No `.section-header` — uses `.hero__eyebrow` pill | Retain existing pill format — no change |
| Pain Points | YES — "Sound Familiar?" | Confirm |
| Services | YES — "What We Do" | Confirm |
| About | YES — "About Us" | Confirm |
| Multi-Channel | YES — "Our Approach" | Confirm |
| How It Works | YES — "Our Process" | Confirm |
| Industries | YES — "Industries" | Confirm |
| Proven | YES — "Results" | Confirm |
| Comparison | YES — "Why Leadzly" (scaffold already present) | Confirm |
| Pricing | YES — "Pricing" | Confirm |
| FAQ | YES — "FAQ" (scaffold already present) | Confirm |
| ROI Calculator | NEW — add "ROI Calculator" | Add in new section HTML |
| Booking | YES — "Book a Call" (scaffold already present) | Confirm |

All existing eyebrow spans are present. No HTML additions required for DSGN-01 other than the new ROI section.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Accordion height animation | Custom JS height measurement with `getBoundingClientRect()` | CSS `max-height` transition trick | No JS measurement needed; max-height from 0 to generous fixed value animates smoothly, browser handles it |
| Number formatting | Custom £ formatter with manual comma insertion | `Number.toLocaleString('en-GB')` | Native browser API handles GBP formatting correctly, including thousands separators |
| Keyboard accordion | Custom keydown handlers for Enter/Space | Native `<button>` element | Browsers handle Enter/Space on `<button>` natively — no extra JS needed |
| Mobile table scroll | CSS table transform or column hiding | `overflow-x: auto` on wrapper + `min-width` on table | Simplest, most robust pattern; native scroll behaviour on iOS and Android |

---

## Common Pitfalls

### Pitfall 1: Dark Section Text Colour Not Overridden
**What goes wrong:** `.section-title` uses `color: var(--color-text)` (#111827) globally. Inside `.comparison` (dark navy `#0f172a` background), this renders the heading as near-invisible dark text on dark background.
**Why it happens:** Global rule applied without scoped override.
**How to avoid:** Add `.comparison .section-title { color: var(--color-text-inverse); }` and `.comparison .section-subtitle { color: rgba(255,255,255,0.75); }` to the comparison CSS block in `page.css`.
**Warning signs:** Section heading appears invisible or barely visible on dark background in browser preview.

### Pitfall 2: Scaffold Background Not Updated
**What goes wrong:** The `.comparison` rule in `page.css` (line 1152) sets `background: var(--section-tint)` (pale off-white). SECT-01 requires `var(--color-dark)` (dark navy `#0f172a`). If the executor only replaces the HTML placeholder without updating this CSS rule, the comparison section will have a light background instead of dark.
**Why it happens:** Two separate edits required — HTML content replacement AND CSS background change.
**How to avoid:** Plan explicitly calls out that `.comparison` background must change from `var(--section-tint)` to `var(--color-dark)`.

### Pitfall 3: FAQ max-height Too Small
**What goes wrong:** If `max-height` on `.faq__answer` when active is set too low (e.g., 200px), longer answers get clipped — overflow is hidden.
**Why it happens:** Trying to set a tight max-height for a "clean" animation.
**How to avoid:** Use 400px as specified in UI-SPEC — it is generous enough for all 7 answers (longest is ~3 sentences, ~90px). The animation speed (300ms) masks the fact that the height transition starts from the actual content height not the max-height.

### Pitfall 4: ROI Calculator `NaN` Outputs
**What goes wrong:** If a user clears an input field, `parseFloat('')` returns `NaN`. Multiplying produces `NaN`, `toLocaleString` on `NaN` returns `"NaN"`, and the output shows `"£NaN"`.
**Why it happens:** Missing null/NaN guard.
**How to avoid:** Use `parseFloat(el.value) || 0` pattern — this short-circuits to 0 when the value is empty, null, or NaN.

### Pitfall 5: Section-divider Rule Adds Border Above ROI Section
**What goes wrong:** The existing CSS rule `main > section + section::before` adds a decorative green gradient border above every section that follows another section. The ROI section (new, inserted between FAQ and booking) will automatically get this divider. This is intentional design behaviour — no action needed, but executor should be aware it happens automatically.
**Why it matters:** Not a pitfall per se, but if the divider looks wrong (e.g., on the dark comparison section), check whether the `::before` pseudo-element is rendering with correct opacity on dark backgrounds. The gradient uses `rgba(22, 163, 74, 0.35)` which is subtle on light backgrounds but may be invisible on dark. On the comparison section (dark navy), the 35% opacity green on dark navy is nearly invisible — acceptable, no fix needed.

### Pitfall 6: ROI Section Inserted in Wrong Position
**What goes wrong:** If the ROI `<section>` is inserted after `.booking` or before `.pricing`, the page section order breaks.
**Why it happens:** index.html has many scaffold sections; easy to miscount.
**How to avoid:** Target insertion point is immediately before `<section class="booking" id="booking">` — find that comment line `<!-- ============ BOOKING` and insert above it.

### Pitfall 7: `font-weight: 600` on `.section-label` in Other CSS Declarations
**What goes wrong:** If `section-label` weight is only updated in the class rule but there are inline overrides or other selectors that specify weight 600, the fix is incomplete.
**Current state:** Only one `.section-label` rule exists in `page.css` (line 19). No overrides in `style.css` or `base.css`. Single edit is sufficient.

---

## Code Examples

### Comparison Table — Minimal Structure
```html
<!-- Source: UI-SPEC §Comparison Table + existing project conventions -->
<div class="comparison__table-wrap">
  <table class="comparison__table">
    <thead>
      <tr>
        <th></th>
        <th class="comparison__th--leadzly">Leadzly</th>
        <th>In-House</th>
        <th>Other Agencies</th>
      </tr>
    </thead>
    <tbody>
      <tr class="comparison__row">
        <td class="comparison__label">Setup time</td>
        <td class="comparison__cell--leadzly">
          <svg ...checkmark.../> Ready in 30 days
        </td>
        <td>3–6 months to hire &amp; ramp</td>
        <td>Varies (often slow)</td>
      </tr>
      <!-- repeat for 8 rows -->
    </tbody>
  </table>
</div>
```

### FAQ Item — Minimal Structure
```html
<!-- Source: UI-SPEC §FAQ Accordion + ARIA accordion pattern -->
<div class="faq__item" id="faq-item-1">
  <button class="faq__question"
          id="faq-btn-1"
          aria-expanded="false"
          aria-controls="faq-answer-1">
    <span>How quickly can you start generating meetings?</span>
    <svg class="faq__icon" width="16" height="16" viewBox="0 0 24 24"
         fill="none" stroke="currentColor" stroke-width="2"
         stroke-linecap="round" stroke-linejoin="round">
      <polyline points="6 9 12 15 18 9"/>
    </svg>
  </button>
  <div class="faq__answer"
       id="faq-answer-1"
       role="region"
       aria-labelledby="faq-btn-1">
    <p>We typically onboard new clients within 30 days...</p>
  </div>
</div>
```

### ROI Input Field — Minimal Structure
```html
<!-- Source: UI-SPEC §ROI Calculator -->
<div class="roi__field">
  <label class="roi__label" for="roi-deal-value">Average deal value (£)</label>
  <input class="roi__input" type="number" id="roi-deal-value"
         min="0" step="500" value="5000"
         aria-describedby="roi-deal-value-hint">
  <span class="roi__hint" id="roi-deal-value-hint">
    The average value of a closed deal in your pipeline
  </span>
</div>
```

### `toLocaleString` for GBP formatting
```js
// Source: MDN Web Docs — Number.prototype.toLocaleString()
// Produces "£8,000" format
var formatted = '£' + (8000).toLocaleString('en-GB');
// → "£8,000"
```

---

## State of the Art

| Topic | Approach Used | Notes |
|-------|--------------|-------|
| Accordion animation | CSS `max-height` transition (0 → fixed max) | No JS height measurement; CSS-only animation with JS class toggle |
| Accordion ARIA | `aria-expanded` + `aria-controls` + `role="region"` | W3C ARIA Authoring Practices pattern |
| Real-time calculator | `input` event listener + direct DOM text update | No debouncing needed for simple arithmetic; instant feedback preferred |
| Table mobile scroll | `overflow-x: auto` wrapper + `min-width` on table | Established cross-browser pattern; no JS required |

---

## Environment Availability

Step 2.6: SKIPPED — Phase 4 is purely HTML/CSS/vanilla JS changes. No external tools, runtimes, databases, CLI utilities, or services required beyond a text editor and browser.

---

## Validation Architecture

No automated test infrastructure exists for this project (static HTML/CSS site with no build step). Manual verification is the only approach.

### Phase Requirements → Test Map

| Req ID | Behaviour | Test Type | Method |
|--------|-----------|-----------|--------|
| SECT-01 | Comparison table renders 8 rows, Leadzly column green-highlighted on dark navy background | Visual manual | Open index.html in browser, scroll to comparison section |
| SECT-01 | Comparison table scrolls horizontally on mobile viewport | Manual mobile | Browser DevTools device emulation at 375px width |
| SECT-02 | FAQ accordion opens one item at a time, others collapse | Manual interaction | Click each of the 7 questions in sequence |
| SECT-02 | FAQ chevron rotates 180° on open | Visual manual | Observe icon during accordion interaction |
| SECT-02 | FAQ keyboard navigation works | Manual keyboard | Tab to question button, press Enter/Space |
| SECT-03 | ROI outputs update in real time on input change | Manual interaction | Change each input, verify outputs update instantly |
| SECT-03 | ROI handles empty/negative inputs with £0 output | Edge case manual | Clear each input field, verify "£0" shown |
| SECT-03 | ROI formula correct: (target - current) × deal × 0.10 × 12 | Manual calculation | Input known values, verify outputs match manual calculation |
| DSGN-01 | Hero h1 ≥3.5rem at narrowest viewport | Visual/DevTools | Check computed font-size at 320px viewport in DevTools |
| DSGN-01 | Every section has a visible green eyebrow label | Visual manual | Scroll full page top to bottom |

### Human Checkpoint Criteria

At end of phase, a human visual verification pass should confirm:
1. Comparison section is dark navy with white text and green Leadzly column — readable at desktop and mobile
2. FAQ opens/closes correctly, chevron rotates, others collapse when a new one opens
3. ROI calculator updates instantly when any input is changed
4. Hero headline is visually large and bold at all viewport sizes
5. Every section has a small green uppercase label above its main heading

---

## Suggested Plan Structure

Based on the scope, 3 plans are appropriate (DSGN-01 can be folded into the final plan as it is small):

| Plan | Covers | Edits |
|------|--------|-------|
| 04-01-PLAN.md | SECT-01: Comparison table | HTML replace placeholder + CSS new classes + `.comparison` bg change |
| 04-02-PLAN.md | SECT-02: FAQ accordion | HTML replace placeholder + CSS new classes + JS IIFE |
| 04-03-PLAN.md | SECT-03 + DSGN-01: ROI calculator + typography | HTML new section + CSS new classes + JS IIFE + 2 CSS value changes |

---

## Sources

### Primary (HIGH confidence)
- `index.html` — live codebase, exact scaffold section positions, existing JS patterns, ARIA usage
- `page.css` — existing CSS rules, design token usage, line numbers for edit targets
- `style.css` — all CSS custom property tokens, confirmed available values
- `04-UI-SPEC.md` — authoritative visual and interaction contract for Phase 4; generated and verified for this project

### Secondary (MEDIUM confidence)
- MDN Web Docs patterns — `max-height` animation, `toLocaleString('en-GB')`, native `<button>` keyboard behaviour, ARIA accordion pattern (W3C APG)

---

## Metadata

**Confidence breakdown:**
- Implementation targets (edit locations): HIGH — confirmed by reading actual files
- CSS token availability: HIGH — confirmed against `style.css :root`
- JS patterns: HIGH — confirmed against existing scripts in `index.html`
- ARIA patterns: HIGH — standard W3C patterns, no library complexity
- Content copy: HIGH — sourced verbatim from `04-UI-SPEC.md`

**Research date:** 2026-04-27
**Valid until:** Remains valid until any of `index.html`, `page.css`, or `style.css` are significantly restructured (not expected within this milestone)
