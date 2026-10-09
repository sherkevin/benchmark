# Notes

## Task Overview
- Predict QED of m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, 11-carbon N-alkyl chain)
- Find one molecule with measured QED ≥ 0.6, PSA ≥ 40, not in m05's congeneric series

## Congeneric Series of m05
Scaffold: `COc1ccc(cc1)C(=O)N-` with varying N-alkyl chain length.
Members in pool: m15 (2C), m24 (3C), m28 (4C), m17 (5C), m22 (6C), m05 (11C).

## Measurements Used (11 of 12 budget)

### m05 series (5 measurements):
| mol_id | chain | alogp | QED    |
|--------|-------|-------|--------|
| m15    | 2C    | 1.445 | 0.7621 |
| m24    | 3C    | 1.835 | 0.7919 |
| m28    | 4C    | 2.225 | 0.7518 |
| m17    | 5C    | 2.615 | 0.7498 |
| m22    | 6C    | 3.005 | 0.7377 |

m05 itself returned `probe_unavailable` (not synthesized, no cost).

### Task 2 candidates (6 measurements):
| mol_id | series        | PSA    | alogp  | QED    | Pass? |
|--------|---------------|--------|--------|--------|-------|
| m09    | nitrobenzene  | 72.24  | 1.345  | 0.5826 | No (QED < 0.6) |
| m02    | nitrobenzene  | 72.24  | 1.735  | 0.6046 | YES |
| m12    | nitrobenzene  | 72.24  | 2.125  | 0.4709 | No (QED < 0.6) |
| m32    | nitrobenzene  | 72.24  | 2.515  | 0.4683 | No (QED < 0.6) |
| m14    | sugar-like    | 110.02 | -2.802 | 0.3237 | No (QED < 0.6) |
| m27    | sugar-like    | 110.02 | -2.412 | 0.3330 | No (QED < 0.6) |

## Task 1: Predicting m05 QED

### Linear regression on chain length vs QED:
- x = [2, 3, 4, 5, 6], y = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- n = 5, x̄ = 4, ȳ = 0.75866
- slope = (5×15.0823 - 20×3.7933) / (5×90 - 400) = -0.4545/50 = -0.00909
- intercept = (3.7933 + 0.00909×20) / 5 = 0.79502
- Predicted QED for chain length 11: 0.79502 - 0.00909×11 = **0.695**

### Cross-check with alogp regression:
Same linear relationship since alogp increases linearly with chain length (~0.39/CH₂).
Result: ~0.695 (consistent).

### Interpretation:
QED declines with longer alkyl chains due to increasing logP and MW. The prediction of 0.695 represents extrapolation from 6C to 11C (5 additional CH₂ groups).

## Task 2: Lead Molecule Selection

**m02** (`O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`) satisfies all constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Scaffold is nitrobenzene amide (`O=[N+]([O-])c1ccc(cc1)C(=O)N-`), different from m05's methoxybenzene amide (`COc1ccc(cc1)C(=O)N-`) ✓

No other measured molecule met all three constraints simultaneously.
