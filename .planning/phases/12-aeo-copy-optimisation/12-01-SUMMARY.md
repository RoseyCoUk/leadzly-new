---
plan: 12-01
phase: 12-aeo-copy-optimisation
status: complete
completed: 2026-04-29
commit: 5382c90

key-files:
  modified:
    - index.html
---

## What Was Built

Restructured all 7 FAQ answers in index.html for AEO eligibility (AEO-01):

1. Replaced every pronoun-led FAQ answer ("We typically onboard...", "Our 30-day guarantee...") with a named-entity-first sentence that begins with "Leadzly" or a specific definition.
2. Each rewritten answer is now self-contained — readable and meaningful without seeing the question, making them extractable by AI systems (ChatGPT, Perplexity, Google AI Overviews).
3. Updated all 7 `acceptedAnswer.text` values in the FAQPage JSON-LD block to match the new visible text exactly, using plain Unicode characters (no HTML entities in JSON strings).

## Self-Check: PASSED

- ✓ All 7 FAQ HTML paragraphs start with "Leadzly" or a noun-first definition
- ✓ FAQPage JSON-LD text values match visible HTML (Unicode-normalised)
- ✓ No HTML entities (&rsquo;, &ndash;, &mdash;) in JSON-LD block
- ✓ Old pronoun-led phrases gone from FAQ section
- ✓ page.css version remains at ?v=14
- ✓ Only index.html modified

## Deviations

None.
