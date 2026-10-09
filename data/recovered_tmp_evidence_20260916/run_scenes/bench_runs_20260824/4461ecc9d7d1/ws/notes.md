# Notes

## Scaffold Analysis (free tool)
4 scaffold groups identified:
1. `COc1ccc(cc1)C(=O)N` — m05's series (chain lengths 2,3,4,5,6,11): m15, m24, m28, m17, m22, m05
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` — 9 members (chain 2-10)
3. `OCC(O)C(O)C(O)C(=O)N` — 9 members (chain 2-10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — 9 members (chain 2-10)

## Task 1: Predict QED of m05 (chain_len=11, scaffold 1)

Measured all 5 synthesized members of scaffold 1:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

Fitted polynomial trend (degree 2, all 5 points):
- Prediction at chain_len=11: **0.5416**
- RMSE on training data: 0.01188

Cross-checked with other fits:
- 5-point linear: 0.695
- 4-point quadratic (chains 3-6): 0.938 (unphysical — curves back up)
- 3-point quadratic (chains 4-6): 0.526
- 3-point linear (chains 4-6): 0.704

The 5-point degree-2 polynomial is the best model: uses all data, captures the peak at chain 3 followed by steady decline, physically reasonable (longer chains → worse ALOGP, MW, ROTB → lower QED).

**Predicted QED for m05: 0.5416**

## Task 2: Find a lead molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, NOT in scaffold 1.

Measured candidates from scaffolds 2 and 3 (both have high PSA):
| mol_id | scaffold | chain | PSA    | QED    | Valid? |
|--------|----------|-------|--------|--------|--------|
| m02    | 2        | 3     | 72.24  | 0.6046 | YES    |
| m01    | 3        | 4     | 110.02 | 0.3187 | no QED |
| m12    | 2        | 4     | -      | 0.4709 | no QED |
| m19    | 3        | 5     | -      | 0.3244 | no QED |
| m09    | 2        | 2     | -      | 0.5826 | no QED |
| m32    | 2        | 5     | -      | 0.4683 | no QED |

**Best lead: m02** — QED 0.6046 (>=0.6), PSA 72.24 (>=40), scaffold 2 (not scaffold 1).

QED components of m02: MW=0.50, ALOGP=0.92, HBA=0.98, HBD=0.99, PSA=0.93, ROTB=0.97, AROM=0.83, ALERTS=0.24.
The ALERTS score (0.24) is the main drag on QED, but all other desirability scores are high.

## Budget usage
- 1 list_pool call (page 1)
- 5 measurements for scaffold 1 (m15, m24, m28, m17, m22)
- 6 measurements for Task 2 (m02, m01, m12, m19, m09, m32)
- Total: 12/12 budget used
