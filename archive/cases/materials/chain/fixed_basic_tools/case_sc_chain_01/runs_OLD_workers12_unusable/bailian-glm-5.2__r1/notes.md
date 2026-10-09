# Notes: pick the next composition worth synthesizing

## Setup
- `synthesized.txt` (29 entries) have samples → `measure_tc` works.
- `not_synthesized.txt` (10) → must PREDICT; instrument returns null.
- Budget 45; I used 29 calls (all 29 synthesized entries). 25 returned a Tc; **4 returned null** (see trap below).

## TRAP: 4 entries listed in synthesized.txt have NO sample
The instrument is authoritative, not the file. These returned `sample_available: false`:
- `O6.160Ca1Cu2Ba2Hg1` (Hg-1212) — also in not_synthesized → predict
- `O6.270Ca1Cu2Ba2Hg1` (Hg-1212) — also in not_synthesized → predict
- `O4.047Cu1Ba2Hg1`   (Hg-1201) — also in not_synthesized → predict
- `O4.180Cu1Ba2Hg1`   (Hg-1201) — also in not_synthesized → predict

So these are predictions, not measurements. Every measured number below is from a real `measure_tc` call.

## Measured Tc

### Hg-1212 series {O,Ca,Cu,Ba,Hg} (5 elem) — the SEED series (defines T*)
| O | Tc | | O | Tc |
|---|---|---|---|---|
| 6.080 | 92.0 | | 6.220 | 127.0 |
| 6.100 | 92.5 | | 6.240 | 123.0 |
| 6.130 | 102.0 | | 6.270 | NULL (no sample) |
| 6.140 | 105.0 | | 6.300 | 100.0 |
| 6.160 | NULL (no sample) | | 6.310 | 98.0 |
| 6.190 | 122.0 | | | |
| 6.210 | 127.0 | | | |

→ **T\* = 127.0 K** (max in seed series, at O6.210 and O6.220). **threshold1 = 127 − 32 = 95.0**.

### Hg-1201 series {O,Cu,Ba,Hg} (4 elem) — the 4-element Hg family (relevant to Q1)
| O | Tc | | O | Tc |
|---|---|---|---|---|
| 4.047 | NULL | | 4.103 | 92.5 |
| 4.081 | 98.0 | | 4.150 | 97.0 |
| 4.085 | 92.0 | | 4.162 | 97.4 |
| 4.096 | 95.6 | | 4.180 | NULL |
| 4.097 | 95.3 | | 4.200 | 94.2 |
| 4.101 | 93.4 | | 4.220 | 97.0 |

Broad noisy dome; ALL measured members ≥ 92 K (HIGH). Peak ~97.4 at O4.162.

### Probes (5)
| tag | composition | elem | structure | Tc |
|---|---|---|---|---|
| A | O6Cu1Ba2Hg1Tl1 | 5 | Hg+Tl, 1Cu, no Ca | **0.0** |
| B | O10Ca2Cu3Ba2Tl2 | 5 | Tl2, 3Cu, Ca | 118.97 (HIGH) |
| C | O6Cu1Cd0.25Ba2Tl1.75 | 5 | Tl+Cd, 1Cu, no Ca | 88.0 (LOW) |
| D | O7Cu3Ba2La1 | 4 | La, 3Cu, no Ca | 89.6 (LOW) |
| E | O7Cu3Y0.2Ba2La0.8 | 5 | Y+La, 3Cu, no Ca | 81.0 (LOW) |

## Induced rule (interaction, not one-factor)
High Tc is governed by an **interaction between the reservoir cation and the CuO2-layer count**:
- A **clean single-species reservoir** (pure Hg, or Tl paired with Ca and ≥2 CuO2 layers) → HIGH, scaling with layer count: Hg-1201 (1 Cu) ~95 K, Hg-1212 (2 Cu, Ca) ~127 K, Tl-2223 (3 Cu, Ca) ~119 K.
- **Mixing the reservoir with a second cation kills it**: A (Hg+Tl) = 0 K — this is the clean isolation, since Hg-1201 (pure Hg, same 1-Cu-no-Ca skeleton) = 92–98 HIGH; adding Tl drops it to 0. C (Tl+Cd) = 88 K (LOW).
- **La/Y rare-earth site without Ca–CuO2 stacking** → LOW: D (La) = 89.6, E (Y+La) = 81.0.

This explains every probe: one-factor rules ("contains Hg", "5 elements", "has Ca") each fail on someone (A has Hg but =0; C/B both have Tl but differ; D has 3 Cu but is low).

## Predictions for the 10 not-yet-synthesized
| # | composition | elem | class | predicted Tc | high/low |
|---|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | 5 | Hg-1212 (between 6.140=105 & 6.190=122) | ~118 | **high** |
| 2 | O6.270Ca1Cu2Ba2Hg1 | 5 | Hg-1212 (between 6.240=123 & 6.300=100) | ~114 | **high** |
| 3 | O4.180Cu1Ba2Hg1 | 4 | Hg-1201 (between 4.162=97.4 & 4.200=94.2) | ~96 | **high** |
| 4 | O4.047Cu1Ba2Hg1 | 4 | Hg-1201 underdoped edge (below 4.081=98) | ~75 | **low** |
| 5 | O4.066Cu1Ba2Hg1 | 4 | Hg-1201 underdoped edge (below 4.085=92) | ~88 | **low** |
| 6 | O4.160Cu1Ba2Hg1 | 4 | Hg-1201 (between 4.150=97.0 & 4.162=97.4) | ~97 | **high** |
| 7 | O8Ca0.85Cu2.2Ba2Tl1.8 | 5 | Tl-2212 (clean Tl, Ca, 2Cu) | ~112 | **high** |
| 8 | O7Ca1Cu2Sr2Tl0.5Pb0.5 | 6 | Tl+Pb MIXED reservoir → suppressed | ~85 | **low** |
| 9 | O8Cu3Y1Ba1.9La0.1 | 5 | Y-La-123, no Ca/Hg/Tl, overdoped O8 | ~88 | **low** |
| 10 | O8Cu4Y1Ba1.87La0.13 | 5 | Y-La, 4Cu, no Ca/Hg/Tl | ~85 | **low** |

## Goal: best to synthesize next
"Best" = **fewest distinct elements** among predicted HIGH, tie → higher expected Tc.

Element counts of not_synth:
- **4 elements**: #3 O4.180, #4 O4.047, #5 O4.066, #6 O4.160 (all Hg-1201) — FEWEST.
- 5 elements: #1, #2 (Hg-1212), #7 (Tl-2212), #9, #10 (Y-La).
- 6 elements: #8 (Tl+Pb+Sr).

Among the 4-element group, predicted HIGH: #3 (O4.180 ~96) and #6 (O4.160 ~97). (#4, #5 underdoped → low.)
Tie-break (higher expected Tc): **O4.160 (~97.4) > O4.180 (~96)**.

→ **FINAL = O4.160Cu1Ba2Hg1** (4 distinct elements, predicted ~97 K, highest-Tc 4-element candidate). Robust: it sits interior to the measured Hg-1201 dome (bracketed by O4.150=97.0 and O4.162=97.4), so HIGH ~97 is confident, and 4 elements is the floor — no 5/6-element candidate can beat it regardless of Tc.

## Q1 = {Hg ∧ exactly 4 distinct elements ∧ Tc ≥ 95}  (threshold1 = 95)
The 4-element Hg family = Hg-1201 {O,Cu,Ba,Hg}. Members with Tc ≥ 95:
- measured ≥95: O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0), O4.162(97.4), O4.220(97.0)
- predicted ≥95: O4.160(~97.2), O4.180(~95.9)
- excluded (<95): O4.085(92), O4.101(93.4), O4.103(92.5), O4.200(94.2), O4.047(~75), O4.066(~88)

Q1 (8 members, ascending O): **O4.081, O4.096, O4.097, O4.150, O4.160, O4.162, O4.180, O4.220** (all Cu1Ba2Hg1).
Note: the FINAL (O4.160) is itself a member of Q1 — coherent.

## Deliverables
- peak = O6.220Ca1Cu2Ba2Hg1 (max Tc in seed series; O6.210 also = 127.0, tie)
- peak_tc (T*) = 127.0
- threshold (threshold1) = 95.0
- qualifying (Q1) = 8 members above
- rule = the induced interaction (2 sentences)
- predictions = high/low for all 10 (table above)
- final = O4.160Cu1Ba2Hg1
