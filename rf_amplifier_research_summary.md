# RF Power Amplifier IC Research Summary
## 5–6 GHz, 5–15W Output Power, Packaged IC

**Generated:** 2026-03-16
**Research scope:** Commercial/infrastructure market tier (WiFi AP, FWA, small cell, ISM/FPV). Defense and radar parts (>15W) explicitly excluded.

---

## Executive Summary

The 5–15W packaged IC market at 5–6 GHz is genuinely sparse. Only **7 qualifying parts** were found from 5 manufacturers across 19 total candidates reviewed. The market splits sharply into three tiers:

- **Consumer WiFi FEMs:** <1.3W, 5V supply, GaAs or GaN-on-Si, $2–8 (Skyworks, Qorvo, pSemi, NXP) — too low power
- **Infrastructure/AP tier (target):** 5–15W, 22–32V GaN-on-SiC — sparse, ~7 parts found
- **Defense/radar/C-band:** 25–110W GaN MMIC — abundant but explicitly out of scope

The 5 active qualifying parts represent the practical universe of drop-in or near-drop-in options. All use GaN technology (GaN on SiC for Qorvo and MACOM, GaN process for ADI).

---

## Qualifying Parts Summary

| Part | Freq (GHz) | Power (W) | Spec | PAE (%) | Package | Status |
|---|---|---|---|---|---|---|
| MACOM CMPA0560008S | 0.5–6.0 | 10 | Psat | 40 | QFN 5×5mm | Active |
| Qorvo QPA1019 | 4.5–7.0 | 10 | Psat | 39 | QFN plastic overmold | Active |
| Qorvo QPA2962 | 2.0–20.0 | 10 | Psat | 22 | Air-cavity SMT 5×5mm | Active |
| Qorvo TGF3020-SM | 4.0–6.0 | 5 | P3dB | 53 | QFN 3×3mm | Active |
| Analog Devices ADPA1116 | 0.3–6.0 | 8.9 | Psat | 40 | LFCSP 32-lead | Active |
| MACOM CGH40010F | DC–6.0 | 13 | Psat | 65* | Flange | NRND |
| MACOM CGH40010P | DC–6.0 | 13 | Psat | 65* | Pill | NRND |

*Drain efficiency, not PAE. Unmatched broadband device.

---

## Market Landscape Analysis

### Why So Few Parts?

The 5–15W tier at 5–6 GHz is thin because:

1. **Below it:** Consumer WiFi FEMs (802.11ac/ax, WiGig) are highly optimized for low cost and integration, targeting <1W. There is no commercial driver for a 2–4W consumer 5 GHz PA — the applications don't exist at scale.

2. **Above it:** The 5.25–5.85 GHz band overlaps C-band radar and satellite downlink. Defense/radar GaN MMICs target 25–100W for phased array elements and are the bulk of catalog products in this frequency range.

3. **The gap:** Infrastructure WiFi access points and small cells at 5 GHz do use 5–15W PAs, but many of these are custom ASICs inside access point SoCs (Qualcomm, Broadcom, MediaTek) or tightly integrated modules, not discrete ICs available separately.

### Technology Breakdown

- **GaN on SiC:** 5 of 7 qualifying parts (all Qorvo and MACOM). High voltage (22–32V), high PAE, proven reliability. Dominant technology for this power/frequency tier.
- **GaN (unspecified process):** 1 qualifying part (ADPA1116 from ADI — GaN process, specific substrate not specified in datasheet).
- **GaAs:** 0 qualifying (several near-misses — MAAP-011027, PMA6-73-10W+, HMC7357 — but all fail on frequency or power).
- **LDMOS:** 0 qualifying (B10G4750N12DL near-miss tops out at 5.0 GHz; LDMOS is fundamentally less suitable above ~5 GHz).

### Manufacturer Coverage

| Manufacturer | Qualifying Parts | Notes |
|---|---|---|
| MACOM (incl. legacy Wolfspeed) | 3 | CMPA0560008S (active), CGH40010F/P (NRND) |
| Qorvo | 3 | QPA1019, QPA2962, TGF3020-SM |
| Analog Devices | 1 | ADPA1116 |
| Skyworks | 0 | SKY66288-11 is NRND and under 5W |
| NXP | 0 | No products found in 5–6 GHz at 5W+ |
| Ampleon | 0 | B10G4750N12DL tops at 5.0 GHz |
| pSemi | 0 | No products in this power range found |
| Guerrilla RF | 0 | GRF5857/GRF5847/GRF5458 all under 5W |
| Mini-Circuits | 0 | PMA6-73-10W+ starts at 5.6 GHz |
| UMS | 0 | CHA7060-QAB starts at 5.6 GHz |
| Millibeam | 0 | HPSB5Q tops at 5.5 GHz |

### Near-Miss Patterns

**Frequency gap at 5.6 GHz:** Several strong parts (PMA6-73-10W+ at 8.5W, CHA7060-QAB at 12W) start at 5.6 GHz. This aligns with the boundary between UNII-2C and the upper 5 GHz bands. These parts are ideal for upper C-band/5.8 GHz ISM but miss lower UNII-2.

**Frequency cap at 5.0–5.5 GHz:** Ampleon B10G4750N12DL and Millibeam HPSB5Q cover the lower portion of the band but don't reach 5.5–5.8 GHz.

**Power gap at 3–4W:** Qorvo QPA0506 (4W, 5.0–6.0 GHz) and Skyworks SKY66288-11 (4W, 5.15–5.9 GHz) are the closest under-threshold parts. There is a conspicuous gap between ~4W and 5W with full 5–6 GHz coverage — no parts were found bridging this.

---

## Recommendations by Use Case

### Drop-in SMT, 10W, Priority on Simplicity
**Qorvo QPA1019** or **MACOM CMPA0560008S** — both are proven, fully matched, in-stock, and well-documented for infrastructure applications.

### Minimum Power, Smallest Package
**Qorvo TGF3020-SM** (3×3mm QFN) — but requires an output matching network design. If you need truly drop-in, **Qorvo QPA0506** (4×4mm, 4W) is the smallest fully matched option, at the cost of ~1W vs the 5W threshold.

### Widest Frequency Coverage
**Qorvo QPA2962** (2–20 GHz) — useful if the design must span multiple bands. Lower PAE (22%) is the tradeoff.

### Best Efficiency
**Qorvo TGF3020-SM** (53% PAE at P3dB, estimated ~6-7W Psat) — but requires output matching.

### Best Near-Miss (if 5.0 GHz lower edge not critical)
**MACOM MAAP-011027** (5.2–5.9 GHz, 8W GaAs, PQFN 5×5mm) — if coverage of the 5.0–5.2 GHz portion is not needed, this is a strong option with good specs and compact SMT package. Verify current active/NRND status before designing in.

---

## Gaps and Limitations

- **Price data not collected:** API quota was exhausted before Phase 4 (detailed spec extraction including pricing). All prices shown as null. Consult Digi-Key, Mouser, and manufacturer direct for current pricing.
- **Package dimensions incomplete:** Several parts (QPA1019, TGF3020-SM pin count, CGH40010F/P footprint) had package details not extracted.
- **QPA2962 needs band-specific verification:** The 10W spec is a wideband rating. Performance at 5–6 GHz specifically should be confirmed against the datasheet power vs. frequency curves before assuming full 10W at band.
- **MAAP-011027 active status unknown:** Part is listed on MACOM's website but lifecycle status was not confirmed. Verify before designing in.
- **TGF3020-SM output matching:** Requires external matching network — adds PCB complexity and narrows the usable band. Evaluate whether the efficiency benefit justifies the design effort vs. a fully matched MMIC.
- **CGH40010F/P NRND:** Legacy parts on end-of-life path. Suitable for prototyping and evaluation but not for new production designs without supply chain risk assessment.

---

## IMS 2026 Vendor Search (Supplemental)

19 additional vendors from the IMS 2026 exhibitor list were searched. **No new qualifying parts found.** Key findings:

| Company | Result |
|---|---|
| RFHIC Corp | H004 (DC–6 GHz, 28W, DFN SMD) is closest — over power cap. Announced C-band QFN MMICs exist but no catalog <15W parts. |
| Filtronic | 5–6+ GHz products are high-power modules (>50W), not standalone IC chips |
| Altum RF | Catalog starts at X-band (8 GHz); no sub-6 GHz products |
| Integra Technologies | IGT5259CW25 (5.2–5.9 GHz, 25W, flanged) is closest — over power cap. All their 5 GHz parts are >15W radar transistors. |
| MaXentric Technologies | R&D/systems company; no catalog GaN MMIC ICs sold |
| TagoreTech | Portfolio tops out at ~4 GHz |
| Diramics | LNA transistors only — not a power amplifier company |
| Polyfet RF Devices | Maximum frequency 3 GHz |
| CPC Amps | Makes complete rack/bench amplifier systems, not IC chips |
| Soctera | Pre-commercial startup, no catalog parts |
| Finwave Semiconductor | FW8001 (4W) and FW8002 (8W) cover 3.1–4.1 GHz in 5×5mm QFN — **new near-misses**, frequency falls short of 5 GHz |
| Oso Semiconductor | mmWave beamformer chipsets; no 5–6 GHz PA ICs |
| Otava RF | mmWave beamformer and filter ICs; no 5–6 GHz PA products |
| GCS | Pure-play foundry; no catalog ICs sold |
| WIN Semiconductors | Pure-play foundry; no catalog ICs sold |
| Tower Semiconductor | Pure-play foundry; no catalog ICs sold |
| Nxbeam Inc. | All products >12 GHz (Ka/V/E-band) |
| CML Micro | Gap between sub-GHz and Ka-band; nothing at 5–6 GHz |
| WAVEPIA Co., Ltd. | WP285P5020UH(S) (5–6 GHz, ~25.7W, flanged) is exact frequency target but over 15W cap. MMIC PAs at 5–6 GHz are bare die only. |

The IMS search further confirms the market gap: **the 5–15W range at 5–6 GHz is genuinely underserved.** Packaged GaN transistors from RFHIC, WAVEPIA, and Integra cluster at 25–80W for C-band radar. The only movement toward sub-15W is in foundries' process nodes (WIN NP45-11, Tower GaN) — suggesting future products may emerge as infrastructure customers tape out lower-power designs.

**Finwave FW8002** (8W, 3.1–4.1 GHz, QFN) is the most promising "watch" part — if Finwave extends their FinFET PA to a 5 GHz variant, it would likely qualify.

---

## Search Coverage

50 search terms were used across 4 categories: general discovery, manufacturer-targeted (MACOM, Qorvo, ADI, Skyworks, pSemi, NXP, Ampleon, Guerrilla RF, Mini-Circuits, UMS, Millibeam), application-targeted (WiFi AP, FWA, FPV/drone, backhaul, ISM), and technology-targeted (GaN 5W/10W, GaAs 5 GHz, 5.8 GHz ISM). The search deliberately avoided C-band radar, AESA, and defense terminology to stay in the commercial tier.

Key search term categories that were productive: "GaN 5GHz 5W 10W packaged amplifier IC", "WiFi access point power amplifier IC 5GHz", "5.8 GHz FPV drone video transmitter amplifier IC", manufacturer-specific searches (Qorvo, MACOM, ADI).

Categories that returned no qualifying results: NXP GaN/LDMOS (no 5 GHz products at this power), pSemi (no high-power PAs, only switches/attenuators), Ampleon (LDMOS ceiling at 5.0 GHz).
