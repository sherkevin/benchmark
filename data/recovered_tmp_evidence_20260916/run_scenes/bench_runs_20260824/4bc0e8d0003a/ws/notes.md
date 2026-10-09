# Notes

## Scaffold Grouping (free)
4 scaffolds identified:
1. `COc1ccc(cc1)C(=O)N` — m05's series: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predict QED of m05 (chain_len=11, scaffold 1)

Measured all 5 synthesized members of scaffold 1:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Confirmed m05 returns probe_unavailable (free).

Degree-2 polynomial fit (RMSE = 0.01188): predicts QED = **0.5416** at chain_len=11.
Degree-1 linear fit (RMSE = 0.01308): predicts QED = 0.695 at chain_len=11.

Chose degree-2 fit because:
- Lower RMSE on training data
- m05 has alogp=4.956 (high) and rotb=12 (high) — both penalize QED non-linearly
- Quadratic decline at long chain lengths is physically motivated by QED's logistic penalty functions

## Task 2: Find lead molecule (QED >= 0.6, PSA >= 40, not scaffold 1)

PSA analysis by scaffold:
- Scaffold 2: PSA = 72.24 (pass)
- Scaffold 3: PSA = 110.02 (pass)
- Scaffold 4: PSA = 29.1 (fail - below 40)

Measured candidates:
| mol_id | scaffold | chain | QED    | PSA     | Qualifies? |
|--------|----------|-------|--------|---------|------------|
| m09    | 2        | 2     | 0.5826 | 72.24   | No (QED < 0.6) |
| m02    | 2        | 3     | 0.6046 | 72.24   | YES |
| m12    | 2        | 4     | 0.4709 | 72.24   | No (QED < 0.6) |
| m14    | 3        | 2     | 0.3237 | 110.02  | No (QED < 0.6) |

**Selected: m02** — QED=0.6046 >= 0.6, PSA=72.24 >= 40, scaffold 2 != scaffold 1.

## Budget Usage
- 5 measurements (scaffold 1): m15, m24, m28, m17, m22
- 4 measurements (Task 2 candidates): m09, m02, m14, m12
- 1 free: m05 (probe_unavailable)
- 2 page listings: page 1, page 2
- Total: 11/12 budget used (+ 1 free probe_unavailable)
