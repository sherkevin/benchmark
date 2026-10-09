# Notes

## Goal
- Task 1: Report QED of `m05` (never synthesized → predict from its OWN congeneric series).
- Task 2: Pick one molecule satisfying ALL of: measured QED ≥ 0.6, psa ≥ 40, NOT in same congeneric series as m05.
- Budget: 12 units total, shared by list_pool (1/page) and measure_qed (1/measurement). Probe-unavailable costs nothing.

## Scaffold grouping (from group_by_scaffold, free)
- S1 `COc1ccc(cc1)C(=O)N` (methoxy): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, unsynth)**
- S2 `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- S3 `OCC(O)C(O)C(O)C(=O)N` (sugar): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- S4 `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

m05 is S1. Task-2 candidates must be S2/S3/S4 (any scaffold ≠ S1).

## PSA is constant within a scaffold (alkyl chain adds only carbons/H, no heteroatoms)
Page 1 confirmed: m01(S3) psa 110.02, m03(S3) psa 110.02 (same scaffold → same psa); m02(S2) psa 72.24; m04(S4) psa 29.1 (<40, S4 eliminated).
- S2 (nitro)  psa 72.24  ≥ 40 ✓
- S3 (sugar)  psa 110.02 ≥ 40 ✓
- S4 (biphenyl) psa 29.1 < 40 ✗ (eliminates whole scaffold)
So Task-2 lead must come from S2 or S3.

## Task 1 — m05 QED prediction (from its own S1 series)
Measured S1 members (chain → QED):
- m15 ch2 = 0.7621
- m24 ch3 = 0.7919
- m28 ch4 = 0.7518
- m17 ch5 = 0.7498
- m22 ch6 = 0.7377
Trend rises to ch3 then declines. fit_trend at chain_len=11:
- degree 2 (quadratic): prediction **0.5416**, rmse 0.01188
- degree 1 (linear):    prediction 0.6950,  rmse 0.01308
Quadratic has lower rmse (better fit to the measured points) and a chemically sensible downward
extrapolation: a long C11 lipophilic alkyl chain lowers drug-likeness. Use **target_qed = 0.5416**.

## Task 2 — lead selection
Picked m02 (S2 nitro scaffold, chain 3, drug-like alogp 1.735, psa 72.24).
Measured: **m02 QED = 0.6046** (≥ 0.6 ✓).
Constraints check:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. scaffold S2 ≠ S1 (m05) → not same congeneric series ✓
All three satisfied. No further spend needed.

## Budget accounting
- list_pool page 1: 1
- measure m15, m24, m28, m17, m22 (Task-1 trend): 5
- measure m02 (Task-2 lead): 1
Total used: 7 / 12. Done.
