# Notes

## Strategy
1. Used `group_by_scaffold()` (free) to identify 4 scaffold groups.
2. Identified m05's congeneric series (scaffold: COc1ccc(cc1)C(=O)N) with members: m15 (chain=2), m24 (chain=3), m28 (chain=4), m17 (chain=5), m22 (chain=6), m05 (chain=11).
3. Listed pool pages 1-3 (cost: 3 budget) to get free descriptors (especially PSA).
4. Identified scaffold groups for Task 2:
   - Scaffold 1 (m05's series): excluded by Task 2 constraint
   - Scaffold 2 (nitro): PSA=72.24 ≥ 40 ✓
   - Scaffold 3 (sugar): PSA=110.02 ≥ 40 ✓
   - Scaffold 4 (biphenyl): PSA=29.1 < 40 ✗ (excluded)

## Task 1: Predict m05's QED
Measured 5 members of m05's congeneric series (cost: 5 budget):
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

Used `fit_trend(degree=2)` with all 5 points to extrapolate to chain=11:
- **Predicted QED for m05 = 0.5416**
- RMSE = 0.01188 (good fit)
- Polynomial: QED = 0.749 + 0.017*chain - 0.00326*chain²

Compared with linear fit (degree=1): predicted 0.695 — but degree-2 is more physically reasonable as QED should decrease faster for very long chains (compounding penalties from ALOGP=4.956, ROTB=12, MW=305).

QED component analysis confirmed: as chain length increases, ROTB and eventually ALOGP desirabilities drop sharply. m05's very high ALOGP (4.956) and ROTB (12) would significantly reduce QED.

## Task 2: Find lead molecule
Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's series.

Measured candidates (cost: 4 budget):
| mol_id | scaffold | PSA     | QED    | Pass? |
|--------|----------|---------|--------|-------|
| m12    | 2 (nitro)| 72.24   | 0.4709 | ✗ QED too low |
| m09    | 2 (nitro)| 72.24   | 0.5826 | ✗ QED too low |
| m01    | 3 (sugar)| 110.02  | 0.3187 | ✗ QED too low |
| m02    | 2 (nitro)| 72.24   | 0.6046 | ✓ ALL PASS |

**Lead molecule: m02** (SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC)
- Measured QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Scaffold 2 (nitro), not in m05's series ✓

QED components for m02 showed ALERTS score was the main limiting factor (nitro group structural alert), but overall QED still cleared 0.6.

## Budget Usage
- list_pool pages: 3 (pages 1-3)
- measure_qed: 9 (m15, m28, m22, m12, m24, m09, m01, m02, m17)
- Total: 12/12 budget used
