# Medicinal Chemistry Task - QED Prediction and Lead Selection

## Overview
Task: Predict QED for molecule m05 (never synthesized) and select a backup lead compound meeting three constraints.

Constraints for lead selection:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free from catalogue)
3. Not in same series as m05

Budget: 17 total QED measurements shared between both jobs.

## Initial Approach
1. First, explore the compound pool to identify molecules in the same series as m05
2. Analyze m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
   - Scaffold: COc1ccc(cc1)C(=O)N (methoxybenzamide scaffold)
   - N-alkyl chain: CCCCCCCCCCC (11-carbon chain)
3. Need to find congeners with same scaffold but different chain lengths

Let's start exploring the pool...

## Pool Exploration
Page 1 observations:
- m01: OCC(O)C(O)C(O)C(=O)NCCCC (different scaffold - sugar-like)
- m02: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC (nitrobenzamide with 3-carbon chain)
- m03: OCC(O)C(O)C(O)C(=O)NCCCCCC (similar to m01 but longer chain)
- m04: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC (biphenyl scaffold with 4-carbon chain)

None match m05's scaffold (methoxybenzamide). Need to continue searching.

## Congeners Found (same series as m05)
Based on SMILES analysis, the following molecules share the methoxybenzamide scaffold (COc1ccc(cc1)C(=O)N) with different chain lengths:

1. m15: COc1ccc(cc1)C(=O)NCC (2-carbon chain)
2. m24: COc1ccc(cc1)C(=O)NCCC (3-carbon chain) 
3. m28: COc1ccc(cc1)C(=O)NCCCC (4-carbon chain)
4. m17: COc1ccc(cc1)C(=O)NCCCCC (5-carbon chain)
5. m22: COc1ccc(cc1)C(=O)NCCCCCC (6-carbon chain)

m05 has: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11-carbon chain)

The pool contains shorter-chain congeners (2-6 carbons). We need to extrapolate to 11 carbons.

## QED Measurements for Congeners
Chain length (carbons) vs QED:
- m15 (2C): 0.7621
- m24 (3C): 0.7919 
- m28 (4C): 0.7518
- m17 (5C): 0.7498
- m22 (6C): 0.7377

## Trend Analysis
Chain length vs QED pattern:
1. QED peaks at 3-carbon chain (0.7919)
2. Significant drop from 3C to 4C (-0.0401)
3. Smaller decreases from 4C to,6C (-0.0121 total)

Possible explanations:
- Optimal chain length for this scaffold is 3 carbons
- Longer chains increase lipophilicity (higher AlogP) beyond optimal range
- Increased molecular weight and rotatable bonds reduce QED

For extrapolation to 11 carbons (m05):
- From 6C to 11C is 5 additional carbons
- The trend suggests continued decrease
- Rate of decrease may slow (as seen from 4C-6C)

## Lead Compound Selection
Candidate screening for lead meeting all constraints:
1. Not in m05 series: exclude methoxybenzamides (m15, m24, m28, m17, m22)
2. PSA ≥ 40: exclude biphenyl series (PSA=29.1)
3. Remaining candidates: nitrobenzamide and sugar-like series

Tested nitrobenzamides:
- m02 (3C): QED = 0.6046 ✓ (meets all constraints)
- m09 (2C): QED = 0.5826 ✗ (< 0.6)
- m12 (4C): QED = 0.4709 ✗ (< 0.6)

Selected lead: m02 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCC)

## QED Prediction for m05 (11-carbon chain)
Extrapolation from measured congeners:
Chain lengths: 2C(0.7621), 3C(0.7919), 4C(0.7518), 5C(0.7498), 6C(0.7377)

Trend: QED peaks at 3C then decreases. Average decrease per carbon beyond 3C ≈ -0.018

From 6C to 11C: 5 carbons × -0.018 ≈ -0.09
Predicted QED: 0.7377 - 0.09 ≈ 0.6477

Considering nonlinearity and possible leveling off, predicted QED ≈ 0.65

## Final Answer
- Predicted QED for m05: 0.65
- Selected lead compound: m02 with measured QED 0.6046
- Congeners in m05 series: m15, m24, m28, m17, m22

Budget used: All 17 units consumed (9 exploration + 8 measurements)