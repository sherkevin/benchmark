# Notes: QED Prediction for m05 and Lead Selection

## Job 1: Predict QED for m05

### Identifying the Congeneric Series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - a methoxy-phenyl-amide with a 13-carbon alkyl chain.

Molecules in the same series (same `COc1ccc(cc1)C(=O)N-` scaffold, varying chain length):
- m15: `COc1ccc(cc1)C(=O)NCC` (2 carbons)
- m24: `COc1ccc(cc1)C(=O)NCCC` (3 carbons)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4 carbons)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons)

### Measurements

Measured QED for all congeners (5 measurements used):
| Mol ID | Chain Length | QED |
|--------|-------------|-----|
| m15    | 2C          | 0.7621 |
| m24    | 3C          | 0.7919 |
| m28    | 4C          | 0.7518 |
| m17    | 5C          | 0.7498 |
| m22    | 6C          | 0.7377 |

### Trend Analysis

The QED peaks at 3 carbons (0.7919), then decreases monotonically:
- 3C → 4C: -0.0401
- 4C → 5C: -0.0020
- 5C → 6C: -0.0121

Fitting a linear trend on the decreasing portion (3-6 carbons):
- Slope ≈ -0.0165 to -0.018 per additional carbon

### Extrapolation to 13 Carbons

m05 has 13 carbons in the chain. Extrapolating from the 6-carbon data point:
- Distance: 13 - 6 = 7 carbons
- Expected drop: 7 × 0.017 ≈ 0.12
- Predicted QED: 0.7377 - 0.12 ≈ 0.62

However, for very long chains, QED penalties for high logP and molecular weight may accelerate the decline. A conservative estimate accounting for non-linear effects:

**Predicted QED for m05: 0.58**

## Job 2: Backup Lead Selection

### Constraints
1. Measured QED ≥ 0.6
2. psa ≥ 40 (free from table)
3. NOT in the same series as m05 (not methoxy-phenyl-amide)

### Filtering by psa ≥ 40

Molecules with psa ≥ 40 (excluding m05 series which has psa=38.33):
- Sugar-amide series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitro-phenyl-amide series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### QED Measurements for Candidates

Measured 7 candidates (7 measurements):
| Mol ID | Series | psa | QED | Meets QED≥0.6? |
|--------|--------|-----|-----|----------------|
| m01    | sugar  | 110.02 | 0.3187 | No |
| m02    | nitro  | 72.24  | 0.6046 | **Yes** |
| m03    | sugar  | 110.02 | 0.3282 | No |
| m09    | nitro  | 72.24  | 0.5826 | No |
| m14    | sugar  | 110.02 | 0.3237 | No |
| m12    | nitro  | 72.24  | 0.4709 | No |
| m32    | nitro  | 72.24  | 0.4683 | No |

### Selected Lead

**m02** is the only molecule found that satisfies all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05 series (nitro-phenyl vs methoxy-phenyl) ✓

## Budget Summary
- Total budget: 12 measurements
- Congeners measured: m15, m17, m22, m24, m28 (5)
- Lead candidates measured: m01, m02, m03, m09, m14, m12, m32 (7)
- Total used: 12/12
