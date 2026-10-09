# Task: predict QED of m05

## Target
- `m05` SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Parse: `COc1ccc(cc1)` = 4-methoxyphenyl; `C(=O)N` = amide; `NCCCCCCCCCCC` = N-alkyl chain of 11 C's (C11, N-undecyl).
- So m05 = 4-methoxy-N-undecylbenzamide. Never synthesized → `measure_qed` returns `probe_unavailable`. Must predict.
- m05 descriptors: MW 305.46, ALOGP 4.956, PSA 38.33, rotb 12, arom 1.

## Strategy
- Budget 12 shared units (search page = 1, measurement = 1).
- Search with the scaffold substring `COc1ccc(cc1)C(=O)N` (the part shared by every congener; only the N-alkyl tail varies).

## Searches (2 units)
- `search_pool("COc1ccc(cc1)C(=O)N", page=1)` → 4 of 6 matches: m05, m15, m17, m22.
- `search_pool("COc1ccc(cc1)C(=O)N", page=2)` → remaining 2: m24, m28.
- 6 total = the full series. All share scaffold `COc1ccc(cc1)C(=O)N`; only N-alkyl length differs. ✓

## Congeneric series (5 measurements, units 3–7)
| mol_id | chain | MW | ALOGP | rotb | PSA | arom | QED |
|--------|-------|---------|-------|------|-------|------|------|
| m15 | C2 | 179.22 | 1.445 | 3 | 38.33 | 1 | 0.7621 |
| m24 | C3 | 193.25 | 1.835 | 4 | 38.33 | 1 | 0.7919 | ← peak
| m28 | C4 | 207.27 | 2.225 | 5 | 38.33 | 1 | 0.7518 |
| m17 | C5 | 221.30 | 2.615 | 6 | 38.33 | 1 | 0.7498 |
| m22 | C6 | 235.33 | 3.005 | 7 | 38.33 | 1 | 0.7377 |
| m05 (target) | C11 | 305.46 | 4.956 | 12 | 38.33 | 1 | ?? |

Constant across series: PSA, arom, and (not returned) HBD=0, HBA=2 (tertiary amide N has no H; carbonyl + methoxy acceptors = 2), alerts=0. Only MW, ALOGP, rotb vary → they drive the QED change.

## Observed trend
- QED rises C2→C3 (0.7621→0.7919), peaks at C3, then declines monotonically C3→C6 (0.7919→0.7518→0.7498→0.7377).
- The decline C3→C6 is gentle (~0.054 over 3 carbons) because in that range ALOGP (1.8–3.0) and rotb (4–7) are still inside the high-desirability plateau of the QED property functions.
- m05 sits far past the measured range (C11 vs measured ≤C6) and, critically, crosses the QED penalty thresholds: ALOGP 4.956 (≫3) and rotb 12 (≫7) are both deep in penalty territory, MW 305 is at the upper-optimal edge. So this is a genuine extrapolation, and the decline must steepen beyond C6.

## Fitting / extrapolation (two independent methods)

### Method A — quadratic fit through the 5 measured points (data-only)
Fit QED(n) = a·n² + b·n + c to (C,QED) for n=2..6:
least-squares → a = −0.003263, b = 0.01701, c = 0.74935.
Vertex (peak) at n* = −b/2a = 2.61 ≈ C3 ✓ (consistent with observed peak).
Predicted QED(11) = −0.003263·121 + 0.01701·11 + 0.74935 = 0.5417.
Residuals at measured points ≤ 0.021 (decent fit; slightly underestimates the C3 peak).
→ ~0.54. Caveat: this fit is trained only on the pre-threshold region (ALOGP≤3, rotb≤7), so it likely *over*estimates C11 (it cannot see the penalty curves bending down).

### Method B — mechanistic QED decomposition, anchored at C6 (calibrates out the constant-property baseline)
QED = B · (d_MW^0.185 · d_ALOGP^0.208 · d_rotb^0.078), B = constant-property baseline.
Anchor at C6: with d_MW(235)≈0.99, d_ALOGP(3.0)≈0.95, d_rotb(7)≈0.99 → varying factor 0.9868, so B = 0.7377/0.9868 = 0.748.
For m05 use QED desirability curves past their thresholds: d_MW(305)≈0.96, d_ALOGP(4.96)≈0.30–0.40, d_rotb(12)≈0.35–0.45.
Varying factor(m05) ≈ 0.70–0.79 → QED(m05) ≈ 0.52–0.59, central ~0.55.

### Reconciliation
- Method A (data-only): ~0.54 (bias high, pre-threshold).
- Method B (mechanistic, threshold-aware): ~0.55 central (0.52–0.59).
- Both converge on ~0.54–0.55. m05 < m22 (0.7377) for certain (worse on every varying axis). A value ~0.54 is chemically reasonable for a clogP≈5, rotb=12 compound (high lipophilicity is the dominant QED penalty).

## Prediction
**target_qed ≈ 0.54**

## Budget
Used 7/12: search pages 2 + measurements 5. No further calls needed.
