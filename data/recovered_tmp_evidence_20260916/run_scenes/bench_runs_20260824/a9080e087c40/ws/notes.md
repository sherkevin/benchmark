# Notes on QED Prediction for m05

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is an N-alkyl amide with a methoxy-phenyl group
- The chain is CCCCCCCCCCC (11 carbons in the alkyl chain attached to N)

## Strategy
1. Search for molecules with the scaffold `COc1ccc(cc1)C(=O)N` to find congeners
2. Measure QED for each congener found
3. Analyze the trend of QED vs chain length
4. Extrapolate to predict QED for m05 (11-carbon chain)

## Search Results

Found 6 total matches with the scaffold `COc1ccc(cc1)C(=O)N`:

| mol_id | SMILES | Chain | Chain Length (n) |
|--------|--------|-------|------------------|
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | CCCCCCCCCCC | 11 (TARGET - not measurable) |
| m15 | COc1ccc(cc1)C(=O)NCC | CC | 2 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | CCCCC | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | CCCCCC | 6 |
| m24 | COc1ccc(cc1)C(=O)NCCC | CCC | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | CCCC | 4 |

**Congeners in the same series as m05**: m15, m17, m22, m24, m28

## QED Measurements

| mol_id | Chain Length (n) | QED |
|--------|------------------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis

The QED vs chain length shows:
- n=2: 0.7621
- n=3: 0.7919 (peak)
- n=4: 0.7518 (decreasing)
- n=5: 0.7498 (decreasing slowly)
- n=6: 0.7377 (decreasing)

The pattern shows QED peaks at n=3, then decreases. The decrease from n=3 to n=6 is:
- n=3→4: -0.0401
- n=4→5: -0.0020
- n=5→6: -0.0121

This is not a simple linear trend. The QED appears to follow a curve that peaks around n=3 and then gradually decreases.

### Fitting Approach

Looking at the data, I'll fit a quadratic model: QED = a + b*n + c*n²

Using the 5 data points (n=2,3,4,5,6), I can estimate the curve.

Data:
- (2, 0.7621)
- (3, 0.7919)
- (4, 0.7518)
- (5, 0.7498)
- (6, 0.7377)

The peak appears near n=3, suggesting a parabola opening downward.

Let me do a rough quadratic fit:
- At n=3, QED ≈ 0.79 (peak region)
- The curve descends on both sides

For extrapolation to n=11, I need to consider that QED typically has a bell-shaped or inverted-U relationship with chain length in drug-likeness (optimal lipophilicity window).

Looking at the descent from n=3 to n=6:
- Total drop: 0.7919 - 0.7377 = 0.0542 over 3 units
- Average slope: -0.018 per carbon

If this trend continues linearly from n=6 to n=11 (5 more carbons):
- Predicted QED at n=11 ≈ 0.7377 - 5*0.018 = 0.7377 - 0.09 = 0.6477

But the task warns not to assume a straight line. QED curves often plateau at longer chain lengths as the effect saturates.

Looking more carefully at the rate of change:
- n=3→4: -0.0401 (steep drop)
- n=4→5: -0.0020 (almost flat)
- n=5→6: -0.0121 (moderate drop)

The changes are irregular but the overall trend from n=3 onwards is downward.

Alternative: fit a quadratic through the data. Using points (3, 0.7919), (5, 0.7498), (6, 0.7377):

Let me try a simpler approach - the average QED for n≥4 is about 0.746, and the trend is slowly downward. At n=11, with 5 more carbons beyond n=6, if we assume the slope continues around -0.01 to -0.015 per carbon:

Prediction: 0.7377 - (5 × 0.012) ≈ 0.7377 - 0.06 = 0.6777

Given the uncertainty and the non-linear nature, I'll estimate around **0.67-0.68** for n=11.

Actually, let me reconsider. Drug-likeness (QED) typically decreases significantly for very long alkyl chains due to poor solubility and high molecular weight. The chain of 11 carbons is much longer than the measured range (2-6).

Looking at the pattern more carefully:
- The drop from peak (n=3) to n=6 is about 0.054 over 3 carbons
- If this rate continues: 0.054/3 × 8 = 0.144 drop from n=3 to n=11
- Prediction: 0.7919 - 0.144 ≈ 0.648

I'll go with a prediction of approximately **0.65** for m05.

## Final Prediction

**Predicted QED for m05 (n=11): 0.65**

**Congener IDs found in same series**: m15, m17, m22, m24, m28
