# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (never synthesized, measure_qed returns probe_unavailable)
- Pick a backup lead compound with: QED >= 0.6, psa >= 40, NOT in m05's series

## Identifying m05's Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: para-methoxy-benzamide (`COc1ccc(cc1)C(=O)N`)
- Alkyl chain: 11 carbons (CCCCCCCCCCC)

Looking through the pool for same scaffold (COc1ccc(cc1)C(=O)N...):
- m15: `COc1ccc(cc1)C(=O)NCC` - 2 carbons
- m24: `COc1ccc(cc1)C(=O)NCCC` - 3 carbons
- m28: `COc1ccc(cc1)C(=O)NCCCC` - 4 carbons
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - 5 carbons
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - 6 carbons

**Congeners of m05 in pool: m15, m17, m22, m24, m28**

Chain lengths in the congeneric series:
- m15: 2 carbons (CC)
- m24: 3 carbons (CCC)
- m28: 4 carbons (CCCC)
- m17: 5 carbons (CCCCC)
- m22: 6 carbons (CCCCCC)
- m05: 11 carbons (CCCCCCCCCCC) - target to predict

So we have data for chain lengths 2-6, and need to extrapolate to chain length 11.

## QED Measurements for m05 Congeners

| mol_id | chain_length | QED |
|--------|--------------|-----|
| m15    | 2            | 0.7621 |
| m24    | 3            | 0.7919 |
| m28    | 4            | 0.7518 |
| m17    | 5            | 0.7498 |
| m22    | 6            | 0.7377 |

## Trend Fitting and Extrapolation

Fit a quadratic polynomial (degree=2) through the 5 congener points:
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE on given points: 0.01188
- Prediction at x=11: **0.5416**

Also tried linear fit (degree=1):
- Prediction at x=11: 0.695
- RMSE: 0.01308

The quadratic fit has lower RMSE (0.01188 vs 0.01308), so it fits the observed data better.
The trend shows QED peaks around chain length 3 and then decreases as chain gets longer.
Extrapolating to chain length 11, the quadratic predicts QED = 0.5416.

**Final prediction for m05 QED: 0.5416**

## Lead Compound Selection

Constraints:
1. measured QED >= 0.6
2. psa >= 40
3. NOT in m05's series (not m15, m17, m22, m24, m28)

Screened candidates with psa >= 40: 18 molecules passed.
None of the m05 congeners have psa >= 40 (they all have psa = 38.33), so all 18 are valid candidates for constraint 3.

Measurements taken:
- m01: QED = 0.3187 (fails, < 0.6)
- m02: QED = 0.6046 (PASS!)
- m03: QED = 0.3282 (fails, < 0.6)
- m09: QED = 0.5826 (fails, < 0.6)

**Selected lead: m02**
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Not in m05 series (nitro-benzamide scaffold, not methoxy-benzamide) ✓

## Final Answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
