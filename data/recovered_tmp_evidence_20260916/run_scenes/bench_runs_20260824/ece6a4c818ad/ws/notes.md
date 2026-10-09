# Notes

## Scaffold Grouping (from group_by_scaffold)

4 scaffolds identified:
1. **Scaffold A** `COc1ccc(cc1)C(=O)N` — m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. **Scaffold B** `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. **Scaffold C** `OCC(O)C(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. **Scaffold D** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1: Predicting QED of m05

m05 is in Scaffold A, chain_len=11. It has never been synthesized.

### Measurements of Scaffold A members:
| mol_id | chain_len | QED (measured) |
|--------|-----------|-----------------|
| m15    | 2         | 0.7621          |
| m24    | 3         | 0.7919          |
| m28    | 4         | 0.7518          |
| m17    | 5         | 0.7498          |
| m22    | 6         | 0.7377          |

### QED Component Trends:
As chain length grows (2→6):
- MW desirability: 0.33 → 0.68 (improving)
- ALOGP desirability: 0.87 → 1.00 (improving, but m05 alogp=4.956 may overshoot)
- ROTB desirability: 0.99 → 0.53 (rapidly declining; m05 has rotb=12)
- ALERTS desirability: 0.84 → 0.64 (dropped then stable)

### Polynomial Fits at chain_len=11:
- Degree 2, all 5 points: **0.5416** (RMSE=0.0119) ← chosen
- Degree 1, all 5 points: 0.695

The degree-2 fit captures the peak near chain 3 and accelerating decline from compounding ROTB/ALOGP penalties.

## Task 2: Lead Molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, not in Scaffold A.

Scaffold D has PSA=29.1 (too low). Scaffolds B (PSA=72.24) and C (PSA=110.02) qualify.

**m02** (Scaffold B, chain 3, PSA=72.24): measured QED = 0.6046
- QED ≥ 0.6 ✓ | PSA ≥ 40 ✓ | Different scaffold from m05 ✓

## Budget: 2 pages + 6 measurements = 8 of 12 used
