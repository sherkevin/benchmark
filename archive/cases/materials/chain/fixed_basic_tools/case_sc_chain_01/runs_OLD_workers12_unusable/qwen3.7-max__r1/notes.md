# Notes — case_sc_chain_01, qwen3.7-max r1

## Data overview

### Synthesized (29 listed, 25 actually measured, 4 unavailable)
**Series A** — O-Ca-Cu-Ba-Hg (5 elements), varying O: 12 listed
**Series B** — O-Cu-Ba-Hg (4 elements), varying O: 12 listed  
**Other**: 5 compositions

### Not synthesized (10 compositions)
4 of these overlap with the synthesized list (O6.160, O6.270, O4.180, O4.047) — measure_tc confirmed they have no sample.

---

## Measurement results

### Series A (O-Ca-Cu-Ba-Hg, 5 elements)
| Composition | O content | Tc (K) | High? |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 6.080 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 6.100 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 6.130 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 6.140 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | 6.160 | NO SAMPLE | — |
| O6.190Ca1Cu2Ba2Hg1 | 6.190 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 6.210 | **127.0** | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 6.220 | **127.0** | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 6.240 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | 6.270 | NO SAMPLE | — |
| O6.300Ca1Cu2Ba2Hg1 | 6.300 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 6.310 | 98.0 | Yes |

→ Clean dome peaking at O ≈ 6.21–6.22, T* = **127.0 K**

### Series B (O-Cu-Ba-Hg, 4 elements)
| Composition | O content | Tc (K) | High? |
|---|---|---|---|
| O4.081Cu1Ba2Hg1 | 4.081 | 98.0 | Yes |
| O4.162Cu1Ba2Hg1 | 4.162 | 97.4 | Yes |
| O4.150Cu1Ba2Hg1 | 4.150 | 97.0 | Yes |
| O4.220Cu1Ba2Hg1 | 4.220 | 97.0 | Yes |
| O4.096Cu1Ba2Hg1 | 4.096 | 95.6 | Yes |
| O4.097Cu1Ba2Hg1 | 4.097 | 95.3 | Yes |
| O4.200Cu1Ba2Hg1 | 4.200 | 94.2 | Yes |
| O4.101Cu1Ba2Hg1 | 4.101 | 93.4 | Yes |
| O4.103Cu1Ba2Hg1 | 4.103 | 92.5 | Yes |
| O4.085Cu1Ba2Hg1 | 4.085 | 92.0 | Yes |
| O4.180Cu1Ba2Hg1 | 4.180 | NO SAMPLE | — |
| O4.047Cu1Ba2Hg1 | 4.047 | NO SAMPLE | — |

→ All measured ≥ 90 K (range 92.0–98.0), noisy but all HIGH

### Other synthesized
| Composition | Elements | Tc (K) | High? |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl (5) | 0.0 | **No** |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl (5) | 118.97 | Yes |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl (5) | 88.0 | No |
| O7Cu3Ba2La1 | O,Cu,Ba,La (4) | 89.6 | No |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La (5) | 81.0 | No |

---

## Rule induction

### HIGH compositions (Tc ≥ 90):
- Series A: all have Ca → HIGH
- Series B: all have Hg (no Tl, no Ca) → HIGH  
- O10Ca2Cu3Ba2Tl2: has Ca (no Hg) → HIGH

### LOW compositions (Tc < 90):
- O6Cu1Ba2Hg1Tl1: has Hg + Tl, no Ca → LOW (0.0!)
- O6Cu1Cd0.250Ba2Tl1.750: has Tl, no Ca, no Hg → LOW (88.0)
- O7Cu3Ba2La1: no Ca, no Hg → LOW (89.6)
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg → LOW (81.0)

### Induced rule (interaction, not one-factor):
**HIGH iff (contains Ca) OR (contains Hg AND does NOT contain Tl).**

Verification against all 7 distinct chemical families:
1. Ca + Hg (series A): Ca → HIGH ✓
2. Hg only (series B): Hg ∧ ¬Tl → HIGH ✓
3. Hg + Tl (O6Cu1Ba2Hg1Tl1): ¬Ca ∧ (Hg ∧ Tl) → LOW ✓
4. Ca + Tl (O10Ca2Cu3Ba2Tl2): Ca → HIGH ✓
5. Tl only (O6Cu1Cd0.250Ba2Tl1.750): ¬Ca ∧ ¬Hg → LOW ✓
6. La only (O7Cu3Ba2La1): ¬Ca ∧ ¬Hg → LOW ✓
7. Y + La (O7Cu3Y0.200Ba2La0.800): ¬Ca ∧ ¬Hg → LOW ✓

All 7 match ✓. This is an interaction rule — Hg alone gives high Tc, but Hg+Tl together kills it (unless Ca is present to override).

---

## Predictions for not-synthesized

| # | Composition | Elements | Rule check | Prediction |
|---|---|---|---|---|
| 1 | O6.160Ca1Cu2Ba2Hg1 | 5 (O,Ca,Cu,Ba,Hg) | has Ca | **high** |
| 2 | O6.270Ca1Cu2Ba2Hg1 | 5 (O,Ca,Cu,Ba,Hg) | has Ca | **high** |
| 3 | O4.180Cu1Ba2Hg1 | 4 (O,Cu,Ba,Hg) | Hg ∧ ¬Tl | **high** |
| 4 | O4.047Cu1Ba2Hg1 | 4 (O,Cu,Ba,Hg) | Hg ∧ ¬Tl | **high** |
| 5 | O4.066Cu1Ba2Hg1 | 4 (O,Cu,Ba,Hg) | Hg ∧ ¬Tl | **high** |
| 6 | O4.160Cu1Ba2Hg1 | 4 (O,Cu,Ba,Hg) | Hg ∧ ¬Tl | **high** |
| 7 | O8Ca0.850Cu2.200Ba2Tl1.800 | 5 (O,Ca,Cu,Ba,Tl) | has Ca | **high** |
| 8 | O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 (O,Ca,Cu,Sr,Tl,Pb) | has Ca | **high** |
| 9 | O8Cu3Y1Ba1.900La0.100 | 5 (O,Cu,Y,Ba,La) | ¬Ca ∧ ¬Hg | **low** |
| 10 | O8Cu4Y1Ba1.870La0.130 | 5 (O,Cu,Y,Ba,La) | ¬Ca ∧ ¬Hg | **low** |

---

## T* and threshold

- Seed: O6.080Ca1Cu2Ba2Hg1
- Series = {O, Ca, Cu, Ba, Hg} with varying O (series A)
- Peak measured Tc = 127.0 K at O6.210Ca1Cu2Ba2Hg1 (and O6.220 tied)
- Unmeasured series members (O6.160, O6.270) fall on dome slopes, won't exceed 127
- **T* = 127.0 K**
- **threshold1 = 127.0 - 32 = 95.0 K**

---

## Q1 qualifying set (Hg + 4 elements + Tc ≥ 95.0)

All Hg + 4-element compositions come from series B ({O, Cu, Ba, Hg}).

Measured and qualifying (Tc ≥ 95.0):
1. O4.081Cu1Ba2Hg1 — 98.0 ✓
2. O4.162Cu1Ba2Hg1 — 97.4 ✓
3. O4.150Cu1Ba2Hg1 — 97.0 ✓
4. O4.220Cu1Ba2Hg1 — 97.0 ✓
5. O4.096Cu1Ba2Hg1 — 95.6 ✓
6. O4.097Cu1Ba2Hg1 — 95.3 ✓

Measured and NOT qualifying (Tc < 95.0):
- O4.200Cu1Ba2Hg1 — 94.2 ✗
- O4.101Cu1Ba2Hg1 — 93.4 ✗
- O4.103Cu1Ba2Hg1 — 92.5 ✗
- O4.085Cu1Ba2Hg1 — 92.0 ✗

Predicted (not synthesized, estimated by interpolation):
- O4.160Cu1Ba2Hg1 (~97 K, near O4.150=97.0 and O4.162=97.4) → ≥ 95.0 ✓
- O4.180Cu1Ba2Hg1 (~96 K, between O4.162=97.4 and O4.200=94.2) → ≥ 95.0 ✓
- O4.066Cu1Ba2Hg1 (~95 K, between O4.047 and O4.081=98.0, inv-dist weighted ~95.4) → borderline, include
- O4.047Cu1Ba2Hg1 (~93 K, below O4.081, under-doped side of dome) → < 95.0 ✗

**Q1 = 9 compositions** (6 measured + 3 predicted)

---

## Final selection (goal)

Best to synthesize = fewest elements among HIGH, tie-break by higher expected Tc.

HIGH candidates by element count:
- 4 elements: O4.180 (~96), O4.047 (~93), O4.066 (~95), O4.160 (~97)
- 5 elements: O6.160 (~113), O6.270 (~112), O8Ca0.850Cu2.200Ba2Tl1.800 (~110)
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500 (~100)

Minimum elements among HIGH = **4**. Four candidates tied.

Tie-break by higher expected Tc:
- O4.160Cu1Ba2Hg1: ~97 K (highest)
- O4.180Cu1Ba2Hg1: ~96 K
- O4.066Cu1Ba2Hg1: ~95 K
- O4.047Cu1Ba2Hg1: ~93 K

**Winner: O4.160Cu1Ba2Hg1** (4 elements, expected ~97 K)
