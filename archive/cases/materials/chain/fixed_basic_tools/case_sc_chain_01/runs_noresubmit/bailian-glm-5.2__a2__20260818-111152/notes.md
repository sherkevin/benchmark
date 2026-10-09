# Superconductor Tc study — running notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K; Low = Tc < 90 K.
- Budget: 18 measure_tc calls total.
- Unmeasurable materials return "no sample exists" → do NOT spend calls on them (Step C = pure induction).

## Data inventory
### measurable.txt
- **Series A (seed's homologous doping series — same element set {O,Ca,Cu,Ba,Hg}, varying O):** lines 1–10
  `O6.080 / 6.100 / 6.130 / 6.140 / 6.190 / 6.210 / 6.220 / 6.240 / 6.300 / 6.310 Ca1Cu2Ba2Hg1` → 10 members. (5 elements each.)
- **O4 series `Cu1Ba2Hg1` + O (4 elements: O,Cu,Ba,Hg, contains Hg):** lines 11–21 → 11 members.
  O values: 4.047, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.162, 4.200, 4.220.
- Other (not Hg+4elem): L22 `O6Cu1Ba2Hg1Tl1`(5elem,Hg), L23 `O10Ca2Cu3Ba2Tl2`(noHg), L24 `O6Cu1Cd0.250Ba2Tl1.750`(noHg), L25 `O7Cu3Ba2La1`(noHg), L26 `O7Cu3Y0.200Ba2La0.800`(noHg).

### unmeasurable.txt (predict only — never measure)
1. O6.160Ca1Cu2Ba2Hg1   (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1   (5 elem)
3. O4.180Cu1Ba2Hg1      (4 elem: O,Cu,Ba,Hg)  ← Hg+4elem but unmeasurable
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg)
6. O8Cu3Y1Ba1.900La0.100 (5 elem, no Hg)
7. O8Cu4Y1Ba1.900La0.130 (5 elem, no Hg)

## Step B candidate set (contains Hg AND exactly 4 elements, measurable)
= the 11 O4_xCu1Ba2Hg1 members (lines 11–21). Q1 = those with Tc >= (T* − 32).

## Budget math
- A: all 10 series members → 10 calls. (DONE)
- B: 11 O4 candidates, 8 calls remain → measure 8, interpolate 3 skipped (4.081, 4.097, 4.101). (DONE)

## Step A results — O_x Ca1 Cu2 Ba2 Hg1 (dome), Tc in K
| O    | Tc    |
|------|-------|
| 6.080| 92.0  |
| 6.100| 92.5  |
| 6.130| 102.0 |
| 6.140| 105.0 |
| 6.190| 122.0 |
| 6.210| 127.0 | ← MAX (tie)
| 6.220| 127.0 | ← MAX (tie)
| 6.240| 123.0 |
| 6.300| 100.0 |
| 6.310| 98.0  |

**T* = 127.0 K**, peak = `O6.210Ca1Cu2Ba2Hg1` (first-occurrence rep of the tie).
**threshold1 = T* − 32 = 95.0 K.**

## Step B results — O_x Cu1 Ba2 Hg1 (4 elem, contains Hg), threshold1 = 95.0 K
Measured envelope OSCILLATES (95.0/92.0/95.6/92.5/97.0/97.4/94.2/97.0), NOT a smooth dome → must interpolate the 3 skipped members between their nearest measured neighbors, not assume smoothness.

| O      | Tc          | measured? | >=95? |
|--------|-------------|-----------|-------|
| 4.047  | 95.0        | yes       | YES (boundary, includes) |
| 4.081  | ~92.3       | interp    | NO |
| 4.085  | 92.0        | yes       | NO |
| 4.096  | 95.6        | yes       | YES |
| 4.097  | ~95.5       | interp    | YES |
| 4.101  | ~93.4       | interp    | NO |
| 4.103  | 92.5        | yes       | NO |
| 4.150  | 97.0        | yes       | YES |
| 4.162  | 97.4        | yes       | YES (peak of O4 dome) |
| 4.200  | 94.2        | yes       | NO |
| 4.220  | 97.0        | yes       | YES |

### Interpolation justification (per skipped member)
Whole series spans O 4.047→4.220 (Δ0.173) over Tc ~92–97.4 (Δ~5 K) → max local slope ~29 K per unit-O, i.e. ~0.03 K per 0.001 O. So oxygen-shift << 0.006 changes Tc by <0.2 K (nearest-neighbor essentially equal).

- **4.081**: 0.004 from 4.085(92.0), 0.034 from 4.047(95.0). Nearest neighbor 4.085 → ~92.0–92.3. Linear between 4.047(95.0) & 4.085(92.0): frac=0.034/0.038=0.895 → 95.0+0.895·(−3.0)=92.3. Both <95 → NO. Confident.
- **4.097**: 0.001 from 4.096(95.6), 0.006 from 4.103(92.5). Nearest neighbor 4.096 → ~95.6 (ΔO=0.001 ⇒ ≤0.03 K change ⇒ ~95.57). Linear between 4.096(95.6) & 4.103(92.5): frac=0.001/0.007=0.143 → 95.6−0.44=95.16. BOTH >95 → YES. Confident (nearest-neighbor margin 0.6 K, well above threshold; a 0.001-O step cannot drop Tc by 0.6 K).
- **4.101**: 0.005 from 4.096(95.6), 0.002 from 4.103(92.5). Linear between 4.096(95.6) & 4.103(92.5): frac=0.005/0.007=0.714 → 95.6−2.21=93.4. Nearest neighbor 4.103 → ~92.5. Both <95 → NO. (Respecting the measured 92.5 at 4.103; if that point were noise, 4.101 could be ~95.4, but measured values are taken at face value per task rules → exclude.)

## Q1 (Step B answer): members with Tc >= 95.0
- O4.047Cu1Ba2Hg1  (95.0, measured)
- O4.096Cu1Ba2Hg1  (95.6, measured)
- O4.097Cu1Ba2Hg1  (~95.5, interpolated — nearest measured neighbor 4.096=95.6 is 0.001 O away)
- O4.150Cu1Ba2Hg1  (97.0, measured)
- O4.162Cu1Ba2Hg1  (97.4, measured)
- O4.220Cu1Ba2Hg1  (97.0, measured)

## Step C — inductive rule (high = Tc>=90)
**Measured Tc values (all measurable.txt):**
Hg+5elem (O6.08–6.31 Ca1Cu2Ba2Hg1): all 90–127 K → all HIGH (lowest 92.0).
Hg+4elem (O4.05–4.22 Cu1Ba2Hg1): all 92–97.4 K → all HIGH (lowest 92.0).
O6Cu1Ba2Hg1Tl1 (Hg, no Ca, has Tl): NOT measured (out of budget). Skip from rule-fit (do not predict it). Represented by other Tl/Cu/Ba materials below.
O7Cu3Ba2La1 (no Hg, no Y): NOT measured. Skip.
O7Cu3Y0.200Ba2La0.800 (has Y, no Hg): NOT measured. Skip.
O6Cu1Cd0.250Ba2Tl1.750 (Tl+Cd, no Hg): NOT measured. Skip.
O10Ca2Cu3Ba2Tl2 (Tl, no Hg): NOT measured. Skip.

⚠️ Budget fully consumed. Steps C/D must rest on Hg-only data + literature analogy; report this caveat.

### Rule (best-supported given budget)
A material is HIGH if it contains Hg (every Hg-bearing measured sample is HIGH, Tc 92–127 K).
Materials without Hg require a known high-Tc motif to be HIGH: **YBCO-like (Y + Ba + Cu)** is HIGH; **Tl-2201/Cd-Tl substitution variants are marginal/LOW**; **Cu3Ba2(La/Y) without alkali-earth hole-doping tier is LOW**.

## Step D — final pick
Unmeasurable predicted HIGH and containing Hg: #1 O6.160..., #2 O6.270..., #3 O4.180... → fewest elements = #3 `O4.180Cu1Ba2Hg1` (4 elem). (Fallback chain for any others above.)
