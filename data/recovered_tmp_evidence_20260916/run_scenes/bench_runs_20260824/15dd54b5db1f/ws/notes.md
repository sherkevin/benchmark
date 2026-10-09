# Notes on QED Prediction and Lead Selection

## Target molecule m05
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N-` (methoxy-phenyl amide)
- Chain: CCCCCCCCCCCC (13 carbons in alkyl chain)
- Has NOT been synthesized - measure_qed returns probe_unavailable

## Identifying congeners (same series as m05)
Looking for molecules with the same scaffold `COc1ccc(cc1)C(=O)N-` but different chain lengths:

From the pool:
- m15: `COc1ccc(cc1)C(=O)NCC` - chain length 2
- m24: `COc1ccc(cc1)C(=O)NCCC` - chain length 3
- m28: `COc1ccc(cc1)C(=O)NCCCC` - chain length 4
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - chain length 5
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - chain length 6

These 5 molecules are the congeners of m05 in the pool.

## Other series identified (for lead selection exclusion)
1. Nitro-phenyl series: `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (m02, m06, m09, m10, m12, m29, m31, m32, m33)
2. Biphenyl series: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` (m04, m08, m11, m13, m16, m20, m23, m26, m30)
3. Sugar-like polyol series: `OCC(O)C(O)C(O)C(=O)N-` (m01, m03, m07, m14, m18, m19, m21, m25, m27)

## Plan
1. Measure QED for all 5 congeners (m15, m24, m28, m17, m22)
2. Fit a trend (polynomial) to QED vs chain length
3. Extrapolate to chain length 13 for m05
4. For lead selection: find molecules with psa >= 40, not in m05's series, and measure QED >= 0.6

## Measurements taken

### Congeners of m05 (for trend fitting):
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

### Trend fitting results:
- Degree 1 (linear): prediction = 0.6768, RMSE = 0.01308
- Degree 2 (quadratic): prediction = 0.419, RMSE = 0.01188
- Degree 3 (cubic): prediction = 3.8993 (unreasonable, >1)

Selected degree 2 as it has lower RMSE than linear and gives a reasonable value (unlike cubic which overfits).

**Predicted QED for m05 (chain 13): 0.419**

### Lead compound search:
Candidates passing psa >= 40: 18 molecules (m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33)

None of these are in m05's series (m15, m24, m28, m17, m22), so all pass constraint 3.

Measured QED for lead candidates:
- m02: QED = 0.6046 >= 0.6 ✓ SELECTED
- m01: QED = 0.3187 < 0.6 ✗
- m09: QED = 0.5826 < 0.6 ✗

**Selected lead: m02**
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- psa: 72.24 >= 40 ✓
- QED: 0.6046 >= 0.6 ✓
- Not in m05 series ✓
