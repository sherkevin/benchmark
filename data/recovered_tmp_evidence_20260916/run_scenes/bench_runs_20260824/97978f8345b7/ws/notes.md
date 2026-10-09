# Notes

## Catalogue Overview
Read all 9 pages of the compound pool (33 molecules). Budget used: 9 pages + 3 measurements = 12.

## Congeneric Series Analysis for m05

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: methoxybenzene amide `COc1ccc(cc1)C(=O)N-`

Members of the m05 congeneric series (same scaffold, only N-alkyl chain length varies):
| mol_id | N-alkyl chain | MW     | ALogP | PSA    | RotB |
|--------|---------------|--------|-------|--------|------|
| m15    | CC (C2)       | 179.22 | 1.445 | 38.33  | 3    |
| m24    | CCC (C3)      | 193.25 | 1.835 | 38.33  | 4    |
| m28    | CCCC (C4)     | 207.27 | 2.225 | 38.33  | 5    |
| m17    | CCCCC (C5)    | 221.30 | 2.615 | 38.33  | 6    |
| m22    | CCCCCC (C6)   | 235.33 | 3.005 | 38.33  | 7    |
| m05    | CCCCCCCCCCC (C11) | 305.46 | 4.956 | 38.33 | 12 |

## Task 1: Predicting QED for m05

Measured QED values from the series:
- m15 (C2): QED = 0.7621
- m22 (C6): QED = 0.7377

Trend: QED decreases as chain length increases.
- Rate of decrease: (0.7621 - 0.7377) / (6 - 2) = 0.0244 / 4 = 0.0061 per carbon

Linear extrapolation to C11:
- From C6 to C11: 5 more carbons
- Expected decrease: 5 × 0.0061 = 0.0305
- Predicted QED(m05) = 0.7377 - 0.0305 = 0.7072

Rounding: **target_qed ≈ 0.71**

## Task 2: Finding a Lead Molecule

Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in same congeneric series as m05

m02 SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Scaffold: nitrobenzene amide — different from m05's methoxybenzene amide ✓ (not congeneric)
- PSA: 72.24 ≥ 40 ✓
- Measured QED: 0.6046 ≥ 0.6 ✓

All three constraints satisfied.

## Budget Summary
- Pages read: 9 (cost: 9)
- Measurements: m15, m22, m02 (cost: 3)
- Total: 12/12
