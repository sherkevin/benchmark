# Task notes — predict m05 QED and pick a backup lead

## Data layout

33 molecules in `data/pool.tsv`. Five congeneric series, each a fixed aryl-C(=O)-N scaffold with only the N-alkyl chain length varying:

1. **Methoxy-phenyl amide** `COc1ccc(cc1)C(=O)N…` — psa 38.33 (this is m05's series)
   - m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6), **m05 (C11, TARGET)**
2. **Nitro-phenyl amide** `O=[N+]([O-])c1ccc(cc1)C(=O)N…` — psa 72.24
   - m09 (C2), m02 (C3), m12 (C4), m32 (C5), m33 (C6), m29 (C7), m31 (C8), m06 (C9), m10 (C10)
3. **Sugar(ish) amide** `OCC(O)C(O)C(O)C(=O)N…` — psa 110.02
   - m14 (C2), m27 (C3), m01 (C4), m19 (C5), m03 (C6), m21 (C7), m18 (C8), m07 (C9), m25 (C10)
4. **Biphenyl amide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N…` — psa 29.1
   - m13 (C2), m11 (C3), m04 (C4), m30 (C5), m23 (C6), m20 (C7), m26 (C8), m08 (C9), m16 (C10)

Series membership decided purely from the SMILES (the aryl-`C(=O)N` head group); the tail `C`/`CCC` length is the only other variable.

## Job 1 — predict m05 QED

m05 is `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` = the methoxy-phenyl amide with an 11-carbon N-alkyl chain (rotb=12 ⇒ 11 rotatable tail bonds). The pool holds only C2–C6 members of this series, so the prediction is an extrapolation past the measured range.

Measured (5 of budget):
| chain | mol | QED |
|------:|-----|------|
| C2 | m15 | 0.7621 |
| C3 | m24 | 0.7919 |
| C4 | m28 | 0.7518 |
| C5 | m17 | 0.7498 |
| C6 | m22 | 0.7377 |

QED rises C2→C3 then falls. Not a line; fit a degree-2 polynomial.

`fit_trend(xs=[2,3,4,5,6], ys=[0.7621,0.7919,0.7518,0.7498,0.7377], x_query=11, degree=2)`:
- coefficients (low→high): 0.74932, 0.017024, -0.003264
- QED(n) ≈ 0.74932 + 0.017024·n − 0.003264·n²
- prediction at n=11: **0.5416** (RMSE on fitted points 0.0119)

So extrapolating the observed parabola, m05's predicted QED ≈ **0.542**.

## Job 2 — backup lead

Constraints (all must hold): measured QED ≥ 0.6; psa ≥ 40; not in m05's methoxy-phenyl series.

- m05's own series (methoxy-phenyl, psa 38.33) and the biphenyl series (psa 29.1) both fail the psa ≥ 40 gate for every member. Excluded.
- Leaves the nitro-phenyl series (psa 72.24) and the sugar series (psa 110.02). Short chains with logP near 2 and few rotatable bonds tend to score best, so I prioritized the short nitro-phenyl members (good alogp ~1.7–2.1, low rotb).

Measured (3 more, total 8 of 12):
| mol | series | chain | QED | ≥0.6? |
|-----|--------|------|------|------|
| m09 | nitro | C2 | 0.5826 | no |
| m02 | nitro | C3 | 0.6046 | **yes** |
| m12 | nitro | C4 | 0.4709 | no |

**m02** clears all three:
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- nitro-phenyl scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's methoxy-phenyl scaffold ✓

Lead = **m02**, measured QED = **0.6046**.

## Deliverable values

- target_qed (m05, predicted): 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids (m05's series, pool members only — m05 itself excluded as unmade): m15, m24, m28, m17, m22
