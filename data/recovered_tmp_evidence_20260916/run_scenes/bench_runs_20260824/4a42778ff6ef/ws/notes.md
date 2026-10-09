# QED Prediction for m05

## Search Strategy

The target molecule m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. To find congeners (same scaffold, different chain lengths), I searched using the scaffold substring `COc1ccc(cc1)C(=O)N`.

## Molecules Found

Search returned 6 total matches across 2 pages:

| Mol ID | SMILES | Chain Length | MW | alogP | rotb |
|--------|--------|--------------|-----|-------|------|
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 11 | 305.46 | 4.956 | 12 |
| m15 | COc1ccc(cc1)C(=O)NCC | 2 | 179.22 | 1.445 | 3 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 | 221.30 | 2.615 | 6 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 | 235.33 | 3.005 | 7 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 | 193.25 | 1.835 | 4 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 | 207.27 | 2.225 | 5 |

## QED Measurements

Measured QED for all 5 available congeners (budget used: 7/12):

| Mol ID | Chain Length (n) | QED |
|--------|------------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis

The QED vs chain length relationship is **non-linear**:
- QED peaks at n=3 (0.7919)
- Declines for n>3: 0.7518 (n=4) → 0.7498 (n=5) → 0.7377 (n=6)

This pattern reflects how QED balances multiple properties. Short chains have lower lipophilicity but may lack optimal hydrophobic interactions. As chain length increases beyond the optimum (n=3), the increasing MW, logP, and rotatable bonds all contribute penalties to QED.

## Extrapolation Method

For extrapolation from n=2-6 to n=11, I considered multiple models:

1. **Linear fit** from peak (n=3) to longest measured (n=6):
   - Slope = (0.7377 - 0.7919) / 3 = -0.0181 per carbon
   - Prediction: 0.7919 - 0.0181 × 8 = 0.647

2. **Exponential decay** from peak:
   - QED(n) = 0.7919 × exp(-0.0236 × (n-3))
   - Prediction for n=11: 0.7919 × exp(-0.1888) = 0.656

3. **Quadratic fit** considering the peak and decline:
   - The data suggests a downward-curving relationship for longer chains
   - Given m05's significantly higher MW (305 vs 235) and logP (4.96 vs 3.0), penalties compound

The target m05 has MW=305.46 and alogP=4.956, approaching QED penalty thresholds (~350 MW, ~5 logP). This suggests the decline may accelerate for longer chains.

## Final Prediction

Considering the observed trend and the compounding penalties for high MW and logP at n=11:

**Predicted QED for m05: 0.62**

This represents a reasonable extrapolation that accounts for:
- The observed decline rate from n=3 to n=6
- The accelerating penalties as MW and logP approach suboptimal ranges
- The non-linear nature of QED as a composite metric

## Congeners in the Same Series

All pool molecules in the m05 series (same scaffold): m15, m17, m22, m24, m28
