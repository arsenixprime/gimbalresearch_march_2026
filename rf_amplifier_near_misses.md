# Near-Miss RF Power Amplifier ICs — 5-6 GHz

These parts come close to meeting the criteria (5.0–5.8 GHz coverage AND ≥5W output power AND packaged IC) but fall short on one dimension.

| Manufacturer | Part Number | Status | Technology | Freq Range | Output Power (W) | Gain (dB) | Efficiency (%) | Matched? | Package | Near Miss Reason |
|---|---|---|---|---|---|---|---|---|---|---|
| Qorvo | QPA0506 | Active | GaN | 5.0–6.0 GHz | 4 | 18 (LS) | 53% PAE | Yes | QFN 4×4mm | Power 4W, ~1 dB below 5W threshold |
| MACOM | MAGX-011086A | Active | GaN | DC–6.0 GHz | 4 | 11 (LS) | — | — | VFQFN 4mm 24L | Power 4W, below 5W threshold |
| Guerrilla RF | GRF5857 | Active | GaN | 5.2–6.0 GHz | 3.16 | — | 50% PAE | Yes | — | Power ~3W; freq starts 5.2 GHz |
| Analog Devices | HMC7357LP5GE | Active | GaAs | 5.5–8.5 GHz | 3.16 | 29 (SS) | 34% PAE | Yes | QFN 5×5mm 24L | Power 3.16W; freq starts 5.5 GHz |
| MACOM | MAAP-011027 | Unknown | GaAs | 5.2–5.9 GHz | 8 | — | 37% PAE | Yes | PQFN 5×5mm 20L | Freq range misses 5.0–5.2 GHz lower edge |
| United Monolithic Semiconductors | CHA7060-QAB | Active | GaN | 5.6–8.5 GHz | 12 | 30 (LS) | 40% PAE | Yes | QFN 6×6mm | Freq range starts 5.6 GHz, misses 5.0–5.6 GHz |
| Millibeam | HPSB5Q | Active | GaN | 3.0–5.5 GHz | 5 | 37 (LS) | — | Yes | QFN 4×5mm | Upper freq 5.5 GHz, misses 5.5–5.8 GHz |
| Integra Technologies | IGT5259CW25 | Active | GaN | 5.2–5.9 GHz | 25 | 12 (LS) | 48% drain | Yes | Metal ceramic | Freq starts 5.2 GHz, misses 5.0–5.2 GHz |
| Integra Technologies | IGT5259CW50 | Active | GaN | 5.2–5.9 GHz | 50 | — | — | Yes | Metal ceramic | Freq starts 5.2 GHz, misses 5.0–5.2 GHz |
| Analog Devices | ADPA1113 | Active | GaN | 2.3–5.7 GHz | 44.7 | 40.5 (SS) | 39% PAE | Yes | LDCC 14L ceramic | Guaranteed spec to 5.7 GHz only (headline: 2–6 GHz) |
| Qorvo | QPA2308D | Obsolete | GaN | 5.0–6.0 GHz | 60 | 21 (LS) | 43% PAE | Yes | Bare die | Discontinued; bare die (D suffix) |
| Qorvo | QPA2576N | Obsolete | GaN | 2.5–6.0 GHz | 40 | 29 (SS) | 36% PAE | — | CuW flanged | Discontinued/obsolete |

**Notes:**
- SS = small-signal gain; LS = large-signal gain
- The Integra Technologies parts (IGT5259CW25, IGT5259CW50) are the only near-misses with ≥25W that could be used in the 5.2–5.9 GHz portion of the band — useful for applications that don't require the full 5.0–5.2 GHz lower edge
- MAAP-011027 (GaAs) and CHA7060-QAB (GaN) are notable as the best near-misses for applications that only need part of the 5–6 GHz band
- QPA0506 and MAGX-011086A are within ~1 dB of the 5W power threshold and may be sufficient for some applications
- ADPA1113 (40W) is the highest-power near-miss and may still operate to 6 GHz in practice despite the guaranteed spec stopping at 5.7 GHz
