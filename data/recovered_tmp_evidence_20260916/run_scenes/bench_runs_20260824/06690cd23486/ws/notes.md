# Notes on QED Prediction and Lead Selection

## Understanding the Congeneric Series

Looking at the SMILES in pool.tsv, I can identify different scaffolds:

### Series 1: `OCC(O)C(O)C(O)C(=O)N` - polyol amide series
- m01: OCC(O)C(O)C(O)C(=O)NCCCC (chain: CCCC = 4 carbons)
- m03: OCC(O)C(O)C(O)C(=O)NCCCCCC (chain: CCCCCC = 6 carbons)
- m07: OCC(O)C(O)C(O)C(=O)NCCCCCCCCC (chain: 9 carbons)
- m14: OCC(O)C(O)C(O)C(=O)NCC (chain: CC = 2 carbons)
- m18: OCC(O)C(O)C(O)C(=O)NCCCCCCCC (chain: 8 carbons)
- m19: OCC(O)C(O)C(O)C(=O)NCCCCC (chain: 5 carbons)
- m21: OCC(O)C(O)C(O)C(=O)NCCCCCCC (chain: 7 carbons)
- m25: OCC(O)C(O)C(O)C(=O)NCCCCCCCCCC (chain: 10 carbons)
- m27: OCC(O)C(O)C(O)C(=O)NCCC (chain: CCC = 3 carbons)

### Series 2: `O=[N+]([O-])c1ccc(cc1)C(=O)N` - nitrobenzene amide series
- m02: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC (chain: 3 carbons)
- m06: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCC (chain: 9 carbons)
- m09: O=[N+]([O-])c1ccc(cc1)C(=O)NCC (chain: 2 carbons)
- m10: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCCCC (chain: 10 carbons)
- m12: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCC (chain: 4 carbons)
- m29: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCC (chain: 7 carbons)
- m31: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCCCC (chain: 8 carbons)
- m32: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCC (chain: 5 carbons)
- m33: O=[N+]([O-])c1ccc(cc1)C(=O)NCCCCCC (chain: 6 carbons)

### Series 3: `c1ccc(cc1)-c1ccc(cc1)C(=O)N` - biphenyl amide series
- m04: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC (chain: 4 carbons)
- m08: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCC (chain: 9 carbons)
- m11: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCC (chain: 3 carbons)
- m13: c1ccc(cc1)-c1ccc(cc1)C(=O)NCC (chain: 2 carbons)
- m16: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCCCC (chain: 10 carbons)
- m20: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCC (chain: 7 carbons)
- m23: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCC (chain: 6 carbons)
- m26: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC (chain: 8 carbons)
- m30: c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCC (chain: 5 carbons)

### Series 4: `COc1ccc(cc1)C(=O)N` - methoxybenzene amide series (SAME as m05!)
Target m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain: 13 carbons)
- m15: COc1ccc(cc1)C(=O)NCC (chain: 2 carbons)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain: 5 carbons)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain: 6 carbons)
- m24: COc1ccc(cc1)C(=O)NCCC (chain: 3 carbons)
- m28: COc1ccc(cc1)C(=O)NCCCC (chain: 4 carbons)

## Congeners of m05 (Series 4)
The molecules in the same series as m05 are: **m15, m17, m22, m24, m28**

Chain lengths:
- m15: 2 carbons (NCC)
- m24: 3 carbons (NCCC)
- m28: 4 carbons (NCCCC)
- m17: 5 carbons (NCCCCC)
- m22: 6 carbons (NCCCCCC)
- m05: 13 carbons (target - NCCCCCCCCCCC)

## Strategy

1. Measure QED for all congeners of m05 (m15, m17, m22, m24, m28) - 5 measurements
2. Fit a polynomial trend to extrapolate to chain length 13
3. For lead selection:
   - First filter by psa >= 40 (free)
   - Exclude m05 series
   - Measure QED for candidates until finding one >= 0.6

## Measurements - m05 Congeners

| Mol ID | Chain length | QED |
|--------|-------------|-----|
| m15    | 2           | 0.7621 |
| m24    | 3           | 0.7919 |
| m28    | 4           | 0.7518 |
| m17    | 5           | 0.7498 |
| m22    | 6           | 0.7377 |

## Trend Fitting for m05

Fitted a quadratic (degree 2) polynomial through the 5 congener points:
- Coefficients (low to high): 0.74932, 0.017024, -0.003264
- RMSE: 0.01188

Extrapolated to chain length 13: **QED = 0.419**

The quadratic fit is appropriate because:
- The data shows a peak at chain length 3, then decline - non-linear behavior
- Quadratic RMSE (0.01188) < Linear RMSE (0.01308)
- Degree 3 overfits and gives nonsensical predictions (>1)

## Lead Selection

Filtered 18 candidates with psa >= 40 (none from m05 series passed since m05 series has psa=38.33).

Measured QED for candidates:
- m01: 0.3187 (fails QED >= 0.6)
- m02: 0.6046 (PASS - meets all criteria!)
- m09: 0.5826 (fails QED >= 0.6)
- m14: 0.3237 (fails QED >= 0.6)

**Selected lead: m02**
- psa = 72.24 (>= 40) ✓
- Not in m05 series (nitrobenzene scaffold) ✓
- QED = 0.6046 (>= 0.6) ✓
