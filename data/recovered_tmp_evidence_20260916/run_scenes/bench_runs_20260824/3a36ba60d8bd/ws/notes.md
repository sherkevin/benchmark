# Task notes

## Task overview
- 33 molecule pool, catalogue not given. Tools: `list_pool(page)` (4/page, 1 budget), `measure_qed(mol_id)` (1 budget). Shared budget 17 units.
- Task 1: report QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC, chain_len 11), never synthesized → probe_unavailable. Predict from its OWN congeneric series (same scaffold, N-alkyl chain differs).
- Task 2: pick ONE molecule: measured QED ≥ 0.6 AND psa ≥ 40 AND not in m05's congeneric series.
- Deliverables: notes.md + answer.json {target_qed, lead_mol_id, lead_qed_measured}. Every measured QED must come from a real measure_qed call.

## Scaffolds (from free group_by_scaffold)
4 groups:
- G1 scaffold `COc1ccc(cc1)C(=O)N` (methoxy-benzamide) — m05's series:
  m15(2), m24(3), m28(4), m17(5), m22(6), m05(11, target, unsynth)
- G2 scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide):
  m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- G3 scaffold `OCC(O)C(O)C(O)C(=O)N` (sugar/polyol amide):
  m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- G4 scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl amide):
  m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Reasoning for Task 2 (psa ≥ 40)
Secondary amide (ArC(=O)NH-alkyl) ≈ 29 Å². Methoxy +9, nitro +45, each OH +20, biphenyl +0.
- G1: ~38 (borderline; excluded anyway — m05's series).
- G2 (nitro): ~74 → passes psa. But nitro is an alert substructure → QED likely penalized/low.
- G3 (sugar): ~100+ → passes psa. 0 aromatic rings → QED aromatic-weight low (~0.394), but high HBD count may hurt.
- G4 (biphenyl): ~29 → likely FAILS psa ≥ 40.
→ Task-2 candidates from G2 or G3. Need to measure to find QED ≥ 0.6.

## Budget plan (17 total)
- Task1: measure m15,m24,m28,m17,m22 (5) → fit trend, evaluate at chain 11.
- Task2: measure G2/G3 candidates until QED ≥ 0.6. Confirm psa via 1 list page if needed.
- Keep within 17.

## Task 1 measurements (G1 series, scaffold methoxy-benzamide)
| chain | mol_id | QED |
|---|---|---|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |  ← peak
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |
| 11 | m05 | probe_unavailable → PREDICT |

Note: first parallel batch errored server-side (results lost) but budget still
incremented (m15 retry reported measurements_used=6). So 5 budget wasted; 10 used
after Task-1 series, 7 remaining.

### Fits to predict chain 11
- degree 2 (quadratic): prediction 0.5416, RMSE 0.01188 — captures peak+decline.
- degree 1 (linear):   prediction 0.6950, RMSE 0.01308.

Chosen: quadratic 0.5416. Reasons: lower RMSE; data shows curvature (peak at chain ~2.6
then decline); QED physics — as N-alkyl chain grows, alogP rises past the good [1.5–3]
band and rotB rises past [2–3], so the penalty accelerates (concave decline). Manual
QED estimate for chain 11 (logP~5, rotB~12 → both desirabilities collapse) lands ~0.45–0.55,
consistent with the quadratic, not the linear 0.695.
→ target_qed = 0.5416

## Task 2 strategy
Constraint: QED ≥ 0.6 AND psa ≥ 40 AND NOT in G1 (m05's methoxy-benzamide series).
PSA by SMILES (TPSA, no need to spend a page):
- G1 (methoxy-benzamide): ~38 — excluded anyway (m05's series).
- G2 (nitro-benzamide): amide 29 + nitro ~45 ≈ 74 → passes psa. NOT G1 ✓.
- G3 (sugar polyol amide): 3×OH(60)+ether(9)+amide(29) ≈ 98 → passes psa, but
  logP very low (≈−2…−1, below good band), arom=0, HBD≈4, HBA≈5, PSA~98 → many
  desirabilities ~0.5 → QED ≲ 0.52. Poor bet.
- G4 (biphenyl amide): amide only ≈ 29 → FAILS psa ≥ 40.
→ Best bet: G2, short chain (low MW, low rotB → best QED). m09 (chain 2) first.
Measure one at a time to avoid the lost-result bug.

## Task 2 measurements
- m09 (G2, nitro, chain 2): QED 0.5826  (measurements_used 11)
- m02 (G2, nitro, chain 3): QED 0.6046  (used 12) ← peak of nitro series, ≥ 0.6 ✓
- m12 (G2, nitro, chain 4): QED 0.4709  (used 13) — declining after chain 3
Series shape: 0.5826 → 0.6046 → 0.4709 (chains 2,3,4). Peak at chain 3 = m02.
Longer G2 chains only decline further (rising MW/logP/rotB) → no better G2 lead.
G3 (sugar) estimate ≤ 0.52 (logP ≈ −1.5, PSA ≈ 99, HBD 4 → many low desirabilities).
G4 (biphenyl) fails psa ≥ 40 (amide-only ≈ 29). G1 excluded (m05's series).
→ m02 is the global best for Task 2.

### m02 satisfies all three Task-2 constraints
1. measured QED 0.6046 ≥ 0.6 ✓ (from real measure_qed call)
2. psa ≥ 40 ✓: nitro-benzamide, secondary amide. TPSA ≈ amide(29.1) + nitro(≈45) ≈ 74
   (even most pessimistic nitro contribution keeps it ≥ 40)
3. not in G1 ✓: m02 scaffold is nitro-benzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N`,
   different from m05's methoxy-benzamide `COc1ccc(cc1)C(=O)N`.

## Final answers
- target_qed = 0.5416  (quadratic fit, degree 2, RMSE 0.0119 — lower than linear 0.0131;
  captures peak at chain ~2.6 + accelerating decline; physically motivated since at chain 11
  alogP rises far past optimal band and rotB past 3, so QED penalty accelerates → concave,
  favoring quadratic over the linear 0.6950)
- lead_mol_id = m02
- lead_qed_measured = 0.6046

Budget final: 13 used, 4 spare. (5 wasted by initial parallel-batch server error.)
