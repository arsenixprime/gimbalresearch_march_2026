# Claude Code Prompt: Optical Zoom Drone Gimbal Research

## Prerequisites

Before running this prompt, ensure WebSearch and WebFetch are pre-approved for **all domains** in your project settings. Create or update `.claude/settings.local.json`:

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "WebFetch"
    ]
  }
}
```

**Do NOT use domain-specific rules** like `WebFetch(domain:example.com)` — the research discovers new manufacturer websites dynamically, so blanket access is required.

This is **required** — without it, subagents will be denied web access and the research will fail silently.

## Prompt

```
You are a research agent tasked with building a comprehensive database of drone camera gimbals that have integrated optical zoom capability. Follow this multi-phase workflow precisely. Use subagents heavily to keep context small — each subagent should do one focused task and return only structured results.

## TARGET PRODUCT CRITERIA

We are looking for drone-mountable camera gimbals that meet ALL of these requirements:
- **Optical zoom**: 10x or greater optical zoom (not digital zoom)
- **Weight**: Under 1kg total (gimbal + integrated camera)
- **Price**: Under $10,000 USD
- **Daylight/visible spectrum**: Standard RGB camera (exclude thermal-only and IR-only payloads; dual-sensor units that include a visible+zoom camera alongside thermal are acceptable, but only if the visible camera meets the zoom and weight criteria)
- **Drone-mountable**: Designed to be mounted on a drone (not handheld-only or ground-vehicle-only)

Products that are close to these thresholds but slightly exceed them (e.g., 8x zoom, 1.1kg, $11k) should still be captured and flagged as "near_miss" so I can evaluate them manually.

## PHASE 1: Generate Search Terms

Create a file `search_terms.json` containing 25+ search terms specifically targeting optical zoom drone gimbals. Examples:

- "drone gimbal optical zoom 10x"
- "drone camera 30x optical zoom payload"
- "UAV zoom camera gimbal"
- "drone zoom lens gimbal lightweight"
- "small drone zoom camera payload"
- "optical zoom drone camera under 1kg"
- "drone ISR zoom camera"
- "surveillance drone zoom gimbal"
- "drone gimbal zoom camera enterprise"
- "mapping drone optical zoom"
- "zoom payload DJI Matrice"
- "zoom payload Freefly"
- "zoom gimbal Siyi"
- "zoom gimbal Gremsy"
- "zoom gimbal Viewpro"
- "drone EO zoom camera"
- "multi-rotor zoom camera payload"
- "10x 20x 30x drone camera"
- "starlight zoom drone camera"
- "drone inspection zoom camera"
- "small UAV zoom payload"

Group them by intent: general discovery, spec-focused, brand-targeted, application-targeted (inspection, surveillance, mapping).

## PHASE 2: Company Discovery

For EACH search term group, launch a subagent with this instruction:

"Search the web using these terms: [terms]. For each term, use the WebSearch tool to search, then review the results. Return ONLY a JSON array of objects with {name, url, notes} for every company you find that manufactures drone-mountable camera gimbals with optical zoom capability. Exclude: retailers/resellers, review-only sites, thermal-only manufacturers, handheld gimbal companies (like Zhiyun for handheld stabilizers), companies that only make drones but not payloads. Include OEMs even if they primarily sell B2B."

Collect all results into `companies_raw.json`. Deduplicate by company name (fuzzy match — e.g., "SIYI" and "SIYI Technology" are the same). Save to `companies_deduped.json`.

## PHASE 3: Company Verification

For each company (batches of 3-5), launch a subagent:

"Research these companies: [names + urls]. For each, use WebSearch and WebFetch to visit their website and determine:
1. headquarters_country (ISO 3166-1 alpha-2 code)
2. company_type: 'manufacturer' | 'integrator' | 'OEM_supplier' | 'defunct'
3. makes_zoom_gimbals: true/false — do they make drone gimbals with optical zoom (10x+)?
4. makes_near_miss_zoom: true/false — do they make drone gimbals with optical zoom under 10x but at least 4x?
5. website: confirmed working URL
6. notes: any relevant context (e.g., 'subsidiary of X', 'Chinese OEM brand', 'primarily military')

Return ONLY a JSON array. Do not research individual products yet."

Save to `companies_verified.json`. Filter to companies where makes_zoom_gimbals OR makes_near_miss_zoom is true. Save to `companies_final.json`.

## PHASE 4: Product Research

For EACH company in `companies_final.json`, launch a separate subagent:

"Research [company_name] ([website]). Find ALL drone camera gimbals they make that have optical zoom capability.

IMPORTANT — How to extract specs reliably:

**Step 1: Identify products.** Use WebSearch to find the company's product listing page and identify individual product names and URLs.

**Step 2: Fetch each product page individually.** Use WebFetch on EACH INDIVIDUAL PRODUCT PAGE — never rely on listing/category pages, which typically show only names and summaries without full spec tables. When fetching, prompt specifically for weight, price, dimensions, and all technical specs.

**Step 3: Handle JS-rendered sites.** If WebFetch returns only JavaScript/CSS framework code with no readable product content, the site is client-side rendered. Do NOT retry the same URL. Instead, immediately fall back to WebSearch with the exact product name + 'specifications weight price'.

**Step 4: Mandatory multi-source lookup for weight and price.** Weight and price are the two most critical specs. You MUST NOT leave them as null without trying ALL of these sources:
  a. The manufacturer's own product page (via WebFetch)
  b. WebSearch for '[exact product name] weight grams specifications'
  c. WebSearch for '[exact product name] price USD buy'
  d. Reseller/distributor sites: Amazon, AliExpress, DrUAV (druav.com), RCDrone (rcdrone.top), MotioNew (motionew.com), WorldDroneMarket (worldronemarket.com), UAVGarage (uavgarage.com), ThanksBuyer (thanksbuyer.com)
  e. Aggregator sites: AeroExpo (aeroexpo.online), DroneMajor (dronemajor.net)
  f. PDF datasheets and user manuals (search for '[product name] datasheet PDF' or '[product name] user manual')
  g. Forum posts and reviews that mention specs

Only mark weight_g or price_usd as null after you have tried at least 3 different sources and found nothing.

**Step 5: Cross-check weights.** When you find a weight, verify it makes sense physically. A 10x zoom 3-axis gimbal typically weighs 350-500g. A 30x zoom 3-axis gimbal typically weighs 600-900g. A dual-sensor (EO+thermal) gimbal with 30x zoom typically weighs 700-1500g. If a weight seems implausible, search for a second source to confirm.

For each product, extract:

- product_name: string
- model_number: string or null
- gimbal_axes: 2 | 3
- optical_zoom: string (e.g., '10x', '30x', '10x-30x continuous')
- digital_zoom_max: string or null (e.g., '12x additional')
- total_zoom_combined: string or null
- sensor_type: string or null (e.g., '1/2.8-inch CMOS', 'Sony STARVIS IMX')
- sensor_size: string or null (e.g., '1/2.8-inch', '1/1.8-inch')
- megapixels: number or null
- max_photo_resolution: string or null
- max_video_resolution: string or null (e.g., '4K 30fps', '1080p 60fps')
- has_night_vision: boolean (starlight/low-light capability)
- has_thermal: boolean (if dual-sensor, note this but we care about the visible channel)
- weight_g: number or null (total weight of gimbal + camera unit in grams)
- dimensions_mm: string or null (e.g., '120x110x130')
- power_consumption_w: number or null
- voltage_input: string or null (e.g., '12-25.2V')
- stabilization_range_pitch: string or null (e.g., '-120° to +30°')
- stabilization_range_yaw: string or null
- stabilization_range_roll: string or null
- output_interface: array of strings or null (e.g., ['HDMI', 'IP/Ethernet', 'USB'])
- control_protocol: array of strings or null (e.g., ['UART/TTL', 'S.Bus', 'MAVLink', 'SDK'])
- compatible_drones: array of strings or null (e.g., ['DJI M300', 'DJI M350', 'custom builds'])
- ip_rating: string or null (e.g., 'IP44', 'IP67')
- price_usd: number or null (MSRP or typical listed price)
- price_source: string or null (where you found the price — e.g., 'manufacturer website', 'Amazon', 'distributor listing')
- availability: 'available' | 'preorder' | 'discontinued' | 'unknown'
- product_url: direct link to product page
- notable_features: array of strings
- meets_criteria: boolean — true if optical zoom >= 10x AND weight < 1000g AND price < 10000
- near_miss: boolean — true if it is close but fails one criterion (e.g., 8x zoom, 1050g, $11k)
- near_miss_reason: string or null (e.g., 'zoom is only 8x', 'weight is 1200g')

Return ONLY a JSON array of product objects. Use null for specs you cannot find — do not guess. Exclude thermal-only products entirely."

Save each company's products to `products/[company_name_slug].json`.

## PHASE 4.5: Spec Gap-Fill and Validation

After all product files are written, perform two passes:

### Pass 1: Fill missing weight and price

Read every product JSON file. For each product where weight_g is null OR price_usd is null, launch a subagent (batch up to 3 products per subagent):

"These products are missing weight and/or price data. For EACH product, you MUST perform ALL of the following searches — do not stop after the first failure:

1. WebSearch: '[product_name] [model_number] specifications weight grams'
2. WebSearch: '[product_name] [model_number] price USD buy'
3. WebSearch: '[product_name] [model_number] datasheet PDF'
4. WebFetch on at least 2 reseller pages found in search results (e.g., DrUAV, RCDrone, MotioNew, Amazon, AliExpress, WorldDroneMarket, ThanksBuyer)
5. WebFetch on any PDF datasheet or user manual URLs found in search results
6. WebSearch: '[company_name] [product_name] review specs' (review sites often list specs the manufacturer page hides)

Products to research:
[list of {product_name, company, product_url, missing_fields}]

Return a JSON array of {product_name, weight_g, price_usd, price_source, data_source_url} for each product. Only return null if you searched all 6 strategies above and found nothing. Include the URLs you checked in data_source_url so we can verify."

Update the product files with any newly found specs. Re-evaluate meets_criteria and near_miss for updated products.

### Pass 2: Validate meets_criteria and near_miss

Re-read all product files. For each product, recompute:
- meets_criteria: true only if optical_zoom >= 10x AND weight_g is known AND weight_g < 1000 AND price_usd is known AND price_usd < 10000
- If weight_g or price_usd is null, set meets_criteria to false and near_miss to true with near_miss_reason explaining which spec is unknown
- If weight_g >= 1000 or price_usd >= 10000, set meets_criteria to false and near_miss to true with the specific reason

## PHASE 5: Assembly & Export

1. Merge all product files into `gimbal_database.json` with structure:
```json
{
  "metadata": {
    "generated_date": "YYYY-MM-DD",
    "criteria": {
      "min_optical_zoom": "10x",
      "max_weight_g": 1000,
      "max_price_usd": 10000,
      "spectrum": "visible/daylight"
    },
    "total_companies": N,
    "total_products": N,
    "products_meeting_criteria": N,
    "products_near_miss": N
  },
  "companies": [
    {
      "name": "...",
      "headquarters_country": "...",
      "website": "...",
      "company_type": "...",
      "products": [...]
    }
  ]
}
```

2. Generate `gimbal_database.csv` with one row per product, flattening company info into columns. Sort by: meets_criteria (true first), then by optical_zoom descending, then weight ascending.

3. Generate `qualifying_products.md` — a human-readable table of ONLY products where meets_criteria is true, sorted by optical zoom descending. Include: product name, company, country, optical zoom, weight, video resolution, price, and a link.

4. Generate `near_misses.md` — same format but for near_miss products, with a column explaining why they didn't qualify.

5. Generate `research_summary.md` with:
   - How many companies were found, verified, and had qualifying products
   - List of qualifying products with key specs
   - List of near-miss products and what disqualified them
   - Breakdown by manufacturer country
   - Price range and weight range of qualifying products
   - Zoom range distribution (how many at 10x, 20x, 30x, etc.)
   - Any notable gaps in the market
   - Companies that were excluded and why
   - Search terms that produced the best results

## CRITICAL RULES FOR EFFICIENCY

1. **Subagent isolation**: Each subagent receives ONLY the data it needs. Never pass the full database to a subagent.
2. **Batch sizing**: Company verification in groups of 3-5. Product research one company at a time.
3. **File-based state**: Write results to disk after each phase. If interrupted, the next phase reads from files.
4. **No accumulation**: After a subagent returns and you save its results, do not keep raw results in context. Read from files when needed.
5. **Progressive filtering**: Each phase reduces the dataset. Only companies confirmed to make zoom gimbals get product research.
6. **Structured output only from subagents**: Subagents return JSON arrays, nothing else.
7. **Error handling**: If a subagent fails or returns malformed data, log to `errors.log` and continue. Retry once.
8. **Price verification**: If a price seems unusually low or high, flag it in notable_features and note the source. Many of these products are sold B2B and prices vary widely.
9. **Weight verification**: Confirm whether listed weight includes the gimbal, camera, and any required mounting hardware. Note discrepancies.
10. **Always fetch individual product pages**: Category/listing pages rarely contain full specs. Always navigate to or search for the individual product page to get weight, dimensions, and pricing.
11. **Handle JS-rendered sites**: If WebFetch returns only JavaScript/CSS framework code without product content, the site is client-side rendered. Do NOT retry the same URL — immediately switch to WebSearch for the product specs from third-party sources, PDF datasheets, or reseller listings.
12. **Exhaust price sources**: If the manufacturer says "contact for pricing", you MUST search resellers (Amazon, AliExpress, DrUAV, RCDrone, MotioNew, UAVGarage, ThanksBuyer, WorldDroneMarket) before marking price as null.
13. **Exhaust weight sources**: Weight is listed on virtually every gimbal product page or datasheet. If you don't find it on the manufacturer page, check reseller listings, PDF manuals, and review articles. A null weight after only checking one source is unacceptable.
14. **Multi-source minimum**: For every product, you must attempt to extract weight and price from at least 3 independent sources before marking either as null. Log which sources were checked in the product's notable_features array (e.g., "weight not found after checking: manufacturer page, DrUAV, Amazon, PDF datasheet").
15. **No blanket WebFetch domain restrictions**: The `.claude/settings.local.json` file grants unrestricted WebFetch access. Do not hesitate to fetch any URL from any domain — resellers, aggregators, forums, PDF hosts, etc.

Start now with Phase 1.
```

## Usage

Paste the prompt above (everything inside the triple-backtick code fence) directly into Claude Code.

## Expected Output Files

| File | Description |
|---|---|
| `search_terms.json` | Categorized search terms |
| `companies_raw.json` | All companies found (with dupes) |
| `companies_deduped.json` | Deduplicated company list |
| `companies_verified.json` | Companies with HQ, type, zoom validation |
| `companies_final.json` | Filtered to confirmed zoom gimbal makers |
| `products/*.json` | Per-company product specs |
| `gimbal_database.json` | Complete merged database |
| `gimbal_database.csv` | Flat CSV sorted by qualification status |
| `qualifying_products.md` | Products meeting all criteria |
| `near_misses.md` | Products that almost qualify |
| `research_summary.md` | Full analysis and observations |
| `errors.log` | Any failures encountered |

## Customization Notes

- To adjust thresholds, edit the criteria in the TARGET PRODUCT CRITERIA section and the `meets_criteria`/`near_miss` logic in Phase 4
- To add thermal as a secondary interest later, change the Phase 4 instruction to include thermal sensor specs rather than excluding thermal-only products
- To focus on specific drone platforms, add a compatible_drones filter in Phase 5
- Known brands likely to appear: SIYI, Viewpro, Topotek, DJI (Zenmuse series), Foxtech, EVO Gimbals, Gremsy, Controp, Nextbase
