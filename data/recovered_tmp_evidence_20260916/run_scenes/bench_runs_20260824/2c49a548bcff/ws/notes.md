# Work notes

## Setup
- Pool: 33 molecules, 4 scaffolds (from `group_by_scaffold`, free).
- Budget: 12 units total, shared by `list_pool` (1/page) and `measure_qed` (1/meas).

## Scaffolds (structural only)
- **A** `COc1ccc(cc1)C(=O)N` (methoxy-phenyl): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, unsynth)**
- **B** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-phenyl): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **C** `OCC(O)C(O)C(O)C(=O)N` (sugar): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **D** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl): m13..m16 etc. — **psa 29.1 < 40**, ruled out for Task 2.

## Task 1 — QED of m05 (scaffold A, chain 11, unsynthesizable)
Measured the 5 synthesizable members of scaffold A (chains 2-6):

| chain | mol_id | QED   |
|-------|--------|-------|
| 2     | m15    | 0.7621|
| 3     | m24    | 0.7919|  <- peak
| 4     | m28    | 0.7518|
| 5     | m17    | 0.7498|
| 6     | m22    | 0.7377|

Trend (QED vs chain length), predict at chain = 11:
- deg-2, all 5 pts, RMSE 0.012 -> 0.5416 (parabola accelerates down; over-extrapolates the decline)
- deg-1, all 5 pts, RMSE 0.013 -> 0.6950 (slope flattened by the chain2->3 rise)
- **deg-1, chains 3-6, RMSE 0.009 (best fit; the actual monotonic declining region) -> 0.6508**

Chose the deg-1 fit on chains 3-6: lowest RMSE, reflects the genuine recent downward trend,
avoids the quadratic's downward acceleration artifact. Report **target_qed = 0.6508**.

Sanity: QED of m22 (chain 6) is limited by ROTB=0.533 and ALERTS=0.640 (`qed_components`);
both worsen with longer chains, so a continued decline to ~0.65 at chain 11 is physically sensible.

## Task 2 — pick one molecule: QED>=0.6, psa>=40, NOT in m05's series (scaffold A)
m05's series = scaffold A, so eligible scaffolds: B (nitro, psa~72) and C (sugar, psa~110).
D (biphenyl) excluded: psa 29.1 < 40.

Measured candidates (budget tracked):
- m09 (B, ch2): 0.5826  -> <0.6, fail
- m14 (C, ch2): 0.3237  -> fail
- **m02 (B, ch3): 0.6046**  -> >=0.6 PASS
- m27 (C, ch3): 0.3330  -> fail

m02 verified on all three constraints:
1. measured QED = 0.6046 >= 0.6  ✓
2. psa = 72.24 >= 40  (catalogue, page 1)  ✓
3. scaffold B (nitro-phenyl) != scaffold A (m05's methoxy-phenyl) -> different series  ✓

-> lead_mol_id = m02, lead_qed_measured = 0.6046.

## Budget
Used 11/12: 5 (scaffold A meas) + 1 (page 4) + 4 (m09,m14,m02,m27) + 1 (page 1).
 qed_components and fit_trend and group_by_scaffold are free.
1 unit left unused; m02 already satisfies all constraints, so no need to spend it.
