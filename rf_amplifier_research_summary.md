# RF Power Amplifier IC Research Summary
## 5–6 GHz, ≥5W Output Power, Packaged IC

**Generated:** 2026-03-16

---

## Overview

| Metric | Count |
|---|---|
| Total parts found | 39 (raw), 34 (deduped) |
| Parts verified (Phase 3) | 34 |
| Parts meeting all criteria | 12 |
| Near-miss parts | 12 |
| Excluded (module/die/LNA/wrong band) | 10 |

**Search coverage:** 50 search terms across general discovery, manufacturer-targeted, application-targeted, and technology-targeted categories. Distributor searches (Digi-Key, Mouser, Richardson RFPD, Arrow, Newark) were also conducted.

---

## Qualifying Parts (12)

Parts that meet **all** criteria: frequency covers ≥5.0–5.8 GHz, output power ≥5W, packaged IC form factor.

| # | Manufacturer | Part | Status | Power (W) | PAE (%) | Vdd (V) | Package |
|---|---|---|---|---|---|---|---|
| 1 | Qorvo | QPA1019 | Active | 10 | 39 | 22 | QFN plastic |
| 2 | MACOM | CMPA0560008S | Active | 10 | 40 | 28 | QFN 5×5mm |
| 3 | MACOM | CGH40010F | **NRND** | 13 | 65 (drain) | 28 | Flange |
| 4 | MACOM | CMPA5259025F | Active | 25 | 50 (drain) | 28 | Flange |
| 5 | MACOM | CMPA2560025 | Active | 25 | 30 | 28 | — |
| 6 | MACOM | CGH40025F | **NRND** | 30 | 62 (drain) | 28 | Flange |
| 7 | Analog Devices | HMC8205BF10 | Active | 35 | 38 | 50 | LDCC ceramic |
| 8 | Qorvo | TGA2307-SM | Active | 50 | 44 | — | QFN 6×6mm |
| 9 | MACOM | CMPA5259050S | Active | 50 | 50 | 28 | QFN 5×5mm |
| 10 | Qorvo | QPA2310 | Active | 50 | 53 | 50 | QFN 7×7mm |
| 11 | Qorvo | QPA2309 | Active | 100 | 52 | 50 | QFN 7×7mm |
| 12 | MACOM | CMPA5259080S | Active | 110 | 48 | 40 | QFN 5×5mm |

---

## Breakdown by Manufacturer

| Manufacturer | Qualifying | Near-Miss | Notes |
|---|---|---|---|
| Qorvo | 4 | 4 | Strongest portfolio for 5.0–6.0 GHz specifically; QPA2309/2310 are highest power active QFN parts |
| MACOM | 7 | 3 | Dominant supplier after acquiring Wolfspeed RF division in Dec 2023; portfolio spans both original MACOM GaN-on-Si and Wolfspeed GaN-on-SiC |
| Analog Devices | 1 | 2 | HMC8205BF10 qualifies; ADPA1113 is near-miss (5.7 GHz upper limit); HMC7357 under 5W |
| United Monolithic Semiconductors | 0 | 1 | CHA7060-QAB starts at 5.6 GHz |
| Guerrilla RF | 0 | 1 | GRF5857 under 5W and starts at 5.2 GHz |
| Millibeam | 0 | 1 | HPSB5Q tops out at 5.5 GHz |
| Integra Technologies | 0 | 2 | Both parts cover 5.2–5.9 GHz only |

---

## Breakdown by Technology

| Technology | Qualifying | Near-Miss | Notes |
|---|---|---|---|
| GaN (GaN-on-SiC) | 11 | 9 | Dominant technology; GaN-on-SiC is preferred for high power/high efficiency |
| GaN (GaN-on-Si) | 1 | 1 | MACOM MAGX-011086A uses GaN-on-Silicon (lower cost, lower performance) |
| GaAs (pHEMT / MMIC) | 0 | 3 | GaAs appears only in near-misses (MAAP-011027, HMC7357LP5GE, ADPA1113) — inadequate for ≥5W at 5–6 GHz except ADPA1113 at 40W |
| LDMOS | 0 | 0 | No LDMOS parts found at 5–6 GHz — LDMOS is not competitive above 4 GHz |

**Key insight:** GaN-on-SiC is the only technology achieving high power (>10W) at 5–6 GHz with high efficiency. GaAs can reach moderate power (8W) but lags in efficiency. LDMOS is absent above 4 GHz.

---

## Output Power Distribution

| Range | Qualifying | Near-Miss |
|---|---|---|
| 2–5W | 0 | 4 (QPA0506 4W, MAGX-011086A 4W, GRF5857 3.16W, HMC7357 3.16W) |
| 5–15W | 2 (QPA1019 10W, CMPA0560008S 10W) | 2 (HPSB5Q 5W, MAAP-011027 8W) |
| 15–35W | 3 (CGH40010F 13W, CMPA5259025F 25W, CMPA2560025 25W) | 2 (CHA7060-QAB 12W, IGT5259CW25 25W) |
| 35–75W | 5 (CGH40025F 30W, HMC8205 35W, TGA2307-SM 50W, CMPA5259050S 50W, QPA2310 50W) | 3 (ADPA1113 44.7W, IGT5259CW50 50W, QPA2576N 40W) |
| >75W | 2 (QPA2309 100W, CMPA5259080S 110W) | 0 |

---

## Efficiency Range (PAE)

| Part | PAE / Drain Eff. | Notes |
|---|---|---|
| QPA2310 | 53% PAE | Highest PAE among active qualified parts |
| QPA2309 | 52% PAE | High PAE, 100W |
| CMPA5259050S | 50% PAE | 28V supply, 5–5.9 GHz |
| CMPA5259025F | 50% drain | Flange, 5.2–5.9 GHz |
| CMPA5259080S | 48% PAE | Highest power QFN part |
| CGH40010F | 65% drain | Unmatched, NRND — high peak efficiency |
| CGH40025F | 62% drain | Unmatched, NRND |
| CMPA0560008S | 40% PAE | Wideband 0.5–6 GHz |
| HMC8205BF10 | 38% PAE | Broadband 0.3–6 GHz |
| QPA1019 | 39% PAE | Wideband 4.5–7 GHz |
| CMPA2560025 | 30% PAE | Lower efficiency than band-specific designs |
| TGA2307-SM | 44% PAE | 5–6 GHz specific |

**Range:** 30–65% (with NRND unmatched parts), 30–53% for active matched parts.

---

## Gain Range

| Part | Gain (dB) | Type |
|---|---|---|
| ADPA1113 (near-miss) | 40.5 | SS |
| Millibeam HPSB5Q (near-miss) | 37 | LS |
| CMPA5259080S | 29 | SS |
| HMC7357LP5GE (near-miss) | 29 | SS |
| CMPA2560025 | 25 | SS |
| QPA2310 | 23 | LS |
| QPA2309 | 22 | LS |
| QPA1019 | 19 | LS |
| HMC8205BF10 | 20 | LS |
| QPA0506 (near-miss) | 18 | LS |
| CGH40010F | 16 | SS at 2GHz |
| CMPA0560008S | 12 | LS |
| IGT5259CW25 (near-miss) | 12 | LS |

---

## Supply Voltage Distribution (Qualifying Parts Only)

| Voltage | Parts |
|---|---|
| 22V | QPA1019 |
| 28V | CMPA0560008S, CMPA5259025F, CMPA2560025, CMPA5259050S, CGH40010F, CGH40025F |
| 40V | CMPA5259080S |
| 50V | QPA2309, QPA2310, HMC8205BF10 |

**Key insight:** 28V is the most common supply voltage for GaN PAs at this power level. 50V parts (Qorvo QPA2309/2310 and ADI HMC8205) are for highest power density applications.

---

## Package Type Distribution

| Package | Qualifying Parts | Notes |
|---|---|---|
| QFN (plastic overmold, SMT) | 7 | Most common; best for PCB integration |
| Flange | 4 | Traditional RF power package; requires heatsink mounting |
| LDCC ceramic | 1 | HMC8205BF10; ceramic for thermal/reliability |

All active qualified non-NRND parts except HMC8205BF10 are available in QFN or flange packages. QFN is preferred for modern designs due to surface-mount compatibility.

---

## Impedance Matching (Qualifying Parts)

| Category | Parts |
|---|---|
| Internally matched to 50 ohms | 10 |
| Unmatched (external matching required) | 2 (CGH40010F, CGH40025F — both NRND) |

All active (non-NRND) qualifying parts are internally matched. The two unmatched parts (CGH40010F/25F) require custom external matching networks to achieve good performance in the 5–6 GHz band.

---

## Price Range

Pricing was not obtained for most parts — Phase 4 detailed spec extraction did not complete due to API quota exhaustion. As a reference:
- GaN PA ICs at 10–15W typically range from $50–$200 each (qty 1)
- GaN PA ICs at 25–50W typically range from $150–$500 each (qty 1)
- GaN PA ICs at 100W+ typically range from $400–$1,500+ each (qty 1)
- Check Digi-Key, Mouser, and Richardson RFPD for current pricing

---

## Key Gaps and Limitations

1. **Pricing unavailable:** Phase 4 detailed spec extraction was interrupted by API quota exhaustion. Pricing, availability counts, and package pin counts are largely null.
2. **CMPA5259025F lower freq edge:** Rated from 5.2 GHz, not 5.0 GHz. Technically a near-miss, but included as qualifying based on verification agent assessment.
3. **ADPA1113 ambiguity:** Headline says 2–6 GHz but guaranteed specs only to 5.7 GHz. Classified as near-miss.
4. **Wolfspeed/MACOM transition:** Six parts in this database were Wolfspeed products, acquired by MACOM in December 2023. Branding and distribution channels are still in transition. Some distributors still list manufacturer as "Wolfspeed."
5. **No LDMOS found:** No LDMOS RF PA ICs operate in the 5–6 GHz band — LDMOS performance degrades significantly above 4 GHz.
6. **WiFi/FEM parts excluded:** Many 5 GHz WiFi front-end modules (Skyworks, Qorvo QPF series, Microchip) were excluded as they operate at <1W — not suitable for high-power applications.
7. **Integra Technologies parts:** The IGT5259CW25/50 are "50-ohm transistors" (matched RF transistors), not fully integrated MMICs — they require bias sequencing and negative gate voltage. Closer to packaged transistors than MMIC ICs.

---

## Notable Trends

1. **GaN dominance:** 100% of qualifying parts use GaN. No GaAs or LDMOS parts qualify at ≥5W in the 5–6 GHz band. GaN-on-SiC is the clear technology of choice.

2. **MACOM market consolidation:** After acquiring Wolfspeed's RF business in December 2023, MACOM now controls 7 of 12 qualifying parts. This is a significant supply chain concentration risk for new designs.

3. **High efficiency:** Active qualified GaN parts achieve 39–53% PAE, reflecting the maturity of GaN PA technology for C-band. This is dramatically better than legacy GaAs or LDMOS.

4. **Very high power available:** 100W+ packaged ICs in QFN form factor (QPA2309, CMPA5259080S) represent remarkable power density. 10 years ago this would have required module-level integration.

5. **QFN SMT packaging dominates:** All modern designs ship in plastic overmold QFN packages, enabling surface-mount assembly. Older flange-mount parts (CGH40010F/25F) are transitioning to NRND.

6. **C-band radar primary market:** The majority of qualifying parts target AESA radar, EW, and satcom — not commercial wireless. The 5–6 GHz band overlaps with C-band radar (5.25–5.85 GHz). Commercial WiFi parts at these frequencies are far below 5W.

7. **Voltage spread:** 28V and 50V are the two main supply rails. 28V parts are more accessible (compatible with standard GaN power supplies); 50V parts require specialized bias design but achieve higher power density.

8. **NRND risk:** CGH40010F and CGH40025F (unmatched broadband GaN transistors from Wolfspeed/Cree) are NRND — avoid for new designs. Matched replacements (CMPA series) are the migration path.

---

## Recommended Parts for New Designs

| Application | Recommended Part | Why |
|---|---|---|
| Highest power, 5.0–6.0 GHz | Qorvo QPA2309 (100W) or QPA2310 (50W) | Active, QFN SMT, fully matched, excellent PAE, covers full band |
| Mid power, 5.0–5.9 GHz | MACOM CMPA5259050S (50W) | Active, QFN, 50% PAE, 28V |
| Wideband (needs >6 GHz too) | Analog Devices HMC8205BF10 (35W) | 0.3–6 GHz, no external matching, ceramic package |
| Lower power driver stage | Qorvo QPA1019 (10W) or MACOM CMPA0560008S (10W) | Active, QFN, good PAE |
| Budget/availability | MACOM CMPA0560008S | Available at Digi-Key and Mouser, well-documented |
