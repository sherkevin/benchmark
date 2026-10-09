# Notes

## Approach

### Task 1: Predict QED of m05

**m05** has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain_len=11) and has never been synthesized.

Used `group_by_scaffold()` (free) to identify m05's congeneric series:
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Members: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)

Measured QED of all 5 synthesized members (cost: 5 units):
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Fitted degree-2 polynomial (default) through all 5 points and evaluated at chain_len=11:
- **Predicted QED = 0.5416** (RMSE on training = 0.01188)
- Coefficients: [0.74932, 0.017024, -0.003264]

Cross-checked with other fits:
- Degree-1 (all 5 pts): 0.695
- Degree-2 (pts 3-6 only): 0.9378 — unrealistic upward curve
- Degree-1 (pts 3-6): 0.6508
- Degree-3 (all 5 pts): 2.13 — diverges (overfitting)
- Degree-4 (all 5 pts): -12.1 — diverges (overfitting)

Degree-2 on all 5 points is the most defensible: uses all data, physically reasonable (QED decreases with increasing chain length due to rising lipophilicity and rotatable bonds).

m05's descriptors confirm unfavorable properties: MW=305.46, ALogP=4.956 (borderline high), PSA=38.33 (borderline low), rotb=12 (high), arom=1.

### Task 2: Find a lead molecule

Constraints: QED >= 0.6, PSA >= 40, not in m05's congeneric series.

Listed pool pages 1-4 (cost: 4 units) to check PSA values. Identified candidates with PSA >= 40 not in m05's series:
- Nitro-aromatic series (PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar-like series (PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

Measured QED of three nitro-aromatic candidates (cost: 3 units):
| mol_id | QED    | PSA    | Pass? |
|--------|--------|--------|-------|
| m09    | 0.5826 | 72.24  | QED < 0.6 fail |
| m02    | 0.6046 | 72.24  | ALL pass |
| m12    | 0.4709 | 72.24  | QED < 0.6 fail |

**Selected m02** as lead:
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED = 0.6046 (measured)
- PSA = 72.24 >= 40
- Different scaffold from m05

## Budget Used
- 5 measurements (m05 series): 5 units
- 4 page listings: 4 units
- 3 measurements (Task 2 candidates): 3 units
- **Total: 12 of 17 units**
