# Notes

## Setup
- 33 molecules, 4/page, 9 pages. Budget = 12 total (shared list_pool + measure_qed).
- Listed all 9 pages → 9 units. Made 3 measurements → 12/12. **Budget exhausted.**

## m05 target (Task 1)
- SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` = 4-methoxybenzamide, N-undecyl (11 C).
- mw 305.46, alogp 4.956, psa 38.33, rotb 12, arom 1. Never synthesized (probe_unavailable).

## Methoxy congeneric series (scaffold `COc1ccc(cc1)C(=O)N`, only N-alkyl chain length varies)
| mol | chainC | alogp | rotb | mw   | QED (meas) |
|-----|-------|-------|------|------|------------|
| m15 | 2  | 1.445 | 3  | 179.22 | 0.7621 |
| m24 | 3  | 1.835 | 4  | 193.25 | (not meas) |
| m28 | 4  | 2.225 | 5  | 207.27 | (not meas) |
| m17 | 5  | 2.615 | 6  | 221.30 | (not meas) |
| m22 | 6  | 3.005 | 7  | 235.33 | 0.7377 |
| m05 | 11 | 4.956 | 12 | 305.46 | TARGET (predict) |
- alogp slope ≈ +0.39/CH2 (linear; 3.005+5×0.39=4.955≈m05 4.956 ✓).
- rotb = chainC+1 (3..7, then 12 at 11C; crosses rotb penalty threshold ~10 only at m05).

## Task 1 prediction derivation (weighted-geometric-mean model, QED mean weights)
Constant across methoxy series (d≈1): psa 38.33, arom 1, HBD 1, HBA ~2-3, alerts none.
Varying: alogp, mw, rotb. Normalized weights: alogp 0.1325, mw 0.1305, rotb 0.1225.

Calibrate alogp penalty from m15→m22 (both rotb<10 → d_rotb≈1):
m15(alogp1.445)→m22(alogp3.005) dropped 0.7621→0.7377 despite mw improving (179→235).
=> alogp decline already active by alogp 3; fit gives d_alogp(alogp=3.005)≈0.75 (aggressive, decline starts ~2.0-2.5).

Extrapolate d_alogp linearly to alogp 4.956: d≈0.26 (range 0.20-0.35).
m05 penalties vs m22: d_alogp 0.26 vs 0.75; d_mw 0.95 vs 1.0 (305 vs 235); d_rotb 0.6 vs 1.0 (rotb 12 vs 7).
ln(QED_m05/QED_m22) = 0.1325·ln(0.26/0.75) + 0.1305·ln(0.95/1.0) + 0.1225·ln(0.6/1.0)
= -0.1402 - 0.0067 - 0.0626 = -0.2095 → factor 0.811
=> QED_m05 ≈ 0.7377 × 0.811 = 0.598

Anchor to m15 instead: ln ratio = 0.1325·ln(0.26/1.0)+0.1305·ln(0.95/0.95)+0.1225·ln(0.6/1.0)
= -0.1785 + 0.0 + (-0.0626) = -0.2369 (mw 179 vs 305 ~equal d) → QED ≈ 0.7621×0.789 = 0.601.
Both anchors ≈ 0.60. Residual uncertainty in d_alogp at alogp~5 (cliff risk near logP 5) → range 0.57-0.63.

**Task 1 answer: target_qed = 0.60** (central of the two anchored estimates; reflects alogp penalty at logP≈5 + rotb 12 penalty pulling m05 below the m15/m22 plateau of ~0.74-0.76).

## Task 2 outcome (HONEST — DID NOT FULLY SUCCEED)
Picked **m32** (4-nitrobenzamide, N-pentyl, 5C): alogp 2.515, mw 236.27, psa 72.24(≥40✓), rotb 6, arom 1, not methoxy ✓. Descriptors near-ideal; only risk was aromatic-nitro alert.
- Measured QED = **0.4683** (< 0.6 ✗). FAILED constraint 1.
- The nitro alert (or nitro's QED penalty) was far more severe than my sensitivity estimate predicted.
- Budget exhausted (12/12) → cannot re-measure a sugar amide (the alternative, psa 110, no-alert candidate).

Constraint check on all three measured molecules:
- m15: QED 0.7621(≥0.6✓), psa 38.33(<40✗), methoxy series ✗ → fails 2/3.
- m22: QED 0.7377(≥0.6✓), psa 38.33(<40✗), methoxy series ✗ → fails 2/3.
- m32: QED 0.4683(<0.6✗), psa 72.24(≥40✓), not methoxy ✓ → fails 1/3 (closest).

No measured molecule satisfies all three constraints. m32 is the closest (satisfies psa + not-methoxy, fails only QED) and was my deliberate Task 2 pick, so I report it with its real measured QED (0.4683) rather than fabricate a value for an unmeasured molecule. A sugar amide (e.g. m01/m03) would have been the better Task 2 bet (no structural alert, psa 110) but was not measured before budget ran out.

## Deliverables
- target_qed = 0.60 (Task 1, predicted from methoxy series trend; m05 unsynthesized)
- lead_mol_id = "m32" (Task 2 attempted pick; real measurement)
- lead_qed_measured = 0.4683 (real measure_qed call) — NOTE: below the 0.6 bar; Task 2 not fully satisfied given exhausted budget.
