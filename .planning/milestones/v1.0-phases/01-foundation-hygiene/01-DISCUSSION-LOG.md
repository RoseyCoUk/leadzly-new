# Phase 1: Foundation & Hygiene - Discussion Log

> **Audit trail only.** Do not use as input to planning, research, or execution agents.
> Decisions are captured in CONTEXT.md — this log preserves the alternatives considered.

**Date:** 2026-04-24
**Phase:** 01-foundation-hygiene
**Areas discussed:** Favicon, OG image, GDPR cookie banner

---

## Pre-Discussion Finding

Codebase scout revealed NAV-01, NAV-02, and NAV-03 are already fully implemented:
- Nav is `position: fixed` with `backdrop-filter: blur(16px)` and scroll-driven shadow
- Hamburger HTML, CSS, and JS all in place
- All external links already have `rel="noopener noreferrer"`

These were not discussed — already done.

---

## Favicon

| Option | Description | Selected |
|--------|-------------|----------|
| Green funnel icon | Extract the existing funnel/arrow shape from nav SVG | |
| 'L' lettermark | Simple bold 'L' in green on dark navy | |
| Use Leadzly-logo_without-text | User directed to use the existing file | ✓ |

**User's choice:** Use `Leadzly-logo_without-text.svg` (and `.png`) directly from the repo root

---

## Favicon Format

| Option | Description | Selected |
|--------|-------------|----------|
| SVG + PNG fallback | SVG for modern browsers + 32×32 PNG for Safari/older | ✓ |
| SVG only | Single SVG favicon, modern browsers only | |
| SVG + PNG + apple-touch-icon | Full set including iOS home screen bookmark icon | |

**User's choice:** SVG + PNG fallback (recommended)
**Notes:** PNG fallback (`Leadzly-logo_without-text.png`) already exists in repo — no conversion needed

---

## OG Image

| Option | Description | Selected |
|--------|-------------|----------|
| hero-bg.jpg | Existing hero photo, works everywhere | |
| Branded SVG card | Claude-generated social card | |
| Use logo with text | User directed to use existing PNG | ✓ |

**User's choice:** Use `Leadzly-logo_with-text.png` as `og:image`
**Notes:** User confirmed PNG file exists in repo. Absolute URL: `https://leadzly.co/Leadzly-logo_with-text.png`

---

## GDPR Banner Position

| Option | Description | Selected |
|--------|-------------|----------|
| Bottom bar, full width | Fixed bar across bottom of screen | ✓ |
| Bottom-right corner card | Small floating card, less intrusive | |

**User's choice:** Bottom bar, full width (recommended)

---

## GDPR Banner Controls

| Option | Description | Selected |
|--------|-------------|----------|
| Accept only | Single Accept button | ✓ |
| Accept + Decline | Two buttons, more GDPR-robust | |
| Accept + Preferences | Full granular preference toggles | |

**User's choice:** Accept only (recommended)

---

## GA4 Gating

| Option | Description | Selected |
|--------|-------------|----------|
| Yes — gate GA4 on consent | GA4 only fires after Accept is clicked | ✓ |
| No — visual-only banner | GA4 fires regardless of consent | |

**User's choice:** Gate GA4 on consent (recommended)

---

## Privacy Policy Link

| Option | Description | Selected |
|--------|-------------|----------|
| No link | Banner text + Accept button only | ✓ |
| Link to privacy policy URL | Add link to privacy policy | |

**User's choice:** No privacy policy link (recommended) — can be added later

---

## Claude's Discretion

- Banner copy wording
- CSS implementation (should use design system custom properties)
- Animation for banner appear/dismiss
- Whether GA4 re-fires immediately on Accept or from next session

## Deferred Ideas

None — discussion stayed within phase scope.
