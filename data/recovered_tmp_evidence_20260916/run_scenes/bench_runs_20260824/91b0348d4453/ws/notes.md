# Task notes

## Budget: 12 measure_qed calls total (shared by Task 1 & Task 2). probe_unavailable costs nothing.

## Pool analysis (congeneric series by scaffold)

Scaffold = `Xc1ccc(cc1)C(=O)N` + N-alkyl chain of variable length, OR a sugar head `OCC(O)C(O)C(O)C(=O)N` + chain.

### m05's series: METHOXY-benzamide `COc1ccc(cc1)C(=O)N` (psa=38.33, arom=1)
- m15  C2   mw179  alogp1.445  rotb3
- m24  C3   mw193  alogp1.835  rotb4
- m28  C4   mw207  alogp2.225  rotb5
- m17  C5   mw221  alogp2.615  rotb6
- m22  C6   mw235  alogp3.005  rotb7
- m05  C11  mw305  alogp4.956  rotb12  <-- TARGET, unsynthesizable

m05 excluded from Task 2 (same series) AND psa 38.33 < 40 anyway.

### NITRO-benzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa=72.24, arom=1) -- psa>=40 OK
- m09 C1 alogp1.345 rotb3 ; m02 C2 alogp1.735 rotb4 ; m12 C4 alogp2.125 rotb5
- m32 C5 alogp2.515 rotb6 ; m33 C6 alogp2.905 rotb7 ; m29 C7 alogp3.295 rotb8
- m31 C8 alogp3.685 rotb9 ; m06 C9 alogp4.075 rotb10 ; m10 C10 alogp4.465 rotb11

### BIPHENYL-benzamide `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa=29.1) -- psa<40, excluded
### SUGAR `OCC(O)C(O)C(O)C(=O)N` (psa=110.02, arom=0) -- psa>=40 OK, but very polar/high PSA

## Strategy
- Task 1: measure methoxy series (C2,C4,C5,C6 = m15,m28,m17,m22) to get QED vs chain-length trend, extrapolate to C11 (m05).
- Task 2: search nitro series (psa 72 qualifies) for QED>=0.6, prefer mid-chain (alogp near 2.5-3.2 peak, lower rotb): m32,m12,m02,m33,m09. Fallback sugar.

## Budget plan: 4 (methoxy) + 8 (Task2 search) = 12.

## Task 1 measurements (methoxy series = m05's congeneric series)
- m15 C2: QED 0.7621
- m28 C4: QED 0.7518
- m17 C5: QED 0.7498
- m22 C6: QED 0.7377
Trend: gentle decline with chain length (psa & arom constant across series; QED varies via mw/alogp/rotb).

Extrapolation to m05 (C11, alogp 4.956, rotb 12 — both in penalized regime):
- Linear fit on (chain_C, QED): slope -0.00564/C, intercept 0.7743 → at C11 = 0.712
- Quadratic fit: 0.670 (captures nonlinear alogp/rotb penalty)
- Descriptor reasoning: alogp 3.0→5.0 (ideal→penalized) + rotb 7→12 (ideal→penalized).
  Ratio of geometric-mean factors vs m22 (0.7377): ~0.40-0.56 → QED ≈ 0.66-0.69.
  Linear OVERestimates (ignores nonlinear penalty). Quadratic + descriptor reasoning agree ~0.67.
=> target_qed (m05) = 0.67

## Task 2 candidates with psa>=40
- Nitro series (psa 72.24, all arom=1): m09,m02,m12,m32,m33,m29,m31,m06,m10
  sweet-spot alogp ~2-3: m12(C4,alogp2.125,rotb5), m32(C5,alogp2.515,rotb6), m33(C6,alogp2.905,rotb7)
  RISK: nitro may trigger QED toxicophore alert (lowers QED).
- Sugar series (psa 110.02): alogp all negative/near0 → penalized, likely <0.6. Fallback only.
Measure nitro sweet-spot first: m33, m32, m12.

## Task 2 — nitro series results (psa 72.24, qualifies psa>=40)
- m33 C6 (alogp2.905,rotb7): QED 0.4591
- m32 C5 (alogp2.515,rotb6): QED 0.4683
- m12 C4 (alogp2.125,rotb5): QED 0.4709
=> nitro capped ~0.46-0.47, well below 0.6. Calib check vs methoxy m22(C6,QED0.7377):
m22 vs m33 differ mainly psa(38.33 vs 72.24) + nitro group. ~10x geometric-mean drop.
Nitro series DEAD for Task 2 (even shortest chains won't reach 0.6 from a 0.47 plateau).

## Budget: 7/12 used, 5 left.
## Remaining psa>=40 candidates: SUGAR series (psa 110.02, arom=0), variable chain.
  Best alogp/rotb tradeoffs: m03(C6,alogp-1.242,rotb9,mw249), m21(C7,alogp-0.852,rotb10,mw263),
  m14(C2,alogp-2.802,rotb5,mw193). Measure m03, m21, m14 to map trend.
  If nitro is an alert-cap (and psa penalty mild), sugar (no nitro) may clear 0.6.

## Task 2 — sugar series results (psa 110.02)
- m03 C6 (alogp-1.242,rotb9): QED 0.3282
- m21 C7 (alogp-0.852,rotb10): QED 0.3297
- m14 C2 (alogp-2.802,rotb5): QED 0.3237
=> sugar capped ~0.33, even worse. High psa(110)+arom0 kills it.

## Budget: 10/12 used, 2 left.

### Key finding: BOTH psa>=40 scaffolds are capped BELOW 0.6 by chain-INDEPENDENT factors
- Nitro C4-C6: 0.4709, 0.4683, 0.4591  (flat plateau ~0.47 -> nitro group / psa-72 cap)
- Sugar C2,C6,C7: 0.3237, 0.3282, 0.3297 (flat plateau ~0.33 -> psa-110 cap)
Flatness => hard cap from scaffold, NOT chain length. Shortening chain can't lift it to 0.6.
Constraint-3-eligible scaffolds (nitro, sugar) all fail constraint 1.
Methoxy (high QED ~0.75) fails constraint 2 (psa 38.33<40) AND is m05's series.
Biphenyl fails constraint 2 (psa 29.1<40).
=> NO pool molecule satisfies all three Task-2 constraints simultaneously.

### Last 2 budget: verify nitro cap at shortest, most drug-like chains
Measure m09 (C1, alogp1.345, rotb3) & m02 (C2, alogp1.735, rotb4) — if ANY nitro hits 0.6, these.
Expected ~0.47 (cap). If so, report best nitro as lead (honest best-effort; real measured QED).

## Task 2 — BREAKTHROUGH at short nitro chains (last 2 measurements)
- m09 C1 (alogp1.345, rotb3): QED 0.5826  (just under 0.6)
- m02 C2 (alogp1.735, rotb4): QED 0.6046  *** ≥ 0.6 ***

=> nitro QED vs chain length is NOT flat: rises as chain SHORTENS. My C4-C6 plateau
   was the descending tail; short chains reach 0.6. Calib: m22(methoxy,C6)=0.7377,
   m02(nitro,C2)=0.6046 -> nitro penalty ~0.13 (group+psa), not catastrophic.

## Task 2 LEAD = m02
SMILES O=[N+]([O-])c1ccc(cc1)C(=O)NCCC. Satisfies ALL THREE constraints:
1. QED 0.6046 >= 0.6  (real measurement, budget #12)
2. psa 72.24 >= 40
3. NOT in m05's series: m05 = METHOXY-benzamide (COc1ccc(cc1)C(=O)N),
   m02 = NITRO-benzamide (O=[N+]([O-])c1ccc(cc1)C(=O)N). Different head-group
   on the aryl ring -> different scaffold. (Even if same N-alkyl length C2, the
   scaffold differs, so not "same congeneric series".)

## Budget: 12/12 EXACTLY exhausted. No further measurements possible.

## FINAL ANSWERS
- Task 1 target_qed (m05, predicted from methoxy series trend) = 0.67
- Task 2 lead_mol_id = m02, lead_qed_measured = 0.6046
