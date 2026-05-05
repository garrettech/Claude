# Les Néréides Australia — Ecommerce Automation Opportunities
**Prepared:** May 2026  
**Data Sources Available:** Lightspeed Retail X (POS + inventory), Google Analytics 4, Google Search Console, Shopify  
**Brands in scope:** Les Néréides Australia (primary), extensible to Love Persimmon and Roberto Verino ANZ

---

## Overview

The following automation opportunities leverage live data from Lightspeed (in-store and online sales, inventory), GA4 (web traffic and behaviour), and GSC (organic search performance) to improve the Les Néréides website over time. Each initiative is ranked by implementation priority and includes technical requirements for an AI agent or developer to build.

---

## 1. Dynamic Best Sellers Shelf
**Priority: High | Effort: Medium**

### What It Does
Automatically updates the homepage product shelf each week to display the top 10 best-selling products based on real Lightspeed sales data from the previous Mon–Sun period.

### How It Works
1. Every Monday, a script calls the Lightspeed API and retrieves the top 10 products by revenue for the past 7 days
2. The script matches each Lightspeed product to its Shopify counterpart via SKU (SKUs are consistent across both platforms)
3. The script updates the designated Shopify surface (manual collection or metaobject) with the matched Shopify product IDs in ranked order
4. The homepage shelf reads from this source and renders the updated products automatically

### What's Required
- **Lightspeed API access:** Already available via Garrettech MCP server (`lightspeed_top_products` tool, brand: `les_nereides`)
- **Shopify API access:** Custom App with `read_products` and `write_products` / `write_collections` scopes
- **Shopify collection type:** Must be set to Manual (not Automated/rule-based), OR metaobject support confirmed with Gecko theme
- **Scheduler:** Cron job, GitHub Actions, Make, or Zapier to trigger every Monday
- **SKU matching logic:** Script to cross-reference Lightspeed product SKUs against Shopify product variants

### Notes
- Confirm with Gecko theme support whether the Tabs Collection element supports metaobject references as a product source
- If not, use a Manual Shopify Collection as the data target instead
- Consider extending to Love Persimmon and Roberto Verino once validated on Les Néréides

---

## 2. Weekly Performance Dashboard
**Priority: High | Effort: Medium**

### What It Does
Delivers an automated Monday morning digest combining Lightspeed revenue by store, GA4 top pages and traffic, and GSC top queries — removing the need to manually open multiple platforms for weekly reporting.

### How It Works
1. Every Monday morning, a script pulls the previous week's data from three sources:
   - **Lightspeed:** Revenue, transactions, and units sold by outlet
   - **GA4:** Top 10 pages by sessions, bounce rate, and conversion events
   - **GSC:** Top 10 queries by impressions and clicks, plus CTR
2. The data is compiled into a structured report and delivered via email or Slack
3. Optionally rendered as a live dashboard (e.g. a web app or Google Sheet) that updates automatically

### What's Required
- **Lightspeed API:** `lightspeed_sales_summary` and `lightspeed_top_products` tools
- **GA4 API:** Windsor.ai connector (already connected) or direct GA4 Data API credentials
- **GSC API:** Windsor.ai connector (already connected) or direct Search Console API credentials
- **Delivery method:** Email (SMTP or Gmail API), Slack webhook, or a hosted dashboard
- **Scheduler:** Weekly cron job or automation platform trigger

### Notes
- Windsor.ai is already connected and provides GA4 + GSC data — confirm field availability for weekly aggregation
- Report should include: store revenue vs. target, top web pages, top search queries, week-on-week comparison
- Forooz to be included as a recipient

---

## 3. GA4 × Lightspeed Conversion Gap Report
**Priority: High | Effort: High**

### What It Does
Cross-references GA4 product page traffic with Lightspeed online sales data to identify products that receive high web traffic but generate low sales — signalling pages that need copy, imagery, or pricing improvements.

### How It Works
1. GA4 provides a ranked list of product pages by sessions and add-to-cart rate
2. Lightspeed (or Shopify) provides actual units sold and revenue per product for the same period
3. The script calculates a **conversion gap score** for each product: high views + low sales = high priority for page improvement
4. Output is a weekly or monthly prioritised list of product pages flagged for optimisation

### What's Required
- **GA4 API:** Product page URLs, sessions, add-to-cart events (requires GA4 ecommerce tracking to be properly configured on Shopify)
- **Lightspeed or Shopify API:** Units sold and revenue per product SKU
- **SKU-to-URL mapping:** A reference table linking product SKUs to their Shopify product page URLs
- **Output format:** CSV, Google Sheet, or dashboard view
- **Scheduler:** Weekly or monthly trigger

### Notes
- GA4 ecommerce tracking must be confirmed as active and correctly tagging product views and add-to-cart events on the Shopify store
- This is the highest-value strategic report available — prioritise once data pipeline is confirmed reliable

---

## 4. Low Stock Urgency Signals
**Priority: High | Effort: Low–Medium**

### What It Does
Automatically displays a "Only X left" badge on product pages when real-time Lightspeed inventory drops below a defined threshold, creating genuine urgency without artificial scarcity tactics.

### How It Works
1. A script periodically checks Lightspeed inventory levels for all active products (e.g. every hour or every few hours)
2. When a product's total stock across all outlets (or online-only stock) falls below a threshold (e.g. 3 units), a flag is written to the corresponding Shopify product's metafield
3. The Shopify theme reads the metafield and conditionally renders a low stock badge on the product page

### What's Required
- **Lightspeed API:** `lightspeed_get_inventory` tool (brand: `les_nereides`)
- **Shopify API:** `write_products` scope to update product metafields
- **Shopify theme customisation:** A small Liquid code snippet in the product template to read the metafield and render the badge conditionally
- **Threshold configuration:** Define the stock level that triggers the badge (recommended: 3 units)
- **Scheduler:** Runs every 1–4 hours depending on sales velocity

### Notes
- Decide whether threshold is based on total stock across all outlets or online-only stock
- Badge should disappear automatically once stock is replenished above the threshold
- Consider a separate "Back Soon" or "Out of Stock" state for zero inventory

---

## 5. Trending This Week Shelf
**Priority: Medium | Effort: Medium**

### What It Does
Surfaces products that have seen a sudden spike in sales velocity over the past 7 days relative to their recent baseline — separate from the overall best sellers list. Catches emerging trends early and gives them homepage exposure before they sell out.

### How It Works
1. The script calculates a **velocity score** for each product: units sold in the last 7 days divided by average weekly units sold over the past 4–8 weeks
2. Products with a velocity score significantly above their baseline (e.g. 2x or more) are flagged as "trending"
3. Top 5–10 trending products are pushed to a dedicated Shopify collection or metaobject powering a secondary homepage shelf

### What's Required
- **Lightspeed API:** `lightspeed_top_products` called across multiple date ranges to establish baseline and current week data
- **Shopify API:** Same as Best Sellers shelf setup
- **Baseline calculation:** Requires 4–8 weeks of historical Lightspeed data to establish meaningful averages
- **Scheduler:** Weekly Monday trigger, same as Best Sellers

### Notes
- This shelf works best once at least 6–8 weeks of weekly data has been collected and stored
- Consider storing weekly snapshots in a lightweight database or Google Sheet to build the baseline over time
- Trending products should be excluded from the Best Sellers shelf to avoid duplication

---

## 6. Store vs. Online Demand Signal Analysis
**Priority: Medium | Effort: High**

### What It Does
Compares in-store sales performance (Lightspeed) against online traffic (GA4/GSC) for each product to identify two key opportunities:
- **Underdiscovered online:** Strong in-store seller with low online traffic → candidate for featured placement or paid promotion
- **Undertrained in-store:** High online traffic with low in-store sales → staff may not be recommending it

### How It Works
1. Lightspeed provides units sold per product across all physical outlets for a given period
2. GA4 provides sessions and engagement per product page URL for the same period
3. The script normalises both datasets and calculates a store/online demand ratio per product
4. Output is a bi-weekly or monthly report with two lists: online-underserved products and in-store-underserved products

### What's Required
- **Lightspeed API:** Sales by product across all outlets
- **GA4 API:** Product page sessions and engagement metrics
- **SKU-to-URL mapping:** Reference table linking Lightspeed SKUs to Shopify product URLs
- **Normalisation logic:** Adjust for the fact that in-store and online have different customer volumes
- **Output format:** CSV or Google Sheet with flagged products and recommended actions

### Notes
- This is a uniquely powerful report that pure online retailers cannot produce — leverage it
- Bi-weekly cadence recommended to balance signal quality with actionability
- In-store insights should be shared with store managers as part of weekly briefing

---

## 7. SEO-Driven Collection & Product Page Optimisation Queue
**Priority: Medium | Effort: Medium**

### What It Does
Uses GSC data to automatically flag underperforming pages — those with high impressions but low click-through rates — and adds them to a prioritised SEO task queue for copy and metadata improvements.

### How It Works
1. Weekly, the script pulls GSC data for all indexed Les Néréides pages
2. Pages are scored by an **opportunity score**: high impressions × low CTR = high priority
3. The output is a ranked list of pages that need title tag, meta description, or on-page content improvements
4. Optionally, the AI agent can draft suggested improvements for each flagged page automatically

### What's Required
- **GSC API:** Windsor.ai connector (already connected) — needs page-level impression, click, and CTR data
- **Page-to-template mapping:** Identifies whether each URL is a collection page, product page, or blog post so the right optimisation template is applied
- **Output format:** Google Sheet or task management tool (e.g. Asana, Notion) with page URL, current CTR, impressions, and suggested action
- **Optional:** Anthropic API to auto-generate improved title tags and meta descriptions for flagged pages

### Notes
- GSC data has a 2–3 day lag — account for this when scheduling
- Focus first on collection pages as they have the highest SEO leverage for Les Néréides
- Pairs well with the existing multi-brand SEO workflow already in place

---

## 8. Automated Dead Stock Alerts
**Priority: Medium | Effort: Low**

### What It Does
Identifies products that haven't sold in 30+ days across both online and in-store channels and flags them automatically for action — markdown, bundle, or EDM promotion — before they become a deeper inventory problem.

### How It Works
1. Weekly, the script checks Lightspeed sales history for all active products
2. Any product with zero sales in the past 30 days (configurable) is flagged as dead stock
3. The script generates an alert report listing the product name, SKU, current stock level, last sale date, and recommended action
4. Optionally, flagged products are automatically added to a Shopify Sale collection or trigger a Klaviyo flow

### What's Required
- **Lightspeed API:** Product sales history and current inventory levels
- **Configurable threshold:** Default 30 days, adjustable per product category (e.g. seasonal items may warrant a shorter window)
- **Output format:** Email alert, Slack message, or Google Sheet
- **Optional Klaviyo integration:** Trigger a targeted EDM for flagged products to engaged subscribers

### Notes
- Exclude new arrivals (products added in the last 30 days) from the dead stock flag
- Consider different thresholds by category — jewellery vs. clothing vs. accessories may have naturally different sell-through rates
- This alert should go to both the ecommerce manager and store managers

---

## 9. Search Query → Product & Content Gap Analysis
**Priority: Low–Medium | Effort: Medium**

### What It Does
Uses GSC search query data to identify terms driving impressions for product categories Les Néréides carries but doesn't rank well for — revealing unmet demand that can be filled with new product pages, collection pages, or blog content.

### How It Works
1. Monthly, the script pulls all GSC queries with >100 impressions and <2% CTR
2. Queries are categorised by product type (e.g. "enamel earrings Sydney," "French jewellery Australia")
3. The script checks whether a matching collection page or product exists on the Shopify store
4. Gaps — queries with demand but no strong matching page — are flagged as content or collection opportunities

### What's Required
- **GSC API:** Query-level impression, click, CTR, and average position data
- **Shopify API:** List of active collection and product page URLs for gap matching
- **NLP or keyword matching logic:** To map GSC queries to existing page topics
- **Output format:** Monthly report with flagged queries, estimated traffic opportunity, and recommended page type to create

### Notes
- Monthly cadence is sufficient as GSC query trends move slowly
- This feeds directly into the SEO content calendar and new collection page planning
- High-value queries with no matching page should be escalated to the SEO workflow immediately

---

## Technical Infrastructure Summary

| Component | Tool / Service | Status |
|---|---|---|
| Lightspeed data | Garrettech MCP Server | ✅ Connected |
| GA4 data | Windsor.ai | ✅ Connected |
| GSC data | Windsor.ai | ✅ Connected |
| Shopify API | Custom App (read/write) | ⚠️ To be set up |
| Klaviyo integration | Garrettech MCP Server | ✅ Connected |
| Scheduler / automation | GitHub Actions / Make / Zapier | ⚠️ To be decided |
| Data storage (baselines) | Google Sheet / lightweight DB | ⚠️ To be set up |
| Delivery (alerts/reports) | Gmail / Slack | ✅ Connected |

---

## Recommended Build Order

1. **Dynamic Best Sellers Shelf** — highest visibility, confirmed data pipeline, clear business value
2. **Weekly Performance Dashboard** — immediate operational value for Jun and Forooz
3. **Low Stock Urgency Signals** — quick win for conversion rate, low build complexity
4. **GA4 × Lightspeed Conversion Gap Report** — high strategic value once GA4 ecommerce tracking is confirmed
5. **SEO Optimisation Queue** — builds on existing SEO workflow infrastructure
6. **Automated Dead Stock Alerts** — reduces inventory risk over time
7. **Trending This Week Shelf** — requires 6–8 weeks of baseline data before it's meaningful
8. **Store vs. Online Demand Analysis** — most complex, highest unique strategic value
9. **Search Query → Content Gap Analysis** — monthly cadence, lower urgency

---

*Document prepared by Claude (Anthropic) acting as Head of Marketing & Insights for Garrettech / Les Néréides Australia. To be used as a briefing document for an AI agent or developer when building out these automations.*
