# Notes: QED Prediction for m05

## Search Strategy

The target molecule m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. To find congeners (same scaffold, different chain length), I searched using the scaffold substring `COc1ccc(cc1)C(=O)N`.

## Congeners Found

Search returned 6 molecules total (2 pages):

| mol_id | SMILES | Chain | Carbons |
|--------|--------|-------|---------|
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | dodecyl | 12 (target) |
| m15 | COc1ccc(cc1)C(=O)NCC | ethyl | 2 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | pentyl | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | hexyl | 6 |
| m24 | COc1ccc(cc1)C(=O)NCCC | propyl | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | butyl | 4 |

## QED Measurements

Measured all 5 available congeners (budget used: 7 of 12):

| mol_id | Carbons (n) | QED |
|--------|-------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis

The QED vs chain length shows:
- Peak at n=3 (QED = 0.7919)
- Decreasing trend for n ≥ 3

Rate of change:
- n=2→3: +0.0298 (rising to optimum)
- n=3→4: -0.0401 (sharp drop)
- n=4→5: -0.0020 (small drop)
- n=5→6: -0.0121 (moderate drop)

For n ≥ 3, the overall trend is decreasing with an average rate of approximately -0.018 per carbon from n=3 to n=6.

## Extrapolation to n=12

The target m05 has a 12-carbon chain (dodecyl), which is 6 carbons longer than the longest measured congener (m22, n=6).

Using linear extrapolation from the n=3 to n=6 trend:
- Slope ≈ -0.018 per carbon
- Prediction: 0.7377 + (-0.018 × 6) ≈ 0.63

Considering that:
1. The rate of decrease may slow for longer chains (saturation effect)
2. m05 has very high alogp (4.956) which penalizes QED
3. The extrapolation range is large (6 carbons beyond measured data)

A reasonable estimate accounting for potential non-linearity is **QED ≈ 0.64**.

This reflects:
- Continued decrease from n=6 (QED=0.7377) due to increasing lipophilicity
- Possible slowing of the degradation rate at longer chain lengths
- Significant penalty from high logP (alogp=4.956 for m05 vs 3.005 for m22)

## Final Prediction

**target_qed: 0.64**

**congener_ids: ["m15", "m24", "m28", "m17", "m22"]**
