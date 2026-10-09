# Task notes

## Goal
- Task 1: predict QED of m05 (never synthesized). m05 = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, methoxy-benzamide, N-alkyl chain length 11. Must predict from its OWN congeneric series (same scaffold, vary chain length).
- Task 2: pick ONE molecule with (a) measured QED ≥ 0.6, (b) psa ≥ 40, (c) NOT in same congeneric series as m05.
- Budget: 12 measurements total, shared.

## Pool analysis (from pool.tsv)
Scaffolds (by SMILES, to confirm with group_by_scaffold):
- Methoxy-benzamide `COc1ccc(cc1)C(=O)N-`: m15(ch2), m24(ch3), m28(ch4), m17(ch5), m22(ch6), m05(ch11, target). psa=38.33, arom=1.
- Nitro-benzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N-`: m09(2),m02(3),m12(4),m32(5),m33(6),m29(7),m31(8),m06(9),m10(10). psa=72.24, arom=1.
- Pentose-amide `OCC(O)C(O)C(O)C(=O)N-`: m14(2),m27(3),m01(4),m19(5),m03(6),m21(7),m18(8),m25(9),m07(10). psa=110.02, arom=0.
- Biphenyl-benzamide `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`: m13(2),m11(3),m04(4),m30(5),m23(6),m20(7),m26(8),m16(9?),m08(9?). psa=29.1, arom=2.

## Task 2 constraints
psa ≥ 40 eliminates methoxy (38.33), biphenyl (29.1) series. Also methoxy = m05's series (excluded anyway).
Candidates: nitro-benzamide (psa 72.24) or pentose-amide (psa 110.02).
Pentose has very negative alogp (-2.8..0.3) -> likely low QED. Nitro has good alogp range (1.3-4.5) for short/mid chains -> best bet.
Most promising nitro members: m12(ch4,alogp2.125), m32(ch5,2.515), m33(ch6,2.905), m02(ch3,1.735), m29(ch7,3.295).

## Task 1 — measurements (methoxy series, m05 = ch11 target)
- m15 ch2: 0.7621
- m24 ch3: 0.7919 (peak)
- m28 ch4: 0.7518
- m17 ch5: 0.7498
- m22 ch6: 0.7377
Quadratic fit (degree 2, all 5 pts, RMSE 0.01188), coeffs [0.74932, 0.017024, -0.003264].
Prediction at ch11: **0.5416**.
Reasoning for quadratic (vs linear extrapolation): series peaks ~ch2-3 then declines; between ch6 and ch11 several QED-desirability thresholds are crossed (alogp 3.0→4.96 near logP cliff, rotb 7→12 past rotb=10 flag) → accelerating concave-down decline, well captured by quadratic. target_qed = 0.5416.

## Task 2 — measurements (nitro series, psa 72.24, not m05's series)
- m33 ch6: 0.4591
- m32 ch5: 0.4683
- m29 ch7: 0.4446
- m12 ch4: 0.4709
- m02 ch3: **0.6046** ← winner (≥0.6)
m02 = `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`, psa=72.24 (≥40 ✓), scaffold nitro-benzamide ≠ m05's methoxy scaffold (✓ not same series).
All three Task-2 constraints satisfied.

## Budget
Used 10/12 measurements.
