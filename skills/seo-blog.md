# Workflow 1 — Blog Article

## Inputs required before drafting
1. Primary keyword (or apply keyword selection rule)
2. Title — does the user have one, or generate it?
3. Optional secondary keywords

---

## Steps

**Step 1 — Windsor.ai research**
Query GSC for the primary keyword. Identify:
- High impressions + low CTR → title and meta opportunities
- Position 5–20 + high impressions → strong integration candidates
- Related queries not yet targeted → content gap opportunities

Query GA4 for existing content on related topics → cannibalisation check. If cannibalisation detected, flag before proceeding.

**Step 2 — Draft**
Produce in one response:
- SEO title tag (plain text, ≤60 chars, primary keyword included)
- Meta description (plain text, ≤160 chars, primary keyword + CTA)
- Suggested URL slug (lowercase, hyphenated)
- Full HTML article body:
  - Opening `<p>` with hook and primary keyword in first sentence
  - `<h2>`/`<h3>` structure reflecting search intent
  - GSC-informed supporting terms integrated naturally
  - `<ul>` or `<ol>` lists where useful
  - 2–3 internal link suggestions: `<a href="[INTERNAL LINK: suggested page]">anchor text</a>`
  - 1–2 external authority links formatted the same way
  - Image placeholders: `<!-- IMAGE: [recommended alt text] -->`
  - Closing `<p>` with conclusion and CTA

**Step 2b — Image URLs**
After delivering the HTML draft, list every image placeholder:
```
Image 1: [alt text]
Image 2: [alt text]
```
Ask: "Please provide a Shopify CDN URL for each image. I'll insert them before we finalise."
Wait for URLs or explicit skip before proceeding.

**Step 2c — Image insertion**
Replace each `<!-- IMAGE -->` placeholder with:
`<img src="[URL]" alt="[alt text]" style="max-width:100%;" />`
Deliver updated HTML in full.

**Step 3 — FAQs**
Produce 5 unique Q&As in plain text below the HTML block.

**Step 4 — Helpful Content review**
Apply Helpful Content checklist (see quality frameworks in system prompt). Revise any failing sections.

**Step 5 — Search Quality scoring**
Apply Search Quality scoring (see quality frameworks). Identify weakest dimension and revise.

**Step 6 — Final delivery**
Return: final HTML article + FAQ block + dimension scores + overall rating out of 10 + Helpful Content compliance note. Ask whether edits or another task are needed.
