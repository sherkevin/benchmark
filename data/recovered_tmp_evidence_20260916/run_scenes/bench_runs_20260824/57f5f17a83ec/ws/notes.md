# Notes for QED Prediction and Lead Compound Selection

## Analysis of Congeneric Series for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has a methoxyphenyl group connected to a carbonyl nitrogen with an 11-carbon alkyl chain.

Identified congeners in the same series (same scaffold, different chain lengths):
- m15: COc1ccc(cc1)C(=O)NCC (2-carbon chain)
- m24: COc1ccc(cc1)C(=O)NCCC (3-carbon chain) 
- m28: COc1ccc(cc1)C(=O)NCCCC (4-carbon chain)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5-carbon chain)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6-carbon chain)

## QED Measurements for Trend Analysis

- m15 (2-carbon): QED = 0.7621
- m24 (3-carbon): QED = 0.7919
- m28 (4-carbon): QED = 0.7518
- m17 (5-carbon): QED = 0.7498
- m22 (6-carbon): QED = 0.7377

## Trend Analysis and Prediction for m05

The trend shows:
- Initial increase from 2-carbon (0.7621) to 3-carbon (0.7919)
- Then gradual decrease: 4-carbon (0.7518) → 5-carbon (0.7498) → 6-carbon (0.7377)

This indicates that as the alkyl chain lengthens beyond 3 carbons, QED decreases. Since m05 has 11 carbon atoms in the chain (significantly longer than our measured maximum of 6), I predict its QED will continue the decreasing trend and be lower than 0.7377. Extrapolating the observed trend, I estimate m05's QED to be approximately 0.68.

## Lead Compound Selection

For the lead compound, I needed one that meets all three criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

I identified m02 (O=[N+]([O-])c1ccc(cc1)C(=O)NCCC) as meeting all criteria:
- QED measured as 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Has nitrobenzene scaffold, not methoxyphenyl (different series from m05) ✓

## Summary
- Predicted QED for m05: ~0.68 (based on extrapolation of decreasing trend)
- Selected lead compound: m02
- Lead compound meets all three constraints