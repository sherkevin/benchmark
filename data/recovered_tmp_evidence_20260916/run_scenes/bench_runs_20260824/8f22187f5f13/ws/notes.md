# Task notes

## Setup
- 33 molecules, 4 scaffolds (from `group_by_scaffold`, free):
  - **A** `COc1ccc(cc1)C(=O)N` (methoxy benzamide): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)** ← m05's series (TARGET)
  - **B** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro benzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
  - **C** `OCC(O)C(O)C(O)C(=O)N` (sugar-like): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
  - **D** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl benzamide): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Budget: 12 units shared by list_pool + measure_qed.

## Plan
- Task 1: measure all 5 synthesized members of m05's series (chain 2,3,4,5,6) → fit trend → extrapolate to chain 11.
- Task 2: need QED≥0.6, PSA≥40, NOT in series A.

## PSA reasoning by scaffold (secondary amide -C(=O)NH-alkyl ≈ 29 TPSA)
- A methoxy benzamide: amide(29)+methoxy(9)≈38 → but EXCLUDED (m05's series)
- B nitro benzamide: amide(29)+nitro(45)≈74 → ≥40 ✓ (likely QED ok)
- C sugar: amide(29)+4×OH(80)≈109 → ≥40 ✓ but very polar → QED likely LOW
- D biphenyl benzamide: amide(29) only ≈29 → <40 ✗

→ Scaffold B (short chain) is the best lead candidate for Task 2.

## Task 1 — predict m05 QED (chain 11, scaffold A)

Measured series-A members (chain → QED):
| chain | mol | QED   | MW     | ALOGP  | ROTB   | ALERTS |
|-------|-----|-------|--------|--------|--------|--------|
| 2     | m15 | 0.7621| 0.3319 | 0.8736 | 0.9927 | 0.8425 |
| 3     | m24 | 0.7919| 0.4062 | 0.9363 | 0.9697 | 0.8425 |
| 4     | m28 | 0.7518| 0.4914 | 0.9768 | 0.8476 | 0.6397 |
| 5     | m17 | 0.7498| 0.5853 | 0.9973 | 0.6885 | 0.6397 |
| 6     | m22 | 0.7377| 0.6836 | 0.9973 | 0.5332 | 0.6397 |

Constant (chain-independent) components: HBA=0.9305, HBD=0.9866, PSA=0.933, AROM=0.8273.

Extrapolation to chain 11:
- QED overall, quadratic fit: 0.5416 (RMSE 0.0119)
- QED overall, linear fit: 0.695 (RMSE 0.0131) — too optimistic for long chain
- ROTB quadratic → -1.0463 → floor at 0 (long flexible chain, rotatable bonds max out)
- ALOGP quadratic → 0.684 (peaked at chain 5, declining)
- MW quadratic → 1.31 → cap at 1

Mechanism: at chain 11, ROTB collapses (~0) and ALOGP declines past peak; MW caps high.
Both direct-fit (0.5416) and component-recombination (≈0.5) converge → **target_qed = 0.54**.

## Task 2 — pick lead

Constraints: QED≥0.6, PSA≥40, NOT series A.

Strategy: short-chain member of scaffold B (nitro benzamide, PSA≈72). Measured two:
- m02 (chain 3): QED = 0.6046 ≥ 0.6 ✓
- m09 (chain 2): QED = 0.5826 ✗

Confirmed m02 PSA via list_pool page 1: psa = 72.24 ≥ 40 ✓.
m02 scaffold = B (`O=[N+]([O-])c1ccc(cc1)C(=O)N`), m05 scaffold = A → different series ✓.

**Lead = m02, measured QED = 0.6046.**

## Budget
Used: 5 (series A measurements) + 2 (m02, m09) + 1 (list_pool page 1) = 8 / 12.
