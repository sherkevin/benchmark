# Notes

## Setup
- Pool: 33 molecules, 4 scaffolds (group_by_scaffold):
  1. methoxyphenyl `COc1ccc(cc1)C(=O)N` — m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)  <- m05's series
  2. nitrophenyl  `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10); psa=72.24
  3. sugar        `OCC(O)C(O)C(O)C(=O)N` — m14(2)..m25(10); psa=110.02
  4. biphenyl     `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2)..m16(10); psa=29.1 -> fails Task2 psa>=40
- Budget: 12 total, shared. Used 10.

## Task 1: QED of m05 (methoxyphenyl, chain 11, never synthesized -> probe_unavailable)
Predict from OWN series trend. Measured methoxyphenyl members (chains 2-6; none between 6 and 11 -> extrapolate):
| chain | mol | qed |
|---|---|---|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

fit_trend deg-2 on chains 2-6, eval at 11 -> 0.5416 (rmse on fit pts 0.0119).
Coefs (low->high): [0.74932, 0.017024, -0.003264]. Downward curvature continues to chain 11.
=> target_qed = 0.5416. (Far extrapolation; uncertainty larger than within-range rmse.)

## Task 2: pick molecule with QED>=0.6, psa>=40, NOT methoxyphenyl series.
Eligible scaffolds by psa: nitrophenyl (72.24), sugar (110.02). Biphenyl fails psa.
Measurements:
- nitrophenyl: m09(2)=0.5826, m02(3)=0.6046, m12(4)=0.4709, m32(5)=0.4683 -> peaks ~chain3, declines after
- sugar: m14(2)=0.3237 -> low (very negative alogp); declines further with chain
=> Lead: m02 (nitrophenyl, chain 3), measured QED = 0.6046.
  - QED 0.6046 >= 0.6  ✓
  - psa 72.24 >= 40    ✓
  - nitrophenyl != methoxyphenyl (m05's series) ✓

## Final
- target_qed = 0.5416 (predicted, m05 unsynthesized)
- lead_mol_id = m02
- lead_qed_measured = 0.6046
