# Notes: QED Prediction and Lead Selection

## Series Identification

**Target m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, scaffold `COc1ccc(cc1)C(=O)N`, chain length 11.

### m05's Congeneric Series (methoxybenzamide scaffold)
| mol_id | chain_len | QED (measured) |
|--------|-----------|-----------------|
| m15    | 2         | 0.7621          |
| m24    | 3         | 0.7919          |
| m28    | 4         | 0.7518          |
| m17    | 5         | 0.7498          |
| m22    | 6         | 0.7377          |

### Other Series Identified
- **Nitrobenzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- **Sugar-like** `OCC(O)C(O)C(O)C(=O)N`: m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- **Biphenyl** `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Job 1: QED Prediction for m05

### Observed Trend
The QED values show a non-linear pattern: rising from chain 2 (0.7621) to a peak at chain 3 (0.7919), then declining steadily through chains 4-6 (0.7518 → 0.7498 → 0.7377).

### Fitted Model
Quadratic (degree 2) polynomial fit through all 5 measured points:

**QED(x) = 0.74932 + 0.017024·x − 0.003264·x²**

- RMSE on training points: 0.01188 (good fit)
- The negative quadratic coefficient captures the peak-and-decline pattern

### Extrapolation to chain 11
QED(11) = 0.74932 + 0.017024·11 − 0.003264·121 = **0.5416**

This makes physical sense: longer N-alkyl chains increase lipophilicity, eventually reducing drug-likeness.

## Job 2: Lead Compound Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check via screen_candidates)
3. Not in m05's series (free check via describe_structure)

### QED Measurements of PSA-Passing Candidates
| mol_id | series   | chain | PSA    | QED    | Pass? |
|--------|----------|-------|--------|--------|-------|
| m02    | nitro    | 3     | 72.24  | 0.6046 | YES   |
| m09    | nitro    | 2     | 72.24  | 0.5826 | No    |
| m12    | nitro    | 4     | 72.24  | 0.4709 | No    |
| m32    | nitro    | 5     | 72.24  | 0.4683 | No    |
| m33    | nitro    | 6     | 72.24  | 0.4591 | No    |
| m29    | nitro    | 7     | 72.24  | 0.4446 | No    |
| m01    | sugar    | 4     | 110.02 | 0.3187 | No    |
| m03    | sugar    | 6     | 110.02 | 0.3282 | No    |
| m14    | sugar    | 2     | 110.02 | 0.3237 | No    |
| m27    | sugar    | 3     | 110.02 | 0.333  | No    |
| m19    | sugar    | 5     | 110.02 | 0.3244 | No    |

### Selected Lead: m02
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Not in m05's series (nitrobenzamide ≠ methoxybenzamide) ✓

## Budget Usage
- 1: list_pool page 1
- 5: m05 series measurements
- 11: lead candidate measurements
- Total: 17/17
