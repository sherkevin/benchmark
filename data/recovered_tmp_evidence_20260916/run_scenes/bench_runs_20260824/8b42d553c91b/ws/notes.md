# Notes

## Setup
- Budget: 12 measurements total, shared between Task 1 and Task 2.
- m05 (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) never synthesized → probe_unavailable. Must predict from its OWN congeneric series (same scaffold, only N-alkyl chain length differs).

## Scaffold grouping (from group_by_scaffold)
Four series:
1. **Methoxy benzamide** `COc1ccc(cc1)C(=O)N` — m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target)**
2. **Nitro benzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N` — m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10). psa=72.24 ✓
3. **Sugar amide** `OCC(O)C(O)C(O)C(=O)N` — m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10). psa=110.02 ✓
4. **Biphenyl benzamide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13..m16. psa=29.1 ✗ fails psa≥40

## Task 2 eligibility
m05's series = methoxy benzamide. Exclude all of group 1.
- Group 4 (biphenyl) fails psa≥40 → excluded.
- Candidates: groups 2 (nitro) and 3 (sugar), all members pass psa≥40.
Need measured QED ≥ 0.6.

## Budget allocation
- Task 1: measure m05's series m15, m24, m28, m17, m22 (5 pts) → fit trend, extrapolate to chain_len=11.
- Task 2: remaining 7 measurements to find a nitro/sugar member with QED≥0.6.

## Measurements

### Task 1 — m05's methoxy series (chain_len → QED)
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377
(5 measurements used)

Trend: gentle peak at chain 3, then steady decline.

### m05 prediction (chain 11, extrapolation)
Fits tested:
- deg-1, all 5 pts: 0.695 (rmse 0.013)
- deg-2, all 5 pts: 0.5416 (rmse 0.012) — quadratic extrapolation 5 units past data, curvature amplifies; less trustworthy
- deg-1, chains 3–6: 0.6508
- deg-1, chains 4–6: 0.7041

All linear fits cluster 0.65–0.70. Chose **deg-1 over all 5 points = 0.695** (best data support, low RMSE, robust across windows). Quadratic is rejected for far extrapolation.

→ target_qed = **0.695**

### Task 2 — nitro series scan (psa 72.24 ≥ 40 ✓, not m05's series)
- m09 (chain 2): 0.5826 — <0.6
- m02 (chain 3): 0.6046 — **≥0.6 ✓**  (3 measurements used)
- m12 (chain 4): 0.4709 — <0.6 (extra, confirms decline)

**Lead = m02**: measured QED 0.6046 ≥ 0.6 ✓, psa 72.24 ≥ 40 ✓, nitro scaffold ≠ methoxy (m05) ✓.
Stopped at 8/12 measurements; m02 already satisfies all three constraints.

## Final answers
- target_qed = 0.695
- lead_mol_id = m02
- lead_qed_measured = 0.6046
