# Phase 1: Foundation & Hygiene - Context

**Gathered:** 2026-04-24
**Status:** Ready for planning

<domain>
## Phase Boundary

Add the missing hygiene layer to the existing site: complete meta/OG tags, create a favicon, and add a GDPR consent banner. NAV-01, NAV-02, and NAV-03 are already fully implemented in the existing code — no work needed there.

**Scope anchor:** This phase does NOT restructure HTML or touch any section content. It only adds what's missing from `<head>` and appends a cookie banner + gating logic.

</domain>

<decisions>
## Implementation Decisions

### Pre-Implemented (Verified in Code — No Work Needed)
- **D-00a:** NAV-01 is already done — nav is `position: fixed`, has `backdrop-filter: blur(16px)` always on, and JS adds `.nav--scrolled` (box-shadow) when `scrollY > 50`. Requirements met.
- **D-00b:** NAV-02 is already done — hamburger HTML button, CSS open/close states (`.nav__hamburger--active`, `.nav__links--open`), and click-handler JS are all in place.
- **D-00c:** NAV-03 is already done — all external links (`<a target="_blank">`) already have `rel="noopener noreferrer"`.

### Favicon (QWIN-02)
- **D-01:** Use `Leadzly-logo_without-text.svg` as the SVG favicon source (the green funnel/arrow icon — already in repo).
- **D-02:** Generate two formats: SVG favicon (`favicon.svg`) for modern browsers, and 32×32 PNG fallback (`favicon-32.png`) for Safari and older browsers. The PNG file `Leadzly-logo_without-text.png` already exists in the repo — use it directly as the PNG favicon.
- **D-03:** Add `<link rel="icon" type="image/svg+xml" href="./Leadzly-logo_without-text.svg">` and `<link rel="icon" type="image/png" sizes="32x32" href="./Leadzly-logo_without-text.png">` to `<head>`. No apple-touch-icon needed.

### OG Tags Completion (QWIN-01)
- **D-04:** Existing OG tags (og:title, og:description, og:type) are already present — only add the two missing tags.
- **D-05:** `og:image` → use `Leadzly-logo_with-text.png` (already in repo). Absolute URL: `https://leadzly.co/Leadzly-logo_with-text.png`.
- **D-06:** `og:url` → `https://leadzly.co/`
- **D-07:** Also add `og:image:type` (`image/png`) and `og:image:alt` (`Leadzly logo`) for completeness.

### GDPR Cookie Banner (QWIN-04)
- **D-08:** **Position:** Bottom bar, full width — fixed to the bottom of the viewport.
- **D-09:** **Controls:** Accept-only — single "Accept" button. No Decline or Preferences options.
- **D-10:** **GA4 gating:** GA4 only initialises AFTER the user clicks Accept. The existing `<script type="text/partytown">` GA4 block must be conditionally activated on consent. On first visit with no stored consent: GA4 does not fire. After Accept is clicked: consent stored in `localStorage` and GA4 activates for current + future sessions.
- **D-11:** **Persistence:** Use `localStorage` key (e.g. `leadzly_cookie_consent = 'accepted'`) to remember consent. Banner does not reappear after acceptance.
- **D-12:** **No privacy policy link** — banner text only (e.g. "We use cookies to improve your experience.") + Accept button. Can be added later.
- **D-13:** **Banner styling:** Match brand — dark navy background (`#0f172a`), white text, green Accept button (`#16a34a`). Vanilla JS only, no external library.

### Claude's Discretion
- Banner copy wording (within the spirit of "cookies to improve experience" — keep it short, honest, non-alarming)
- Exact CSS for banner (should feel native to the existing design system — use CSS custom properties from `style.css`)
- Exact localStorage key name
- Animation for banner appear/dismiss (subtle slide-up or fade-in)
- Whether GA4 re-fires immediately on Accept in same session or only from next page load (recommend: fire immediately using the existing `gtag()` function already defined)

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Existing Code to Read
- `index.html` — Full existing HTML structure, inline JS blocks, existing OG tags, nav markup, existing Partytown/GA4 setup
- `style.css` — Full design system (CSS custom properties, colour palette, spacing scale, shadows, radius tokens)
- `page.css` — Nav CSS (`.nav`, `.nav--scrolled`, `.nav__hamburger`, `.nav__links--open`) to ensure banner CSS doesn't conflict
- `base.css` — Base reset and typography

### Existing Assets
- `Leadzly-logo_without-text.svg` — SVG source for favicon (already in repo root)
- `Leadzly-logo_without-text.png` — PNG favicon fallback (already in repo root, use as-is)
- `Leadzly-logo_with-text.png` — PNG for og:image (already in repo root)

### Requirements
- `.planning/REQUIREMENTS.md` §QWIN-01, §QWIN-02, §QWIN-04, §NAV-01–03 — acceptance criteria to verify against

No external ADRs or specs — all decisions captured in this context file.

</canonical_refs>

<code_context>
## Existing Code Insights

### Reusable Assets
- `style.css` CSS custom properties (`--color-dark: #0f172a`, `--color-primary: #16a34a`, `--color-text-inverse: #fff`, etc.) — use these throughout the banner CSS, not hardcoded hex values
- Existing `.btn.btn--primary` class — can be applied or mirrored for the Accept button to stay consistent

### Established Patterns
- All JS is vanilla, inline in a `<script>` block at the bottom of `<body>` — new JS (banner + GA4 gating) should follow the same IIFE pattern `(function() { ... })();`
- GA4 is loaded via Partytown (`type="text/partytown"`) for web worker offloading — the gating approach must work with this setup (conditionally init, not remove the script tag)
- `localStorage` is already the implied pattern for client-side persistence (theme toggle reads/writes state — can mirror that approach)

### Integration Points
- `<head>` — add favicon `<link>` tags and the two missing OG tags here
- End of `<body>` before closing `</body>` — append cookie banner HTML
- Inline `<script>` block — add banner JS and GA4 gating logic as new IIFE at the end of the existing script block (or a new `<script>` tag)

</code_context>

<specifics>
## Specific Ideas

- User confirmed to use `Leadzly-logo_without-text.svg` / `.png` directly from repo root for favicon — no new asset creation needed
- User confirmed to use `Leadzly-logo_with-text.png` for og:image — absolute URL `https://leadzly.co/Leadzly-logo_with-text.png`
- GA4 gating is real (not just cosmetic) — this is the most technically interesting part of the phase

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope.

</deferred>

---

*Phase: 01-foundation-hygiene*
*Context gathered: 2026-04-24*
