# Notes

## Task overview
- 33 molecules in pool. Budget: 12 measurements total (shared by both tasks).
- Task 1: Report QED of `m05` (never synthesized → must extrapolate from its congeneric series).
- Task 2: Pick ONE molecule with (a) measured QED ≥ 0.6, (b) psa ≥ 40, (c) NOT in m05's congeneric series.

## Scaffold groupings (from group_by_scaffold)
4 scaffolds:
1. `COc1ccc(cc1)C(=O)N` — m05's series (chain_len 2,3,4,5,6,11): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target)**. psa=38.33.
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10). psa=72.24 (≥40 ✓).
3. `OCC(O)C(O)C(O)C(=O)N` (polyol): m14..m25. psa=110.02 (≥40 ✓ but 5 HBD → low QED likely).
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl): m13..m16. psa=29.1 (<40 ✗ excluded for Task 2).

## Plan
- Task 1: measure m05's series members (m15,m24,m28,m17,m22) → extrapolate QED vs chain_len to 11.
- Task 2: measure nitro short-chain candidates until one has QED≥0.6.

## Measurements taken (10 of 12 used)

### Task 1 — m05's congeneric series (scaffold COc1ccc(cc1)C(=O)N), QED vs N-alkyl chain_len
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Extrapolation to chain_len=11 (m05) — primary fit:
- fit_trend degree=2 on all 5 points.
- Quadratic: coefficients [0.74932, 0.017024, -0.003264], rmse 0.01188.
- **Prediction at x=11: 0.5416** → target_qed = 0.5416.

### Cross-check / robustness on m05
- The chain gap to m05 (chain 6 → 11) is 5 steps; pure polynomial extrapolation is
  unstable across fits: linear-5pt 0.695, quad-5pt 0.542, quad-4pt(3..6) 0.938,
  quad-4pt(2..5) 0.269. So I corroborated the estimate mechanistically via QED components.
- m05 descriptors: mw 305.5, alogp 4.956, rotb 12, arom 1, psa 38.33. Its series-constant
  parts (HBA,HBD,PSA,AROM = 0.9305,0.9866,0.933,0.8273) and chain-dependent MW/ALOGP/ROTB
  are well-bracketed by measured members m15..m22. At chain 11, MW (~305) and ROTB (~12)
  fall into the steep-decline tails of their desirability curves, dragging QED down sharply.
- Proxy check: m10 (nitro series, chain 10) has near-identical MW (306.4)/alogp (4.47)/rotb
  (11)/arom (1) to m05, and measured QED 0.3724 — with a *worse* scaffold (nitro ALERTS
  0.0817 vs COc1ccc ALERTS 0.6397 at short chains). So m05 should sit comfortably above m10
  (0.3724) and the quadratic estimate 0.5416 is consistent. Reported value 0.5416.

### Task 2 — nitro series candidates (scaffold != m05's; psa 72.24 ≥ 40)
| mol_id | chain_len | psa   | QED measured | ≥0.6? |
|--------|-----------|-------|--------------|-------|
| m09    | 2         | 72.24 | 0.5826       | ✗     |
| m02    | 3         | 72.24 | 0.6046       | ✓     |
| m12    | 4         | 72.24 | 0.4709       | ✗     |
| m32    | 5         | 72.24 | 0.4683       | ✗     |
| m10    | 10        | 72.24 | 0.3724       | ✗     |

**Picked m02** — satisfies all three constraints:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N`, NOT m05's `COc1ccc(cc1)C(=O)N` ✓

qed_components(m02): MW 0.4975, ALOGP 0.9223, HBA 0.9848, HBD 0.9866, PSA 0.9302,
ROTB 0.9697, AROM 0.8273, ALERTS 0.2414. The nitro ALERTS score is the main drag; otherwise
drug-like. Among nitro chains measured, only chain-3 (m02) clears 0.6, so it is the pick.

## Final answers
- target_qed (m05): 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046

## Budget
Used 10/12: m15,m24,m28,m17,m22 (Task1), m09,m02,m12,m32,m10 (Task2).
