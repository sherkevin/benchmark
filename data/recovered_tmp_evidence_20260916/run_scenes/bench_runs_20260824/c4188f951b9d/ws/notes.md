# Notes

## Task 1: Predict QED of m05

m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain_len = 11. It has never been synthesized, so measure_qed returns probe_unavailable.

### Congeneric series measurements (scaffold 1):
| mol_id | chain_len | QED     |
|--------|-----------|---------|
| m15    | 2         | 0.7621  |
| m24    | 3         | 0.7919  |
| m28    | 4         | 0.7518  |
| m17    | 5         | 0.7498  |
| m22    | 6         | 0.7377  |

### Trend fitting:
- Quadratic (degree 2) on all 5 points: QED(chain=11) = **0.5416** (RMSE = 0.01188)
  - Coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
  - Negative quadratic coefficient captures accelerating QED decline for longer chains
- Linear (degree 1) on all 5 points: QED(chain=11) = 0.695 (RMSE = 0.01308)
- Linear on chains 3-6 only: QED(chain=11) = 0.6508 (RMSE = 0.00883)

Chose quadratic fit on all 5 points (0.5416) as it:
1. Uses all available data
2. Has good RMSE (0.01188)
3. Physically sensible: longer alkyl chains → higher lipophilicity → lower QED

**Predicted QED for m05: 0.5416**

## Task 2: Find lead molecule

### Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in same congeneric series as m05 (scaffold 1)

### Scaffold analysis:
- Scaffold 1 (m05's): COc1ccc(cc1)C(=O)N — EXCLUDED
- Scaffold 2: O=[N+]([O-])c1ccc(cc1)C(=O)N — PSA = 72.24 ≥ 40 ✓
- Scaffold 3: OCC(O)C(O)C(O)C(=O)N — PSA = 110.02 ≥ 40 ✓
- Scaffold 4: c1ccc(cc1)-c1ccc(cc1)C(=O)N — PSA = 29.1 < 40 ✗

### Measurements on candidates:
| mol_id | scaffold | chain | PSA    | QED    | QED≥0.6? |
|--------|----------|-------|--------|--------|----------|
| m09    | 2        | 2     | 72.24  | 0.5826 | No       |
| m02    | 2        | 3     | 72.24  | 0.6046 | **Yes**  |
| m12    | 2        | 4     | 72.24  | 0.4709 | No       |
| m32    | 2        | 5     | 72.24  | 0.4683 | No       |

### Selected lead: **m02**
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Scaffold 2 ≠ Scaffold 1 (not in m05's series) ✓

## Budget usage:
- 5 measurements on m05's series: 5 units
- 3 pages of pool listing: 3 units
- 4 measurements on Task 2 candidates: 4 units
- Total: 12/12 units
