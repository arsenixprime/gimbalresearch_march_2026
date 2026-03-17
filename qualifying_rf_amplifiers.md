# Qualifying RF Power Amplifier ICs — 5–6 GHz, 5–15W

**Criteria:** Frequency range must cover 5.0–5.8 GHz; output power 5W–15W (Psat or P3dB); packaged IC (not bare die, not complete module with connectors); hard upper power cap 15W.

**Generated:** 2026-03-16
**Total qualifying parts:** 7 (5 active, 2 NRND)

---

## Active Parts (Recommended for New Designs)

| # | Manufacturer | Part Number | Freq Range (GHz) | Output Power | Spec | Gain (dB) | Vdd (V) | PAE (%) | Package | Notes |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | MACOM | [CMPA0560008S](https://www.macom.com/products/product-detail/CMPA0560008) | 0.5–6.0 | 10W (40 dBm) | Psat | 12 (LS) | 28 | 40 | QFN 5×5mm | GaN on SiC MMIC, fully matched, in stock at Digi-Key/Mouser |
| 2 | Qorvo | [QPA1019](https://www.qorvo.com/products/p/QPA1019) | 4.5–7.0 | 10W (40 dBm) | Psat | 19 (LS) | 22 | 39 | QFN plastic overmold | GaN on SiC (QGaN15), fully matched, lower 22V supply |
| 3 | Qorvo | [QPA2962](https://www.qorvo.com/products/p/QPA2962) | 2.0–20.0 | 10W (40 dBm) | Psat | 13 (LS) | 22 | 22 | Air-cavity SMT 5×5mm | Ultra-wideband GaN MMIC; verify 5–6 GHz perf specifically; lower PAE is tradeoff for bandwidth |
| 4 | Qorvo | [TGF3020-SM](https://www.qorvo.com/products/p/TGF3020-SM) | 4.0–6.0 | 5W (37 dBm) | P3dB | — | 32 | 53 | QFN 3×3mm | Discrete GaN HEMT; input 50Ω matched, **output unmatched** — external output network required; Psat est. 6–7W |
| 5 | Analog Devices | [ADPA1116](https://www.analog.com/en/products/adpa1116.html) | 0.3–6.0 | 8.9W (39.5 dBm) | Psat | 23.5 (LS) | 28 | 40 | LFCSP 32-lead | GaN MMIC, fully matched and AC-coupled; rates to full 6 GHz (sibling ADPA1113 only guarantees to 5.7 GHz) |

## NRND Parts (Not Recommended for New Designs — Legacy Stock Only)

| # | Manufacturer | Part Number | Freq Range (GHz) | Output Power | Spec | Gain (dB) | Vdd (V) | Drain Eff. (%) | Package | Notes |
|---|---|---|---|---|---|---|---|---|---|---|
| 6 | MACOM | [CGH40010F](https://assets.wolfspeed.com/uploads/2020/12/CGH40010.pdf) | DC–6.0 | 13W (41 dBm) | Psat | 16 (SS) | 28 | 65 | Flange (screw-down) | GaN HEMT, **unmatched** — external matching required for 5–6 GHz; NRND; originally Wolfspeed/Cree |
| 7 | MACOM | [CGH40010P](https://assets.wolfspeed.com/uploads/2020/12/CGH40010.pdf) | DC–6.0 | 13W (41 dBm) | Psat | 16 (SS) | 28 | 65 | Pill (solder-down) | Same die as CGH40010F; pill/solder-down package; NRND |

---

## Key Design Notes

**GaN dominance:** All 7 qualifying parts use GaN technology. No GaAs or LDMOS parts meet the 5W threshold across the full 5–6 GHz band.

**Supply voltage:** Most parts require 22–32V drain bias. TGF3020-SM requires 32V. Lower-power near-miss parts (GRF5857, HMC7357) operate at 3–8V but do not reach 5W.

**Impedance matching:** Five of the 7 are fully matched to 50Ω (drop-in SMT). CGH40010F/P are unmatched. TGF3020-SM input is pre-matched; output requires an external matching network.

**Efficiency:** TGF3020-SM leads at 53% PAE at P3dB. CMPA0560008S and ADPA1116 follow at 40% PAE. CGH40010F/P achieve 65% drain efficiency but are broadband unmatched — efficiency at 5–6 GHz with external matching will differ. QPA2962 has lowest PAE (22%) as the cost of 2–20 GHz bandwidth.

**LS** = large-signal gain; **SS** = small-signal (small-signal gain at rated drain)
