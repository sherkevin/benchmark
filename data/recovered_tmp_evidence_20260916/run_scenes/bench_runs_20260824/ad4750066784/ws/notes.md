# Notes

## Task understanding
- Task 1: predict QED of m05 (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, chain length 11). m05 unsynthesized → must extrapolate from its OWN congeneric series (methoxy aromatic), varying only N-alkyl chain length.
- Task 2: pick one pool molecule with (a) measured QED ≥ 0.6, (b) psa ≥ 40, (c) NOT in m05's series.
- Budget: 12 measurements total, shared.

## Pool scaffolds (by SMILES inspection)
- Methoxy aromatic `COc1ccc(cc1)C(=O)N-alkyl` (m05's series; psa 38.33):
  - m15 chain2, m24 chain3, m28 chain4, m17 chain5, m22 chain6, m05 chain11 (target)
- Nitro aromatic `O=[N+]([O-])c1ccc(cc1)C(=O)N-alkyl` (psa 72.24): m09(2),m02(3),m12(4),m32(5),m33(6),m29(7),m31(8),m06(8),m10(10)
- Biphenyl `c1ccc(cc1)-c1ccc(cc1)C(=O)N-alkyl` (psa 29.1) → fails psa≥40, skip for Task2
- Sugar `OCC(O)C(O)C(O)C(=O)N-alkyl` (psa 110.02): m14(2),m27(3),m01(4),m19(5),m03(6),m18(7),m21(7),m07(8),m25(9)

## Task 2 candidates (psa≥40 AND not methoxy series)
- Sugar series (psa 110.02): all qualify on psa
- Nitro series (psa 72.24): all qualify on psa
- Need measured QED ≥ 0.6.

## Budget plan
- Task 1: measure methoxy series m15(2),m24(3),m28(4),m17(5),m22(6) = 5 measurements, then fit & extrapolate to chain 11.
- Task 2: remaining ~7 measurements to find a QED≥0.6 candidate in sugar/nitro series.

## Task 1 results (methoxy series)
- chain2 m15=0.7621, chain3 m24=0.7919, chain4 m28=0.7518, chain5 m17=0.7498, chain6 m22=0.7377
- Quadratic fit → predict chain11 (m05) = **0.5416** (rmse 0.0119). Coeffs [-0.003264, 0.017024, 0.74932].

## Task 2 measurements so far
- Nitro: m12(4)=0.4709, m32(5)=0.4683 → nitro capped low (~0.47, aromatic nitro = structural alert)
- Sugar: m14(2)=0.3237, m27(3)=0.333, m01(4)=0.3187 → sugar very low (psa 110 + alogp<<0)
- Gap methoxy-vs-nitro at same chain ≈ 0.28 (chains 4,5) → nitro maxes ~0.50. Below 0.6.
- Used 10/12. 2 left.
- Best remaining shot: nitro SHORT chain (m09 chain2 / m02 chain3) — lowest MW+rotb, most druglike non-methoxy; methoxy peaks at chain3, so check m09 then m02.
- m09 (chain2) = 0.5826 → below 0.6
- m02 (chain3) = **0.6046** → meets QED≥0.6 ✓

## Task 2 decision: m02
- m02 = `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`, nitro series, chain 3
- Measured QED 0.6046 (≥0.6 ✓), psa 72.24 (≥40 ✓), nitro scaffold ≠ methoxy scaffold of m05 (not same series ✓)
- All three constraints satisfied.

## Budget reconciliation: 12/12 used
- Task1: m15,m24,m28,m17,m22 (5)
- Task2: m12,m32,m14,m27,m01,m09,m02 (7)
