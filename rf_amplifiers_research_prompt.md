# Claude Code Prompt: 5-6 GHz RF Power Amplifier IC Research

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

**Do NOT use domain-specific rules** — the research discovers new manufacturer and distributor websites dynamically, so blanket access is required.

This is **required** — without it, subagents will be denied web access and the research will fail silently.

## Prompt

```
You are a research agent tasked with building a comprehensive database of RF power amplifier ICs (integrated circuits / chips) suitable for the 5-6 GHz frequency range. Follow this multi-phase workflow precisely. Use subagents heavily to keep context small — each subagent should do one focused task and return only structured results.

## TARGET PRODUCT CRITERIA

We are looking for RF power amplifier ICs that meet ALL of these requirements:
- **Frequency range**: Must cover some or all of the 5-6 GHz band. Devices rated for 5.0-6.0 GHz, 5.1-5.9 GHz, 4.9-5.9 GHz, 5.0-5.8 GHz, or similar overlapping ranges all qualify.
- **Output power**: At least 5W total RF output power (P1dB or Psat). Devices up to 15W or higher are of interest.
- **Form factor**: Packaged IC (QFN, SOT, SOIC, flanged, coin, bolt-down, etc.) — NOT bare die, NOT discrete transistor assemblies, NOT complete amplifier modules with connectors. We want the chip itself.
- **Technology**: GaN (gallium nitride), GaAs (gallium arsenide), SiGe, or LDMOS. GaN is most common at this frequency/power level.

Products that are close but slightly outside these thresholds should still be captured as "near_miss":
- Frequency range close but not quite covering 5-6 GHz (e.g., 5.0-5.5 GHz only, or 4.0-5.5 GHz)
- Output power between 2W and 5W (close but under threshold)
- Output power above 15W (still useful but may be overkill)
- Bare die variants of qualifying packaged parts

NOTE: There is no price threshold for exclusion. Capture price when available.

## PHASE 1: Generate Search Terms

Create a file `rf_search_terms.json` containing 30+ search terms. Examples:

- "5 GHz GaN power amplifier IC"
- "5.8 GHz RF power amplifier chip"
- "5-6 GHz PA IC 10W"
- "C-band GaN MMIC power amplifier"
- "5 GHz WiFi power amplifier IC"
- "802.11ac 802.11ax power amplifier FEM"
- "5 GHz GaN PA QFN package"
- "5 GHz LDMOS power amplifier"
- "Qorvo 5 GHz power amplifier"
- "MACOM 5 GHz GaN amplifier"
- "Wolfspeed 5 GHz GaN PA"
- "Analog Devices 5 GHz power amplifier"
- "NXP 5 GHz LDMOS amplifier"
- "Renesas 5 GHz power amplifier"
- "Skyworks 5 GHz PA IC"
- "pSemi 5 GHz amplifier"
- "5 GHz drone video transmitter amplifier IC"
- "5.8 GHz FPV power amplifier chip"
- "UNII-1 UNII-2 UNII-3 power amplifier IC"
- "5 GHz GaN MMIC 10W datasheet"
- "5W 10W 15W 5GHz amplifier IC"
- "C-band power amplifier monolithic"
- "5 GHz PA with integrated matching"
- "GaN HEMT 5 GHz packaged amplifier"
- "5 GHz high power amplifier SMD"

Group them by intent: general discovery, manufacturer-targeted (Qorvo, MACOM, Wolfspeed/Cree, Analog Devices, NXP, Renesas, Skyworks, Guerrilla RF, pSemi), application-targeted (WiFi, FPV/drone, radar, point-to-point), technology-targeted (GaN, GaAs, LDMOS).

## PHASE 2: Part Discovery

For EACH search term group, launch a subagent with this instruction:

"Search the web using these terms: [terms]. For each term, use the WebSearch tool to search, then review the results. Return ONLY a JSON array of objects with {manufacturer, part_number, url, notes} for every RF power amplifier IC you find that operates in or near the 5-6 GHz range with output power of 5W or more. Include parts from major distributors (Digi-Key, Mouser, Arrow, Newark, Richardson RFPD) and manufacturer product pages. Exclude complete amplifier modules with connectors, bare die unless also available packaged, evaluation boards (but note if an eval board exists), and passive components."

Collect all results into `rf_parts_raw.json`. Deduplicate by part number (fuzzy match — e.g., "QPA1022" and "QPA1022-000" are the same base part). Save to `rf_parts_deduped.json`.

## PHASE 3: Part Verification

For each part (batches of 5-8), launch a subagent:

"Research these RF amplifier parts: [part_numbers + manufacturer + urls]. For each, use WebSearch and WebFetch to find the datasheet or product page and determine:
1. manufacturer: string
2. part_number: string (canonical part number)
3. status: 'active' | 'NRND' | 'obsolete' | 'sampling' | 'unknown'
4. frequency_range_ghz: string (e.g., '4.8-6.0')
5. covers_5_to_6_ghz: boolean — does the rated frequency range cover at least 5.0-5.8 GHz?
6. output_power_w: number or null (P1dB or Psat, whichever is specified)
7. output_power_dbm: number or null
8. meets_power_threshold: boolean — is output power >= 5W?
9. notes: any relevant context

Return ONLY a JSON array. Do not extract full specs yet — just verify frequency/power/status."

Save to `rf_parts_verified.json`. Filter to parts where covers_5_to_6_ghz is true AND (meets_power_threshold is true OR near-miss). Save to `rf_parts_final.json`.

## PHASE 4: Detailed Spec Extraction

For EACH part in `rf_parts_final.json`, launch a subagent (batch up to 3-5 parts per subagent):

"Research these RF power amplifier ICs in detail. For EACH part, you MUST find and fetch the datasheet (PDF or product page) from the manufacturer website. Also check distributor pages (Digi-Key, Mouser, Richardson RFPD) for pricing and availability.

IMPORTANT — How to extract specs reliably:

**Step 1: Find the datasheet.** WebSearch for '[part_number] datasheet PDF'. The manufacturer datasheet is the authoritative source for all electrical specs.

**Step 2: Fetch the product page.** Use WebFetch on the manufacturer's product page for the part. Also fetch at least one distributor page (Digi-Key or Mouser) for pricing.

**Step 3: Handle JS-rendered sites.** If WebFetch returns only JavaScript/CSS, fall back to WebSearch for '[part_number] specifications'.

**Step 4: Cross-reference pricing.** Check Digi-Key, Mouser, and Richardson RFPD for current pricing. Note quantity breaks if visible.

For each part, extract:

- manufacturer: string
- part_number: string
- description: string (one-line summary)
- technology: 'GaN' | 'GaAs' | 'LDMOS' | 'SiGe' | 'other'
- frequency_range_ghz: string (e.g., '5.0-6.0')
- output_power_w: number or null (rated output power in watts, specify whether P1dB or Psat)
- output_power_spec: 'P1dB' | 'Psat' | 'other' | null (which spec the wattage refers to)
- output_power_dbm: number or null
- gain_db: number or null (small-signal or large-signal gain, specify which)
- gain_type: 'small_signal' | 'large_signal' | 'unknown' | null
- drain_voltage_v: number or null (Vdd/Vcc/Vds recommended operating voltage)
- supply_voltage_label: string or null (e.g., 'Vdd', 'Vcc', 'Vds')
- efficiency_pct: number or null (PAE or drain efficiency, specify which)
- efficiency_type: 'PAE' | 'drain' | 'unknown' | null
- impedance_matched: boolean or null — is the input and/or output internally matched to 50 ohms?
- matching_notes: string or null (e.g., 'input matched, output requires external match', 'fully matched 50 ohm', 'unmatched')
- package_type: string or null (e.g., 'QFN 5x5mm', 'SOT-886', 'flanged', 'coin')
- package_size_mm: string or null (e.g., '5x5x0.85', '4x4')
- thermal_pad: boolean or null (has exposed thermal pad?)
- num_pins: number or null
- operating_temp_range: string or null (e.g., '-40 to +85C')
- price_usd: number or null (unit price at qty 1 or lowest available qty)
- price_qty: string or null (e.g., '1', '100', '1000')
- price_source: string or null (e.g., 'Digi-Key', 'Mouser')
- availability: 'in_stock' | 'lead_time' | 'obsolete' | 'unknown'
- datasheet_url: string or null (direct link to PDF datasheet)
- product_url: string or null (manufacturer product page)
- eval_board: string or null (eval board part number if one exists)
- notable_features: array of strings (e.g., 'integrated bias sequencing', 'single supply', 'analog predistortion', 'enable/disable pin')
- meets_criteria: boolean — frequency covers 5.0-5.8 GHz AND output_power_w >= 5
- near_miss: boolean
- near_miss_reason: string or null

Return ONLY a JSON array of part objects. Use null for specs you cannot find — do not guess."

Save each batch's results to `rf_products/[manufacturer_slug]_[batch].json`.

## PHASE 4.5: Spec Gap-Fill

After all product files are written, scan for parts missing key specs (gain_db, efficiency_pct, price_usd, package_size_mm, impedance_matched). Launch targeted subagents to fill gaps:

"These parts are missing key specs. For EACH part, search for:
1. '[part_number] datasheet' and fetch the PDF link
2. '[part_number] Digi-Key' or '[part_number] Mouser' for pricing
3. '[part_number] application note' for matching/efficiency info

Parts to research:
[list of {part_number, manufacturer, missing_fields}]

Return a JSON array of {part_number, gain_db, efficiency_pct, price_usd, package_size_mm, impedance_matched, datasheet_url} for each."

Update product files with newly found specs.

## PHASE 5: Assembly & Export

1. Merge all product files into `rf_amplifier_database.json` with structure:
```json
{
  "metadata": {
    "generated_date": "YYYY-MM-DD",
    "criteria": {
      "frequency_range": "5-6 GHz",
      "min_output_power_w": 5,
      "target_output_power_w": "5-15",
      "form_factor": "packaged IC"
    },
    "total_parts": N,
    "parts_meeting_criteria": N,
    "parts_near_miss": N
  },
  "parts": [...]
}
```

2. Generate `rf_amplifier_database.csv` with one row per part. Key columns: manufacturer, part_number, technology, frequency_range_ghz, output_power_w, output_power_dbm, gain_db, drain_voltage_v, efficiency_pct, impedance_matched, package_type, package_size_mm, price_usd, datasheet_url, meets_criteria, near_miss, near_miss_reason.

3. Generate `qualifying_rf_amplifiers.md` — Markdown table of parts where meets_criteria=true. Sort by output power ascending. Columns: Manufacturer, Part Number, Technology, Freq Range, Output Power (W), Gain (dB), Vdd (V), Efficiency (%), Matched?, Package, Price, Datasheet.

4. Generate `rf_amplifier_near_misses.md` — same format for near_miss parts with reason column.

5. Generate `rf_amplifier_research_summary.md` with:
   - Total parts found, verified, qualifying
   - Breakdown by manufacturer
   - Breakdown by technology (GaN vs GaAs vs LDMOS)
   - Output power distribution
   - Efficiency range
   - Price range
   - Package type distribution
   - How many are impedance matched vs unmatched
   - Gain range
   - Supply voltage distribution
   - Key gaps and limitations
   - Notable trends (e.g., GaN dominance at this power level)

## CRITICAL RULES FOR EFFICIENCY

1. **Subagent isolation**: Each subagent receives ONLY the data it needs.
2. **Batch sizing**: Part verification in groups of 5-8. Detailed spec extraction in groups of 3-5 parts.
3. **File-based state**: Write results to disk after each phase.
4. **No accumulation**: After a subagent returns, save results and release context.
5. **Progressive filtering**: Each phase narrows the set.
6. **Structured output only from subagents**: JSON arrays only.
7. **Error handling**: Log failures to `rf_amplifier_errors.log` and continue. Retry once.
8. **Datasheet is king**: The manufacturer datasheet is the authoritative source for all electrical specifications. Distributor pages may have errors or outdated info.
9. **Price verification**: Prices vary by quantity. Always note the quantity break. Check both Digi-Key and Mouser when possible.
10. **Distinguish packaged parts from bare die**: Many GaN parts are available as both packaged IC and bare die. Only capture the packaged version unless the die is the only option.
11. **Distinguish amplifier ICs from modules**: We want the IC/chip, not a complete module with connectors and housing. Front-end modules (FEMs) that are single-chip ICs in standard packages DO qualify.
12. **Handle JS-rendered sites**: Fall back to WebSearch if WebFetch returns only framework code.
13. **Multi-source minimum**: For price and availability, check at least 2 distributors before marking as null.
14. **No blanket WebFetch domain restrictions**: The settings grant unrestricted WebFetch access.
15. **Part number variants**: Many parts have suffixes for packaging, tape-and-reel, lead-free, etc. (e.g., QPA1022-000, QPA1022-0TR). Treat these as the same base part — capture the canonical part number and note packaging variants.

Start now with Phase 1.
```

## Usage

Paste the prompt above (everything inside the triple-backtick code fence) directly into Claude Code.

## Expected Output Files

| File | Description |
|---|---|
| `rf_search_terms.json` | Categorized search terms |
| `rf_parts_raw.json` | All parts found (with dupes) |
| `rf_parts_deduped.json` | Deduplicated parts list |
| `rf_parts_verified.json` | Parts verified for frequency/power/status |
| `rf_parts_final.json` | Filtered to qualifying and near-miss parts |
| `rf_products/*.json` | Per-batch detailed specs |
| `rf_amplifier_database.json` | Complete merged database |
| `rf_amplifier_database.csv` | Flat CSV |
| `qualifying_rf_amplifiers.md` | Parts meeting all criteria |
| `rf_amplifier_near_misses.md` | Near-miss parts |
| `rf_amplifier_research_summary.md` | Full analysis |
| `rf_amplifier_errors.log` | Any failures encountered |

## Customization Notes

- To adjust the frequency range, edit the criteria and the `covers_5_to_6_ghz` logic
- To change the power threshold, edit `min_output_power_w` and the `meets_power_threshold` logic
- Known manufacturers likely to appear: Qorvo (formerly TriQuint), MACOM, Wolfspeed (Cree), Analog Devices, NXP, Renesas (formerly IDT), Skyworks, Guerrilla RF, pSemi, Ampleon, WIN Semiconductors, RFMD (now Qorvo), Microsemi (now Microchip)
- WiFi-specific parts (802.11ac/ax FEMs) may appear — these are valid if they meet the power criteria, though many WiFi FEMs are under 5W
- FPV/drone video transmitter amplifier ICs are a relevant application — search specifically for "5.8 GHz FPV amplifier IC"
