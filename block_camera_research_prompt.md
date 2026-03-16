# Claude Code Prompt: Optical Zoom Block Camera Research

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
You are a research agent tasked with building a comprehensive database of small block cameras (also called zoom block cameras, board-level cameras, OEM zoom modules, or camera modules) with optical zoom capability. These are bare camera modules without gimbals — they are the camera units that get integrated into gimbals, drones, robots, and other systems. Follow this multi-phase workflow precisely. Use subagents heavily to keep context small — each subagent should do one focused task and return only structured results.

## TARGET PRODUCT CRITERIA

We are looking for block cameras / zoom camera modules that meet ALL of these requirements:
- **Optical zoom**: 10x to 40x optical zoom (not digital zoom)
- **Weight**: Under 1kg total (camera module + lens assembly)
- **Form factor**: Block camera, board-level camera, or OEM zoom module — NOT a complete gimbal assembly, NOT a handheld camera, NOT an action camera. These are bare camera units meant for integration.
- **Daylight/visible spectrum**: Standard RGB camera (exclude thermal-only and IR-only modules; dual-sensor units that include a visible+zoom camera alongside thermal are acceptable, but only if the visible camera meets the zoom and weight criteria)

Products that are close to these thresholds but slightly exceed them (e.g., 8x zoom, 1.1kg, 45x zoom) should still be captured and flagged as "near_miss" so I can evaluate them manually.

NOTE: There is no price threshold for this research. Capture price when available but do not exclude products based on price.

## PHASE 1: Generate Search Terms

Create a file `block_search_terms.json` containing 30+ search terms specifically targeting optical zoom block cameras and zoom modules. Examples:

- "block camera 10x optical zoom"
- "block camera 30x optical zoom module"
- "zoom block camera module OEM"
- "board level zoom camera 20x"
- "OEM zoom camera module drone"
- "Sony block camera FCB"
- "Tamron zoom module MP3010M"
- "mini zoom camera module 30x"
- "integrated zoom camera board"
- "compact zoom camera unit 10x 20x 30x"
- "LVDS zoom camera module"
- "IP zoom camera module MIPI"
- "HDMI output zoom block camera"
- "analog zoom camera module CVBS"
- "autofocus zoom lens camera module"
- "UAV zoom camera module lightweight"
- "surveillance zoom camera block"
- "PTZ camera module zoom core"
- "zoom camera engine OEM"
- "Sony FCB-EV7520 specifications"
- "Tamron MP3010M-EV zoom module"
- "block camera zoom ethernet output"
- "MIPI CSI zoom camera module"
- "global shutter zoom block camera"
- "starlight zoom camera module"
- "EO zoom camera core ISR"

Group them by intent: general discovery, interface-focused (MIPI, HDMI, Ethernet, analog), brand-targeted (Sony FCB, Tamron, etc.), application-targeted (drone, surveillance, robotics, ISR).

## PHASE 2: Company Discovery

For EACH search term group, launch a subagent with this instruction:

"Search the web using these terms: [terms]. For each term, use the WebSearch tool to search, then review the results. Return ONLY a JSON array of objects with {name, url, notes} for every company you find that manufactures or sells optical zoom block cameras, zoom camera modules, or OEM zoom camera units. Include companies that make both the camera module and the lens assembly as an integrated unit. Include OEMs even if they primarily sell B2B. Exclude: companies that only sell complete gimbal assemblies (unless they also sell the bare camera module separately), handheld camera brands (Canon, Nikon consumer cameras), action camera brands (GoPro, Insta360), and general electronics retailers."

Collect all results into `block_companies_raw.json`. Deduplicate by company name (fuzzy match — e.g., "Sony" and "Sony Imaging" are the same). Save to `block_companies_deduped.json`.

## PHASE 3: Company Verification

For each company (batches of 3-5), launch a subagent:

"Research these companies: [names + urls]. For each, use WebSearch and WebFetch to visit their website and determine:
1. headquarters_country (ISO 3166-1 alpha-2 code)
2. company_type: 'manufacturer' | 'OEM_supplier' | 'distributor' | 'defunct'
3. makes_zoom_block_cameras: true/false — do they make or sell block cameras / zoom modules with 10x-40x optical zoom?
4. makes_near_miss_zoom: true/false — do they make block cameras with optical zoom under 10x but at least 4x, or over 40x?
5. website: confirmed working URL
6. notes: any relevant context (e.g., 'subsidiary of X', 'also makes gimbal assemblies', 'primarily security/surveillance market')

Return ONLY a JSON array. Do not research individual products yet."

Save to `block_companies_verified.json`. Filter to companies where makes_zoom_block_cameras OR makes_near_miss_zoom is true. Save to `block_companies_final.json`.

## PHASE 4: Product Research

For EACH company in `block_companies_final.json`, launch a separate subagent:

"Research [company_name] ([website]). Find ALL block cameras, zoom camera modules, and OEM zoom units they make that have optical zoom capability in the 10x-40x range (or near-miss range of 4x-9x or 41x-60x).

IMPORTANT — How to extract specs reliably:

**Step 1: Identify products.** Use WebSearch to find the company's product listing page and identify individual product names and URLs.

**Step 2: Fetch each product page individually.** Use WebFetch on EACH INDIVIDUAL PRODUCT PAGE — never rely on listing/category pages, which typically show only names and summaries without full spec tables. When fetching, prompt specifically for weight, dimensions, video interfaces, and all technical specs.

**Step 3: Handle JS-rendered sites.** If WebFetch returns only JavaScript/CSS framework code with no readable product content, the site is client-side rendered. Do NOT retry the same URL. Instead, immediately fall back to WebSearch with the exact product name + 'specifications weight dimensions'.

**Step 4: Mandatory multi-source lookup for weight and interfaces.** Weight, dimensions, and video output interface are the most critical specs. You MUST NOT leave them as null without trying ALL of these sources:
  a. The manufacturer's own product page (via WebFetch)
  b. WebSearch for '[exact product name] weight grams specifications'
  c. WebSearch for '[exact product name] dimensions mm datasheet'
  d. Reseller/distributor sites: Alibaba, AliExpress, Mouser, Digi-Key, Phase 1 Technology, Aegis Electronic Group, imaging distributor sites
  e. PDF datasheets and technical manuals (search for '[product name] datasheet PDF')
  f. Forum posts, integration guides, and reviews that mention specs

Only mark weight_g or dimensions_mm as null after you have tried at least 3 different sources and found nothing.

For each product, extract:

- product_name: string
- model_number: string or null
- optical_zoom: string (e.g., '10x', '30x', '10x-40x continuous')
- digital_zoom_max: string or null (e.g., '12x additional')
- total_zoom_combined: string or null
- focal_length_range_mm: string or null (e.g., '4.3-129mm', '6.1-183mm')
- sensor_type: string or null (e.g., '1/2.8-inch CMOS', 'Sony STARVIS IMX335', 'Sony Exmor R')
- sensor_size: string or null (e.g., '1/2.8-inch', '1/1.8-inch')
- megapixels: number or null
- max_photo_resolution: string or null
- max_video_resolution: string or null (e.g., '4K 30fps', '1080p 60fps')
- has_night_vision: boolean (starlight/low-light capability)
- has_thermal: boolean (if dual-sensor module)
- stabilization_type: 'optical' | 'electronic' | 'none' | 'unknown' (OIS = optical image stabilization, EIS = electronic image stabilization)
- stabilization_notes: string or null (e.g., 'OIS built into lens', 'EIS via ISP', 'no stabilization')
- weight_g: number or null (total weight of camera module + lens in grams)
- dimensions_mm: string or null (e.g., '60x55x85' or 'L x W x H')
- video_output_interfaces: array of strings or null — this is critical. Capture ALL available interfaces. Common types:
  - 'HDMI' (specify version if known, e.g., 'HDMI 1.4')
  - 'Ethernet' or 'IP' (specify protocol if known, e.g., 'RTSP over Ethernet', 'ONVIF')
  - 'MIPI CSI' (specify lanes if known, e.g., 'MIPI CSI-2 4-lane')
  - 'LVDS'
  - 'analog' (specify type: 'CVBS/composite', 'HD-SDI', 'AHD', 'TVI', 'EX-SDI')
  - 'USB' (specify version, e.g., 'USB 3.0 UVC')
  - 'SDI' (specify: 'HD-SDI', '3G-SDI', '12G-SDI')
  - 'parallel digital' (specify bit width)
  - Other interfaces as found
- control_interface: array of strings or null (e.g., ['UART/TTL', 'RS-232', 'I2C', 'SPI', 'Pelco-D', 'VISCA'])
- control_protocol: string or null (e.g., 'VISCA', 'Pelco-D', 'ONVIF', 'proprietary SDK')
- power_consumption_w: number or null
- voltage_input: string or null (e.g., '5V', '12V', '3.3-5V')
- operating_temp_range: string or null (e.g., '-20 to +60C')
- ip_rating: string or null (e.g., 'none — bare module', 'IP66')
- autofocus: boolean or null
- price_usd: number or null (MSRP or typical listed price)
- price_source: string or null (where you found the price)
- availability: 'available' | 'preorder' | 'discontinued' | 'unknown'
- product_url: direct link to product page
- notable_features: array of strings (e.g., 'global shutter', 'defog', 'WDR', 'face detection', 'onboard H.265 encoding')
- meets_criteria: boolean — true if optical zoom >= 10x AND optical zoom <= 40x AND weight_g is known AND weight_g < 1000
- near_miss: boolean — true if it is close but fails one criterion (e.g., 8x zoom, 1050g, 45x zoom)
- near_miss_reason: string or null (e.g., 'zoom is only 8x', 'weight is 1200g', 'zoom is 55x exceeds 40x range')

Return ONLY a JSON array of product objects. Use null for specs you cannot find — do not guess. Exclude thermal-only modules entirely."

Save each company's products to `block_products/[company_name_slug].json`.

## PHASE 4.5: Spec Gap-Fill and Validation

After all product files are written, perform two passes:

### Pass 1: Fill missing weight, dimensions, and interfaces

Read every product JSON file. For each product where weight_g is null OR dimensions_mm is null OR video_output_interfaces is null, launch a subagent (batch up to 3 products per subagent):

"These products are missing weight, dimensions, and/or interface data. For EACH product, you MUST perform ALL of the following searches — do not stop after the first failure:

1. WebSearch: '[product_name] [model_number] specifications weight grams dimensions'
2. WebSearch: '[product_name] [model_number] datasheet PDF'
3. WebSearch: '[product_name] [model_number] video output interface HDMI MIPI ethernet'
4. WebFetch on at least 2 reseller or distributor pages found in search results
5. WebFetch on any PDF datasheet or technical manual URLs found in search results
6. WebSearch: '[company_name] [product_name] review specs integration' (integration guides often list interface details)

Products to research:
[list of {product_name, company, product_url, missing_fields}]

Return a JSON array of {product_name, weight_g, dimensions_mm, video_output_interfaces, stabilization_type, data_source_url} for each product. Only return null if you searched all 6 strategies above and found nothing. Include the URLs you checked in data_source_url so we can verify."

Update the product files with any newly found specs. Re-evaluate meets_criteria and near_miss for updated products.

### Pass 2: Validate meets_criteria and near_miss

Re-read all product files. For each product, recompute:
- meets_criteria: true only if optical_zoom >= 10x AND optical_zoom <= 40x AND weight_g is known AND weight_g < 1000
- If weight_g is null, set meets_criteria to false and near_miss to true with near_miss_reason explaining weight is unknown
- If weight_g >= 1000, set meets_criteria to false and near_miss to true with the specific reason
- If optical_zoom < 10x or optical_zoom > 40x, set meets_criteria to false and near_miss to true with the specific reason

## PHASE 5: Assembly & Export

1. Merge all product files into `block_camera_database.json` with structure:
```json
{
  "metadata": {
    "generated_date": "YYYY-MM-DD",
    "criteria": {
      "min_optical_zoom": "10x",
      "max_optical_zoom": "40x",
      "max_weight_g": 1000,
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

2. Generate `block_camera_database.csv` with one row per product, flattening company info into columns. Sort by: meets_criteria (true first), then by optical_zoom descending, then weight ascending. Key columns must include: manufacturer, model_number, optical_zoom, weight_g, dimensions_mm, stabilization_type, video_output_interfaces (semicolon-separated), sensor_type, megapixels, max_video_resolution, price_usd, meets_criteria, near_miss, near_miss_reason, product_url.

3. Generate `qualifying_block_cameras.md` — a human-readable table of ONLY products where meets_criteria is true, sorted by optical zoom descending then weight ascending. Include: model, manufacturer, country, optical zoom, weight (g), dimensions, stabilization, video interfaces, sensor, resolution, price, and a link.

4. Generate `block_camera_near_misses.md` — same format but for near_miss products, with a column explaining why they didn't qualify.

5. Generate `block_camera_research_summary.md` with:
   - How many companies were found, verified, and had qualifying products
   - List of qualifying products with key specs
   - List of near-miss products and what disqualified them
   - Breakdown by manufacturer country
   - Weight range and dimensions range of qualifying products
   - Zoom range distribution (how many at 10x, 20x, 30x, etc.)
   - Stabilization breakdown: how many have OIS, EIS, or no stabilization
   - Interface breakdown: how many have HDMI, MIPI, Ethernet, analog, etc.
   - Price range of qualifying products (where known)
   - Any notable gaps in the market
   - Companies that were excluded and why
   - Search terms that produced the best results

## CRITICAL RULES FOR EFFICIENCY

1. **Subagent isolation**: Each subagent receives ONLY the data it needs. Never pass the full database to a subagent.
2. **Batch sizing**: Company verification in groups of 3-5. Product research one company at a time (or 2-3 small companies together).
3. **File-based state**: Write results to disk after each phase. If interrupted, the next phase reads from files.
4. **No accumulation**: After a subagent returns and you save its results, do not keep raw results in context. Read from files when needed.
5. **Progressive filtering**: Each phase reduces the dataset. Only companies confirmed to make zoom block cameras get product research.
6. **Structured output only from subagents**: Subagents return JSON arrays, nothing else.
7. **Error handling**: If a subagent fails or returns malformed data, log to `block_camera_errors.log` and continue. Retry once.
8. **Price verification**: If a price seems unusually low or high, flag it in notable_features and note the source. Block camera prices vary widely by channel (direct vs distributor vs small quantity).
9. **Weight verification**: Confirm whether listed weight includes only the camera module or also mounting hardware/cables. Note discrepancies.
10. **Always fetch individual product pages**: Category/listing pages rarely contain full specs. Always navigate to or search for the individual product page to get weight, dimensions, interfaces, and stabilization info.
11. **Handle JS-rendered sites**: If WebFetch returns only JavaScript/CSS framework code without product content, the site is client-side rendered. Do NOT retry the same URL — immediately switch to WebSearch for the product specs from third-party sources, PDF datasheets, or distributor listings.
12. **Exhaust weight sources**: Weight is listed on virtually every camera module datasheet. If you don't find it on the manufacturer page, check distributor listings, PDF datasheets, and integration guides. A null weight after only checking one source is unacceptable.
13. **Exhaust interface sources**: Video output interface is critical for integration decisions. If not on the product page, check datasheets, application notes, and integration guides. Many block cameras support multiple interfaces via different board configurations — capture all variants.
14. **Multi-source minimum**: For every product, you must attempt to extract weight, dimensions, and interfaces from at least 3 independent sources before marking any as null.
15. **No blanket WebFetch domain restrictions**: The `.claude/settings.local.json` file grants unrestricted WebFetch access. Do not hesitate to fetch any URL from any domain.
16. **Distinguish block cameras from gimbal assemblies**: Some manufacturers sell both the bare camera module and a complete gimbal assembly using that module. Only capture the bare camera module here — the gimbal version belongs in the gimbal research, not this one.

Start now with Phase 1.
```

## Usage

Paste the prompt above (everything inside the triple-backtick code fence) directly into Claude Code.

## Expected Output Files

| File | Description |
|---|---|
| `block_search_terms.json` | Categorized search terms |
| `block_companies_raw.json` | All companies found (with dupes) |
| `block_companies_deduped.json` | Deduplicated company list |
| `block_companies_verified.json` | Companies with HQ, type, zoom validation |
| `block_companies_final.json` | Filtered to confirmed zoom block camera makers |
| `block_products/*.json` | Per-company product specs |
| `block_camera_database.json` | Complete merged database |
| `block_camera_database.csv` | Flat CSV sorted by qualification status |
| `qualifying_block_cameras.md` | Products meeting all criteria |
| `block_camera_near_misses.md` | Products that almost qualify |
| `block_camera_research_summary.md` | Full analysis and observations |
| `block_camera_errors.log` | Any failures encountered |

## Customization Notes

- To adjust the zoom range, edit the criteria in the TARGET PRODUCT CRITERIA section and the `meets_criteria`/`near_miss` logic in Phase 4
- To include thermal modules, change the Phase 4 instruction to capture thermal sensor specs rather than excluding thermal-only products
- Known brands likely to appear: Sony (FCB series), Tamron, Dahua, Hikvision, Hanwha/Samsung Techwin, Videology, InfiRay, Sunny Optical, and various Chinese OEMs (many gimbal companies from the gimbal research also sell bare zoom modules)
- The stabilization_type and video_output_interfaces fields are critical for integration planning — ensure subagents prioritize extracting these
