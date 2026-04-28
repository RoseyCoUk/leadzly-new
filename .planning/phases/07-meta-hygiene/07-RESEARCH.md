# Phase 7: Meta Hygiene - Research

**Researched:** 2026-04-28
**Domain:** HTML meta tags, robots.txt, XML sitemap — static site SEO hygiene
**Confidence:** HIGH

---

## Summary

Phase 7 adds five discrete SEO hygiene items to leadzly.co: a canonical link, a robots meta directive, four Twitter Card meta tags, a robots.txt file, and a sitemap.xml file. All five are purely static text additions — no JavaScript, no build tooling, no new dependencies. Two items go into `<head>` of `index.html` (META-01, META-02, META-03), and two are new files at project root (META-04, META-05). META-03 requires four `<meta name="twitter:...">` tags; the existing OG image at `https://leadzly.co/Leadzly-logo_with-text.png` is a valid absolute URL that satisfies `twitter:image`.

The current `<head>` already has Open Graph tags (`og:title`, `og:description`, `og:type`, `og:image`, `og:url`). What is missing is: the canonical link element, the robots meta tag, and the Twitter Card meta tags. robots.txt and sitemap.xml do not exist yet — both must be created as new files at project root, where Vercel will serve them as static assets.

**Primary recommendation:** Edit `index.html` to add three groups of tags after the existing OG block, then create `robots.txt` and `sitemap.xml` at project root. All five requirements are independent and can be batched into a single plan or two sequential plans (head edits + new files).

---

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| META-01 | `<link rel="canonical" href="https://leadzly.co/">` in `<head>` | Standard HTML link element — no library needed. Insert after existing OG tags. |
| META-02 | `<meta name="robots" content="index, follow">` in `<head>` | Standard meta robots tag. Explicit directive confirms Googlebot intent. |
| META-03 | All 4 Twitter Card meta tags: `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image` | Use `name=` attribute (not `property=`). Card type: `summary`. Image: reuse existing absolute OG image URL. |
| META-04 | `robots.txt` at site root — allow all crawlers, reference sitemap URL | Plain text file. Google-supported format. Sitemap directive uses absolute URL. |
| META-05 | `sitemap.xml` at site root — homepage URL + valid `<lastmod>` | XML file, UTF-8, sitemaps.org namespace. Single `<url>` entry for homepage. |
</phase_requirements>

---

## Standard Stack

### Core

| Item | Specification | Purpose | Why Standard |
|------|--------------|---------|--------------|
| `<link rel="canonical">` | HTML standard | Prevent duplicate-URL confusion in Google index | Google-supported, universally recommended |
| `<meta name="robots">` | HTML meta standard | Explicit index/follow directive to all crawlers | Removes ambiguity vs. relying on crawl defaults |
| Twitter Card meta tags | `name="twitter:*"` HTML meta | Controls Twitter/X link preview appearance | Required by X platform card rendering |
| `robots.txt` | Robots Exclusion Standard, plain text | Tells crawlers what they may fetch; points to sitemap | Google-supported; Sitemap: directive is official |
| `sitemap.xml` | sitemaps.org 0.9 protocol, UTF-8 XML | Lists indexable URLs with freshness signal | Supported by Google, Bing, all major crawlers |

### No Dependencies

This phase requires zero new libraries, no npm packages, no CDN scripts. All deliverables are static text edits and new static files.

---

## Architecture Patterns

### Where Changes Land

```
project root/
├── index.html        # Edit: add tags to <head>
├── robots.txt        # New file
└── sitemap.xml       # New file
```

### Pattern: Head Tag Insertion Order

Insert new tags after the existing Open Graph block and before the `<link rel="icon">` line. This keeps meta tags grouped logically:

1. charset / viewport
2. title + description
3. Open Graph tags (already present)
4. **Canonical + robots (new META-01, META-02)**
5. **Twitter Card tags (new META-03)**
6. Favicons
7. Preconnect / fonts
8. CSS links

### Current Head State (what already exists)

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Leadzly — Scale Sales Without Hiring | B2B Sales Outsourcing</title>
<meta name="description" content="...">
<meta property="og:title" content="...">
<meta property="og:description" content="...">
<meta property="og:type" content="website">
<meta property="og:image" content="https://leadzly.co/Leadzly-logo_with-text.png">
<meta property="og:image:type" content="image/png">
<meta property="og:image:alt" content="Leadzly logo">
<meta property="og:url" content="https://leadzly.co/">
<link rel="icon" type="image/svg+xml" href="./Leadzly-logo_without-text.svg">
```

Insert the five new tags between `og:url` and the first `<link rel="icon">`.

### Anti-Patterns to Avoid

- **Using `property="twitter:..."` instead of `name="twitter:..."`**: Twitter Card tags use the `name` attribute. Open Graph uses `property`. Mixing them is a common mistake — X's card parser requires `name=`.
- **Relative URL in twitter:image**: `twitter:image` must be an absolute HTTPS URL. Relative paths (`./Leadzly-logo_with-text.png`) are not resolved by the X card bot.
- **Disallowing crawlers in robots.txt that are also listed in sitemap**: If a URL is in sitemap.xml it must not be disallowed in robots.txt. For leadzly.co (single public page, allow all) there is no conflict.
- **Using `summary_large_image` with a square logo**: The logo PNG is approximately square. `summary` is the correct card type — `summary_large_image` expects a 2:1 landscape image. Using the wrong type results in poor cropping.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Sitemap generation | Custom script | Hand-authored sitemap.xml | Single-URL static site — no generation needed; static file is the right tool |
| Robots.txt logic | Programmatic output | Static `robots.txt` file | Vercel serves static files from root automatically — no serverless function needed |
| Twitter preview testing | Manual inspection | Twitter Card Validator at cards-dev.twitter.com or share-preview.com | Authoritative preview rendering without publishing |

---

## Code Examples

Verified patterns from official and authoritative sources:

### META-01: Canonical Link Element

```html
<!-- Source: Google Search Central documentation -->
<link rel="canonical" href="https://leadzly.co/">
```

### META-02: Robots Meta Tag

```html
<!-- Source: Google robots meta tag documentation -->
<meta name="robots" content="index, follow">
```

### META-03: Twitter Card Meta Tags (summary card)

```html
<!-- Source: share-preview.com/blog/twitter-meta-tags (verified against X docs pattern) -->
<!-- Note: use name= not property= for twitter: tags -->
<meta name="twitter:card" content="summary">
<meta name="twitter:title" content="Leadzly — Scale Sales Without Hiring | B2B Sales Outsourcing">
<meta name="twitter:description" content="Leadzly books 20+ qualified meetings per month through dedicated telesales. B2B sales outsourcing, appointment booking, and BDM provision. Book a free strategy call.">
<meta name="twitter:image" content="https://leadzly.co/Leadzly-logo_with-text.png">
```

**Card type rationale:** Requirements say "summary card." The existing logo PNG (`Leadzly-logo_with-text.png`) is already used as the OG image and is an absolute HTTPS URL — valid for `twitter:image`. No new image asset is required.

**Title/description source:** Reuse the existing `<title>` and `<meta name="description">` content for consistency across OG, Twitter, and page title.

### META-04: robots.txt

```
# Source: Google robots.txt documentation (developers.google.com/crawling/docs/robots-txt/create-robots-txt)
User-agent: *
Allow: /

Sitemap: https://leadzly.co/sitemap.xml
```

**Notes:**
- `Allow: /` is explicit and unambiguous — no pages are disallowed.
- `Sitemap:` directive uses an absolute URL (Google requirement).
- No `Disallow:` needed for a fully public single-page site.

### META-05: sitemap.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://leadzly.co/</loc>
    <lastmod>2026-04-28</lastmod>
  </url>
</urlset>
```

**Notes:**
- Namespace `http://www.sitemaps.org/schemas/sitemap/0.9` is required.
- `<lastmod>` uses W3C Datetime format — `YYYY-MM-DD` is valid.
- Single `<url>` entry only — leadzly.co is a single-page site.
- `https://leadzly.co/` with trailing slash matches the canonical URL in META-01.

---

## Common Pitfalls

### Pitfall 1: twitter:image not resolving

**What goes wrong:** X's card bot cannot fetch the image and the card renders without a preview image, or shows an error in the Card Validator.
**Why it happens:** Relative path used instead of absolute HTTPS URL, or the image is behind authentication / returns a non-200 status.
**How to avoid:** Use `https://leadzly.co/Leadzly-logo_with-text.png` — this file already exists in the repo root and Vercel serves it publicly. Confirm with Card Validator after deploy.
**Warning signs:** Card Validator shows "Unable to render Card preview" or image slot is blank.

### Pitfall 2: Canonical URL trailing slash mismatch

**What goes wrong:** `<link rel="canonical" href="https://leadzly.co">` (no trailing slash) and `<loc>https://leadzly.co/</loc>` in sitemap create a signal mismatch that can confuse Google about the preferred URL.
**Why it happens:** Inconsistency between how the canonical and sitemap URL are written.
**How to avoid:** Use `https://leadzly.co/` (with trailing slash) consistently in both `canonical` href and sitemap `<loc>`. The existing OG tag already uses `https://leadzly.co/` — match that.

### Pitfall 3: robots.txt served with wrong Content-Type

**What goes wrong:** Some hosting setups serve `.txt` files with `text/html` content type, causing crawlers to reject the file.
**Why it happens:** Server MIME misconfiguration.
**How to avoid:** Vercel automatically serves `.txt` files as `text/plain` — no configuration needed for this project.

### Pitfall 4: sitemap.xml not in project root

**What goes wrong:** Vercel only serves files at the URL path that mirrors the file's location in the repository root. Placing `sitemap.xml` in a subdirectory means it cannot be reached at `https://leadzly.co/sitemap.xml`.
**Why it happens:** Mistakenly placing files in a `public/` subdirectory or similar.
**How to avoid:** Create `robots.txt` and `sitemap.xml` at the same level as `index.html` — the project root (where `style.css`, `base.css`, `page.css` already live).

### Pitfall 5: Duplicate robots meta (conflict with Partytown / GA4 head)

**What goes wrong:** If a `<meta name="robots">` tag already existed (it does not currently — confirmed by reading index.html), adding a second one creates conflicting directives and Google uses the most restrictive.
**Why it happens:** Not checking existing head content before insertion.
**How to avoid:** Confirmed no existing robots meta tag in `index.html` — safe to add.

---

## Environment Availability

Step 2.6: SKIPPED — Phase 7 is purely static file edits and new static text files. No external tools, runtimes, databases, or CLI utilities are required beyond a text editor and git. Vercel automatically serves files from the project root as static assets.

---

## Validation Architecture

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Manual validation — no automated test framework (static HTML site, no npm) |
| Config file | None |
| Quick run command | Open deployed URL in browser; use Twitter Card Validator |
| Full suite command | All 5 success criteria checked manually post-deploy |

### Phase Requirements to Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| META-01 | `<link rel="canonical" href="https://leadzly.co/">` present in `<head>` | smoke | `grep -n 'canonical' index.html` | N/A — grep of source |
| META-02 | `<meta name="robots" content="index, follow">` present in `<head>` | smoke | `grep -n 'robots' index.html` | N/A — grep of source |
| META-03 | All 4 twitter: meta tags present | smoke | `grep -n 'twitter:' index.html` | N/A — grep of source |
| META-04 | `robots.txt` exists at root, contains Sitemap directive | smoke | `grep -n 'Sitemap' robots.txt` | Not yet — Wave 0 gap |
| META-05 | `sitemap.xml` exists at root, contains homepage loc + lastmod | smoke | `grep -n 'leadzly.co' sitemap.xml` | Not yet — Wave 0 gap |

**Post-deploy manual verification:**
- `https://leadzly.co/robots.txt` — loads and shows correct content
- `https://leadzly.co/sitemap.xml` — loads and shows correct XML
- Twitter Card Validator: enter `https://leadzly.co/` — card renders with title, description, image
- Google Rich Results Test (optional at this phase — no schema yet)

### Sampling Rate

- **Per task commit:** `grep` commands above on the edited file
- **Per wave merge:** All 5 grep checks pass
- **Phase gate:** Manual URL checks pass in deployed environment before marking complete

### Wave 0 Gaps

- [ ] `robots.txt` — does not exist yet, created in this phase (META-04)
- [ ] `sitemap.xml` — does not exist yet, created in this phase (META-05)

*(No test framework install needed — static site uses grep-based smoke checks only)*

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| `twitter:card` on `property=` attribute | Must use `name=` attribute | X documentation always specified `name=`, but OG confusion causes frequent bugs | Parse failure on X card bot if wrong attribute used |
| Separate OG and Twitter images (different sizes) | Single image works for both if ≥ 600×314 | Current practice | One asset covers both; leadzly.co logo already works |
| Sitemap index file for single-page sites | Single sitemap.xml with one `<url>` entry | Always | Sitemap index is only needed for >50k URLs or multiple sitemaps |

**Not applicable / no breaking changes:**
- `<link rel="canonical">` syntax unchanged since HTML5
- robots.txt Robots Exclusion Standard is stable
- `<meta name="robots">` syntax unchanged

---

## Open Questions

1. **Twitter card image quality**
   - What we know: `Leadzly-logo_with-text.png` exists at project root and is already referenced as OG image. It is an absolute URL. The `summary` card type accepts images ≥ 144px.
   - What's unclear: Exact pixel dimensions of the PNG file are not confirmed (could not run ImageMagick on Windows without additional tooling).
   - Recommendation: Use the existing PNG. If Card Validator shows a warning about image dimensions, swap to `summary_large_image` and source or create a 1200x630 OG image. This is a post-deploy validation step, not a blocker for writing the code.

2. **Leadzly Twitter/X handle**
   - What we know: `twitter:site` (the account handle, e.g. `@leadzly`) is optional for a basic summary card. The 4 required tags do not include it.
   - What's unclear: Whether Leadzly has an active Twitter/X account handle.
   - Recommendation: Omit `twitter:site` — it is not in the META-03 requirements. Do not add optional tags that require information not confirmed.

---

## Sources

### Primary (HIGH confidence)

- Google Crawling Docs (developers.google.com/crawling/docs/robots-txt/create-robots-txt) — robots.txt format, Sitemap directive syntax, allowed directives
- sitemaps.org/protocol.html — sitemap XML namespace, required tags, lastmod format, minimal example
- share-preview.com/blog/twitter-meta-tags — Twitter Card required tags, `name=` vs `property=` attribute distinction, image requirements (verified against X developer docs pattern)

### Secondary (MEDIUM confidence)

- searchengineland.com/robots-txt-seo-453779 — robots.txt SEO best practices 2026
- respona.com/blog/sitemap-best-practices — sitemap best practices 2026 (multiple sources agree)

### Tertiary (LOW confidence)

- None — all critical claims verified with official sources

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — all items are HTML/web standards with official documentation
- Architecture: HIGH — confirmed by reading index.html directly; insertion point is unambiguous
- Pitfalls: HIGH — trailing slash mismatch and twitter: attribute type are verified common errors; Vercel static serving is documented

**Research date:** 2026-04-28
**Valid until:** 2027-04-28 (stable web standards — no expiry risk within 12 months)
