# Notes

## Setup
- Budget: 12 measurements total, shared by Job 1 and Job 2.
- m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — scaffold `COc1ccc(cc1)C(=O)N`, N-alkyl chain length = 11. Never synthesized → `probe_unavailable`. Must extrapolate from its OWN series.

## m05's congeneric series (same scaffold `COc1ccc(cc1)C(=O)N`)
From `describe_structure` and SMILES:
| mol_id | chain_len | mw    | alogp  | psa   | rotb | arom |  QED   |
|--------|-----------|-------|--------|-------|------|------|--------|
| m15    | 2         | 179.22| 1.445  | 38.33 | 3    | 1    | 0.7621 |
| m24    | 3         | 193.25| 1.835  | 38.33 | 4    | 1    | 0.7919 |
| m28    | 4         | 207.27| 2.225  | 38.33 | 5    | 1    | 0.7518 |
| m17    | 5         | 221.30| 2.615  | 38.33 | 6    | 1    | 0.7498 |
| m22    | 6         | 235.33| 3.005  | 38.33 | 7    | 1    | 0.7377 |
| m05    | 11 (target) | 305.46 | 4.956 | 38.33 | 12 | 1   | (predict) |

Measured range = chain 2–6. Target = chain 11. → **Extrapolation** well beyond measured range (nearly 2× max).

Within this series, as chain grows: PSA, AROM, HBD/HBA, alerts all constant; only MW↑, alogp↑, rotb↑ change. Expect QED to rise toward a peak then possibly decline as alogp/rotb overshoot.

## Job 1 — trend fit (m05 series)
Measured QED: chains 2,3,4,5,6 = 0.7621, 0.7919, 0.7518, 0.7498, 0.7377. Curve peaks at chain 3 then declines. Not linear.
Quadratic fit through (2,0.7621)(3,0.7919)(4,0.7518)(5,0.7498)(6,0.7377), query x=11:
- coeffs (low→high): [0.74932, 0.017024, -0.003264]
- RMSE on given points: 0.01188
- **prediction at chain 11 = 0.5416**
Linear fit (for reference): 0.695, RMSE 0.01308. But task explicitly says do NOT assume a straight line; the data clearly curves downward, so quadratic is the honest choice. → **target_qed = 0.5416**

## Other series in the pool (for Job 2 exclusion / lead search)
- Nitro series, scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`, psa=72.24: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- Sugar series, scaffold `OCC(O)C(O)C(O)C(=O)N`, psa=110.02: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- Biphenyl series, scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`, psa=29.1: m13, m11, m04, m30, m23, m20, m26, m08, m16 — **all psa<40 → excluded from Job 2**

## Job 2 — lead search
Free filters first (psa≥40, not m05 series). m05 series has psa=38.33<40 so automatically excluded; no overlap. Candidates = nitro (psa 72.24) + sugar (psa 110.02). screen_candidates confirms 18 pass psa≥40 (all nitro + sugar).

QED must be measured (constraint 1, ≥0.6). Measurements used (budget 12 total; 5 spent on Job 1, leaving 7 for Job 2):

| mol_id | series | chain | psa   | alogp | QED measured | ≥0.6? |
|--------|--------|-------|-------|-------|--------------|-------|
| m33    | nitro  | 6     | 72.24 | 2.905 | 0.4591       | no    |
| m29    | nitro  | 7     | 72.24 | 3.295 | 0.4446       | no    |
| m32    | nitro  | 5     | 72.24 | 2.515 | 0.4683       | no    |
| m12    | nitro  | 4     | 72.24 | 2.125 | 0.4709       | no    |
| m09    | nitro  | 2     | 72.24 | 1.345 | 0.5826       | no (closest) |
| m27    | sugar  | 3     |110.02 |-2.412 | 0.3330       | no    |
| m14    | sugar  | 2     |110.02 |-2.802 | 0.3237       | no    |

Budget exhausted at 12/12. None of the 7 Job-2 candidates measured ≥0.6. Highest measured was m09 = 0.5826 (still below the gate). The remaining unmeasured candidates (m02, m31, m06, m10 nitro; m01, m19, m03, m21, m18, m25, m07 sugar) could not be tested within budget.

## Lead decision
Constraint 1 (measured QED ≥ 0.6) cannot be certified for any molecule — no candidate I measured cleared 0.6, and the budget is spent. The molecule with the highest measured QED among the valid-lead pool is **m09 (0.5826)**: it passes the free constraints (psa 72.24≥40; nitro scaffold ≠ m05's methoxyphenyl series). It is the best-available lead even though it falls short of the 0.6 gate. Choosing m09 as lead and reporting its true measured QED = 0.5826 (citing only real measure_qed calls). I note explicitly that this lead does NOT satisfy constraint 1; it is the strongest candidate the budget permitted.
