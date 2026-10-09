# Notes

## Pool structure (from group_by_scaffold, free)

4 scaffold groups, 33 molecules total.

**Group A — scaffold `COc1ccc(cc1)C(=O)N`** (this is m05's series):
- m15 chain 2, m24 chain 3, m28 chain 4, m17 chain 5, m22 chain 6, m05 chain 11  ← TARGET

**Group B — scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`:**
- m09 c2, m02 c3, m12 c4, m32 c5, m33 c6, m29 c7, m31 c8, m06 c9, m10 c10

**Group C — scaffold `OCC(O)C(O)C(O)C(=O)N`:**
- m14 c2, m27 c3, m01 c4, m19 c5, m03 c6, m21 c7, m18 c8, m07 c9, m25 c10

**Group D — scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`:**
- m13 c2, m11 c3, m04 c4, m30 c5, m23 c6, m20 c7, m26 c8, m08 c9, m16 c10

## PSA by scaffold (from list_pool pages 1-2)
- Group A: psa ~38.33 (m05 itself)
- Group B: psa ~72.24 (m02, m06)  → satisfies psa>=40 ✓
- Group C: psa ~110.02 (m01, m03, m07) → satisfies psa>=40 ✓ BUT alogp very negative → low QED likely
- Group D: psa ~29.1 (m04, m08) → FAILS psa>=40 ✗

→ Task 2 candidates must come from Group B (preferred) or C.

## Catalogue descriptors
- m05 (target, Group A, chain 11): mw 305.46, alogp 4.956, psa 38.33, rotb 12, arom 1
- m02 (Group B, chain 3): mw 208.22, alogp 1.735, psa 72.24, rotb 4, arom 1
- m06 (Group B, chain 9): mw 292.38, alogp 4.075, psa 72.24, rotb 10, arom 1
- m01 (Group C, chain 4): mw 221.25, alogp -2.022, psa 110.02, rotb 7, arom 0

## Measurements log (budget 12 total; list_pool pages cost 1 each)
| mol_id | group | chain | QED    | budget_used |
|--------|-------|-------|--------|-------------|
| (page1)| -     | -     | -      | 1           |
| (page2)| -     | -     | -      | 2           |
| m15    | A     | 2     | 0.7621 | 3           |
| m28    | A     | 4     | 0.7518 | 4           |
| m17    | A     | 5     | 0.7498 | 5           |
| m22    | A     | 6     | 0.7377 | 6           |
| m02    | B     | 3     | 0.6046 | 7           |
| m24    | A     | 3     | 0.7919 | 8           |
| m09    | B     | 2     | 0.5826 | 9           |
| m12    | B     | 4     | 0.4709 | 10          |

Final budget_used = 10 (2 pages + 8 measures). 2 units spare.

## QED components (free) — Group A
| mol | chain | rotb | QED    | MW    | ALOGP | ROTB  | ALERTS | (others constant) |
|-----|-------|------|--------|-------|-------|-------|--------|--------------------|
| m15 | 2     | 3    | 0.7621 | 0.332 | 0.874 | 0.993 | 0.843  | HBA0.931 HBD0.987 PSA0.933 AROM0.827 |
| m24 | 3     | 4    | 0.7919 | 0.406 | 0.936 | 0.970 | 0.843  | " |
| m28 | 4     | 5    | 0.7518 | 0.491 | 0.977 | 0.848 | 0.640  | " |
| m17 | 5     | 6    | 0.7498 | 0.585 | 0.997 | 0.689 | 0.640  | " |
| m22 | 6     | 7    | 0.7377 | 0.684 | 0.997 | 0.533 | 0.640  | " |

Drivers: QED rises c2->c3 (MW score climbs toward ~300 optimum), declines c3->c6
as ROTB score falls (~0.155/rotb) and ALERTS drops (long-chain alert) at c>=4.

## Task 1 — predict m05 (chain 11, rotb 12, mw 305, alogp 4.956, psa 38.33)
Congeneric-series trend fits on QED vs chain_len (chains 2-6):
- Quadratic (5 pts), x=11: **0.5416**  (coeffs: 0.7493 + 0.01702 x - 0.003264 x^2; rmse 0.0119)
- Linear (5 pts), x=11: 0.695 (rmse 0.0131)
- Linear (3 pts, c4-6), x=11: 0.7041 (rmse 0.0024)

Component-based projection (physics):
- ROTB score vs rotb: quad fit (3-7) at rotb=12 -> -1.05 (floors at 0); linear -> -0.034 (floors at 0).
  => m05 ROTB component ~0 (rotb=12 collapses the rotatable-bond desirability).
- MW (mw305): near peak, ~0.72 (m22 mw~277 was 0.684 and rising)
- ALOGP (4.956): slightly past QED peak window -> ~0.7-0.85
- HBA~0.93, HBD~0.99, PSA~0.933 (same as series), AROM~0.827
- ALERTS: long alkyl chain, ~0.5-0.64
With ROTB floored ~0 (geometric-mean-style weighting), overall QED is pulled hard down.

Reconciliation: linear fits (~0.70) IGNORE the ROTB cliff that the component data
shows accelerating. The quadratic QED-vs-chain fit (0.5416) and the component model
(ROTB->0) agree on a LOW value. m05 is 5 chains beyond the measured range; the
quadratic trend of its OWN congeneric series is the instructed predictor.
=> **target_qed = 0.5416** (primary, from quadratic congeneric-series trend),
consistent with the component-level ROTB collapse.

## Task 2 — pick one molecule
Constraint checklist for candidates (must satisfy ALL three):
1. measured QED >= 0.6
2. psa >= 40
3. NOT in Group A (m05's series)

Group B (psa 72.24, satisfies #2):
- m09 (c2): QED 0.5826  -> FAILS #1
- m02 (c3): QED 0.6046  -> PASSES (psa 72.24, Group B) ✓✓✓
- m12 (c4): QED 0.4709  -> FAILS #1
(Group B peaks at c3 and declines; longer chains only worse.)

Group C (psa 110.02 satisfies #2, but alogp strongly negative -> QED low):
not measured; m01 alogp -2.022 implies low QED. Not competitive vs m02.

=> **lead_mol_id = m02**, **lead_qed_measured = 0.6046** (real measure_qed result).
m02 components (free, confirms drug-likeness): MW0.498 ALOGP0.922 HBA0.985
HBD0.987 PSA0.930 ROTB0.970 AROM0.827 ALERTS0.241. Only weak spot is ALERTS
(nitro-aromatic structural alert) which caps overall QED at 0.6046. All other
components strong. Legitimate lead.

## Final answer
target_qed: 0.5416
lead_mol_id: "m02"
lead_qed_measured: 0.6046
