# Superconductor Tc Study — Notes

## Step A — Series peak (O6*Ca1Cu2Ba2Hg1 series)
All 10 members measured (budget 1-10).

| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (achieved by both O6.210 and O6.220; pick O6.210Ca1Cu2Ba2Hg1 as the peak composition).

## Step B — filter: Hg + exactly 4 elements
threshold1 = T* − 32 = 127.0 − 32 = **95.0 K**

Candidate series in measurable.txt: O4*Cu1Ba2Hg1 (11 members, lines 11-21). Each has exactly 4 elements: O, Cu, Ba, Hg. ✓
Other Hg-containing measurable: O6*Ca1Cu2Ba2Hg1 (5 elements — Ca too), excluded.

Need Tc >= 95.0 K. Measuring all 11 O4* members (budget 11-21).

| Composition | Tc (K) | >=95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ |
| O4.047Cu1Ba2Hg1 | 95.0 | ✓ (exactly at threshold) |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |

**Q1 (7 members, Tc >= 95.0):** O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047 (all Cu1Ba2Hg1).

Budget used: 21 / 24. **3 calls remaining** for Step C rule induction.

## Step C — rule induction

Unmeasurable materials (7):
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212 series (Ca1Cu2). Interpolate in measured O6* series.
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212 series. Interpolate.
3. O4.180Cu1Ba2Hg1 — Hg-1201 series (Cu1, no Ca). Interpolate in O4* series.
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl/Ca/Ba/Cu cuprate. No direct data.
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl/Pb/Sr/Ca/Cu. No direct data.
6. O8Cu3Y1Ba1.900La0.100 — Y-Ba-Cu-La (123-type, Cu3). No direct data.
7. O8Cu4Y1Ba1.870La0.130 — Y-Ba-Cu-La (124-type, Cu4). No direct data.

Interpolations (within measured series — high confidence):
- #1 O6.160: between O6.140(105) & O6.190(122) → 105 + 17*(0.020/0.050) = 111.8 K → HIGH
- #2 O6.270: between O6.240(123) & O6.300(100) → 123 - 23*(0.030/0.060) = 111.5 K → HIGH
- #3 O4.180: between O4.162(97.4) & O4.200(94.2) → 97.4 - 3.2*(0.018/0.038) = 95.9 K → HIGH

For #4-#7, no direct series. Spend last 3 measurements on closest measurable analogs:
- O7Cu3Y0.200Ba2La0.800 → YBCO-123 + La analog (informs #6, #7)
- O10Ca2Cu3Ba2Tl2 → Tl-2223 analog (informs #4)
- O7Cu3Ba2La1 → LaBa2Cu3O7 123-type w/ La (La-substituted 123 context, clean 4-elem data point)

### Analog measurements (budget 22-24)
| Composition | Tc (K) | informs |
|---|---|---|
| O10Ca2Cu3Ba2Tl2 (Tl-2223) | 118.97 | #4 Tl-cuprate family baseline |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | #6/#7 La-substituted 123 context |
| O7Cu3Ba2La1 (La-123, La on Y-site) | 89.6 | #6/#7 La-substitution effect |

### Rule induced (Step C)
Tc is governed by an **interaction of structural family (Cu-layer count / cation layering) with oxygen-doping level**, NOT any single factor:
- Single-factor rules fail: "contains Hg" wrongly flags low-O Hg-1201 (92 K, low); "has 4 elements" wrongly mixes high & low Hg-1201; "has Y+Ba+Cu" wrongly conflates 123-type (high) with 124-type (low).
- Within a fixed family, Tc is a smooth function of O content peaking near optimal doping then declining — so unmeasured members of a measured series interpolate reliably.
- Across families, baseline Tc differs by structure: Hg-1212 ~127, Hg-1201 ~98, Tl-2223 ~119, Tl-2212 ~110, YBCO-123 ~92 (La-sub on Y-site drops to ~81-90), YBCO-124 ~80.

### Predictions for the 7 unmeasurables
1. **O6.160Ca1Cu2Ba2Hg1** → HIGH. Interpolate O6* series: O6.140=105, O6.190=122 → ~111.8 K.
2. **O6.270Ca1Cu2Ba2Hg1** → HIGH. Interpolate: O6.240=123, O6.300=100 → ~111.5 K.
3. **O4.180Cu1Ba2Hg1** → HIGH. Interpolate O4* series: O6.162=97.4, O6.200=94.2 → ~95.9 K (just above 90).
   *(typo: O4.162, O4.200)*
4. **O8Ca0.850Cu2.200Ba2Tl1.800** → HIGH. Tl-2212 structure; measured Tl-2223 analog = 118.97 K; Tl-2212 baseline ~108-110 K. 5 elements.
5. **O7Ca1Cu2Sr2Tl0.500Pb0.500** → LOW. (Tl,Pb)-1212 with Sr (not Ba) + 50/50 Tl/Pb disorder; known Tc ~80 K, below 90. 6 elements.
6. **O8Cu3Y1Ba1.900La0.100** → HIGH. YBCO-123-type (Cu3), full Y on rare-earth site, only 10% Ba→La; very close to pure YBa2Cu3O7 (~92 K). Contrast: La-on-Y-site variant O7Cu3Ba2La1 = 89.6 K (low), Y-depleted O7Cu3Y0.2Ba2La0.8 = 81 K (low) — Ba-site La with full Y keeps Tc ~92. 5 elements.
7. **O8Cu4Y1Ba1.870La0.130** → LOW. YBCO-124-type (Cu4) — 124 structure baseline ~80 K, below 90. 5 elements.

## Step D — final pick
HIGH predictions and element counts:
- O6.160Ca1Cu2Ba2Hg1 → 5
- O6.270Ca1Cu2Ba2Hg1 → 5
- O4.180Cu1Ba2Hg1 → **4** ← fewest
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5
- O8Cu3Y1Ba1.900La0.100 → 5

Only 4-element high = **O4.180Cu1Ba2Hg1**. No tie → final answer.

## Summary
- peak (Step A): O6.210Ca1Cu2Ba2Hg1, T* = 127.0 K
- threshold (Step B): 95.0 K
- Q1 (7): O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047 (all Cu1Ba2Hg1)
- predictions (Step C): see above (5 high, 2 low)
- final (Step D): O4.180Cu1Ba2Hg1
