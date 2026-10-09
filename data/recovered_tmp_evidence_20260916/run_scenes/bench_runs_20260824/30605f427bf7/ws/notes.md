# Notes

## Budget
- 17 total. Used 9 listing all 9 pages (33 entries) + 8 measurements = 17/17.

## Catalogue by scaffold series (n = N-alkyl chain length)

### Series A — p-methoxybenzamide `COc1ccc(cc1)C(=O)N(CH2)n`  (m05's series; psa 38.33; arom 1)
| mol | n | MW    | alogp  | rotb | QED     |
|-----|---|-------|--------|------|---------|
| m15 | 2 | 179.2 | 1.445  | 3    | 0.7621  |
| m24 | 3 | 193.3 | 1.835  | 4    | 0.7919  | ← peak
| m28 | 4 | 207.3 | 2.225  | 5    | 0.7518  |
| m17 | 5 | 221.3 | 2.615  | 6    | 0.7498  |
| m22 | 6 | 235.3 | 3.005  | 7    | 0.7377  |
| m05 |11| 305.5 | 4.956  | 12   | ??? (unsynth → extrapolate)

### Series B — p-nitrobenzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N(CH2)n`  (psa 72.24; arom 1) — NOT m05 series
| mol | n | QED     | note                |
|-----|---|---------|---------------------|
| m09 | 2 | 0.5826  | <0.6 ✗              |
| m02 | 3 | 0.6046  | ≥0.6 ✓ ← PICK       |
| m12 | 4 | 0.4709  | <0.6 ✗              |
(m32 C5, m33 C6, m29 C7, m31 C8, m06 C9, m10 C10: not measured)

### Series C — biphenyl-4-carboxamide (psa 29.1 < 40) — fails constraint 2. Not usable.
### Series D — sugar amide `OCC(O)C(O)C(O)C(=O)N(CH2)n` (psa 110.02; alogp<0; arom 0) — backup, not measured.

## Task 1 — m05 QED extrapolation
Series-A QED: n=2→0.7621, n=3→0.7919 (peak), n=4→0.7518, n=5→0.7498, n=6→0.7377.
Plateau around n=3-5 (~0.75-0.79), then steady decline. m05 (n=11): MW 305.5,
alogp 4.956, rotb 12. Within a congeneric series only MW/alogp/rotb change;
PSA(38.33), arom(1), rings are constant.

Data-driven extrapolation: linear regression of QED vs n on the post-peak
points n=3,4,5,6 (0.7919, 0.7518, 0.7498, 0.7377):
  slope = -0.0165/n, intercept = 0.8319
  QED(n=11) = 0.8319 - 0.0165*11 = 0.6504  ≈ 0.65

Cross-checks:
- 2-point slope n=4→6 (-0.0141/n): 0.7377 - 5*0.0141 ≈ 0.667.
- Weight-function reasoning: alogp 3→5 ~halves d_alogp; rotb 7→12 drops d_rotb
  to ~0.65. Weighted-geometric-mean effect (Bickerton weights) gives
  QED_m05/QED_m22 ≈ 0.88 → ~0.65. Consistent.
- m05 sits at rotb>10 and alogp≈5 QED penalty bands, so the true value could
  dip slightly below the linear estimate; but a simple alkyl chain likely
  triggers no discrete QED alert, so no extra cliff. Central estimate ~0.65.
→ Predicted m05 QED ≈ 0.65.

## Task 2 — lead
m02 (p-nitrobenzamide, n=3): QED 0.6046 (measured) ≥ 0.6 ✓; psa 72.24 ≥ 40 ✓;
not m05's congeneric series (nitro-benzamide vs methoxy-benzamide) ✓.
All three constraints satisfied. PICK m02.
