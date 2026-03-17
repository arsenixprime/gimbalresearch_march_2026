# Near-Miss RF Power Amplifier ICs — 5–6 GHz

These parts come close to meeting the criteria (5.0–5.8 GHz coverage AND 5W–15W output power AND packaged IC) but fall short on one or two dimensions.

**Generated:** 2026-03-16
**Total near-miss parts:** 12

---

## Near Misses by Failure Reason

### Frequency Range Too High (starts above 5.0 GHz)

| Manufacturer | Part Number | Freq Range (GHz) | Power | Spec | Technology | Package | Miss Reason | URL |
|---|---|---|---|---|---|---|---|---|
| MACOM | [MAAP-011027](https://www.macom.com/products/product-detail/MAAP-011027) | 5.2–5.9 | 8W (39 dBm) | Psat | GaAs | PQFN 5×5mm 20-lead | Starts at 5.2 GHz — misses 5.0–5.2 GHz | [Link](https://www.macom.com/products/product-detail/MAAP-011027) |
| Mini-Circuits | [PMA6-73-10W+](https://www.minicircuits.com/pdfs/PMA6-73-10W+.pdf) | 5.6–7.1 | 8.5W (39.3 dBm) | Psat | GaAs | QFN 6×6mm 28-lead | Starts at 5.6 GHz — misses 5.0–5.6 GHz | [Link](https://www.minicircuits.com/pdfs/PMA6-73-10W+.pdf) |
| UMS | [CHA7060-QAB](https://www.ums-rf.com/product/cha7060-qab-5-6-8-5ghz-power-amplifier/) | 5.6–8.5 | 12W (41 dBm) | Psat | GaN | QFN 6×6mm | Starts at 5.6 GHz — misses 5.0–5.6 GHz | [Link](https://www.ums-rf.com/product/cha7060-qab-5-6-8-5ghz-power-amplifier/) |
| Guerrilla RF | [GRF5857](https://www.guerrilla-rf.com/products/detail/sku/GRF5857) | 5.2–6.0 | 3.16W (35 dBm) | Psat | GaN | — | Starts at 5.2 GHz AND under 5W | [Link](https://www.guerrilla-rf.com/products/detail/sku/GRF5858) |
| Analog Devices | [HMC7357LP5GE](https://www.analog.com/en/products/hmc7357.html) | 5.5–8.5 | 3.16W (35 dBm) | Psat | GaAs pHEMT | QFN 5×5mm 24-lead | Starts at 5.5 GHz AND under 5W | [Link](https://www.analog.com/en/products/hmc7357.html) |
| Guerrilla RF | [GRF5458](https://www.guerrilla-rf.com/products/detail/sku/GRF5458) | 5.7–6.1 | 1.26W (31 dBm) | Psat | GaN | — | Starts at 5.7 GHz AND well under 5W | [Link](https://www.guerrilla-rf.com/products/detail/sku/GRF5458) |

### Frequency Range Too Low (upper edge below 5.8 GHz)

| Manufacturer | Part Number | Freq Range (GHz) | Power | Spec | Technology | Package | Miss Reason | URL |
|---|---|---|---|---|---|---|---|---|
| Ampleon | [B10G4750N12DL](https://www.ampleon.com/products/mobile-broadband/3.3-5.0-ghz-transistors/B10G4750N12DL.html) | 4.7–5.0 | 12W (40.8 dBm) | Psat | LDMOS | LGA 7×7mm | Tops at 5.0 GHz — does not cover target band | [Link](https://www.ampleon.com/products/mobile-broadband/3.3-5.0-ghz-transistors/B10G4750N12DL.html) |
| Millibeam | [HPSB5Q](https://www.millibeam.com/product-variation/hpsb5q) | 3.0–5.5 | 5W (37 dBm) | Psat | GaN | QFN 4×5mm | Tops at 5.5 GHz — misses 5.5–5.8 GHz | [Link](https://www.millibeam.com/product-variation/hpsb5q) |
| Guerrilla RF | [GRF5847](https://www.guerrilla-rf.com/products/detail/sku/GRF5847) | 4.4–5.2 | 3.16W (35 dBm) | Psat | GaN | — | Tops at 5.2 GHz AND under 5W | [Link](https://www.guerrilla-rf.com/products/detail/sku/GRF5847) |

### Output Power Below 5W Threshold

| Manufacturer | Part Number | Freq Range (GHz) | Power | Spec | Technology | Package | Miss Reason | URL |
|---|---|---|---|---|---|---|---|---|
| Qorvo | [QPA0506](https://www.qorvo.com/products/p/QPA0506) | 5.0–6.0 | 4W (36 dBm) | Psat | GaN | QFN 4×4mm | 4W — ~1 dB under 5W threshold; exact 5–6 GHz coverage otherwise perfect | [Link](https://www.qorvo.com/products/p/QPA0506) |
| Skyworks Solutions | [SKY66288-11](https://www.skyworksinc.com/en/Products/Amplifiers/SKY66288-11) | 5.15–5.925 | 4W (36 dBm) | Psat | GaAs | QFN 5×5mm 16-lead | 4W AND NRND AND misses 5.0–5.15 GHz | [Link](https://www.skyworksinc.com/en/Products/Amplifiers/SKY66288-11) |
| MACOM | [CMPA0060002F](https://www.digikey.com/en/product-highlight/c/cree-wolfspeed/cmpa0060002f-power-amplifier) | 0.02–6.0 | 2W (33 dBm) | Psat | GaN | Flange | 2W — well under 5W; good freq coverage otherwise | [Link](https://www.digikey.com/en/product-highlight/c/cree-wolfspeed/cmpa0060002f-power-amplifier) |

---

## Notable Near Misses

**Qorvo QPA0506** — The most interesting near miss. Covers exactly 5.0–6.0 GHz with 53% PAE, 4×4mm QFN, fully matched. Only fails by ~1 dB (~1W). If a 4W output is acceptable, this is the most elegant drop-in solution. Qorvo does not appear to offer a 5W version in the same package.

**Millibeam HPSB5Q** — Misses by only 300 MHz on the upper end (5.5 vs 5.8 GHz). For applications not requiring the 5.5–5.8 GHz portion (lower UNII-2A only), this qualifies. Very high 37 dB gain.

**MAAC MAAP-011027** — GaAs MMIC that reaches 8W in the 5.2–5.9 GHz band. If 5.0–5.2 GHz coverage is not critical, this is a strong candidate. Unknown active/NRND status requires confirmation.

**UMS CHA7060-QAB** — 12W GaN MMIC, strong specs, but lower edge is 5.6 GHz. Strong candidate for applications above 5.6 GHz (upper UNII-2C, 5.8 GHz ISM band, lower C-band P2P).

**Ampleon B10G4750N12DL** — Only LDMOS part found. Designed for LTE n79/CBRS; the upper band limit of 5.0 GHz means it just touches the target band edge.
