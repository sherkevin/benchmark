# Notes

## Setup
- 33-molecule pool, budget = 12 (shared list_pool + measure_qed).
- m05 = target (Task 1): `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, methoxy-benzamide, N-alkyl chain_len = 11. Never synthesized → probe_unavailable.

## Scaffold grouping (free, group_by_scaffold)
4 scaffolds:
1. **methoxy** `COc1ccc(cc1)C(=O)N` — m05's series (EXCLUDED from Task 2):
   m15(2), m24(3), m28(4), m17(5), m22(6), m05(11, unsynth)
2. **nitro** `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2),m02(3),m12(4),m32(5),m33(6),m29(7),m31(8),m06(9),m10(10)
3. **sugar** `OCC(O)C(O)C(O)C(=O)N` — m14(2),m27(3),m01(4),m19(5),m03(6),m21(7),m18(8),m07(9),m25(10)
4. **biphenyl** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2),m11(3),m04(4),m30(5),m23(6),m20(7),m26(8),m08(9),m16(10)

## Key insight
PSA is constant within a scaffold (N-alkyl chain is nonpolar → adds no polar atoms).
So one catalogue entry per scaffold reveals that scaffold's PSA. Page 1 (m01,m02,m03,m04)
covers all 3 candidate scaffolds (sugar=m01/m03, nitro=m02, biphenyl=m04).

## Task 1 plan
Measure QED of m15(2),m24(3),m28(4),m17(5),m22(6), fit chain_len→QED trend, evaluate at 11 = m05.

## Task 2 plan
Need: measured QED ≥ 0.6, psa ≥ 40, not m05's scaffold.
Short-chain nitro-benzamide is the most drug-like candidate (moderate MW/logP, psa = amide+nitro ≥ 40).
