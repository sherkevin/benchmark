# Notes - Medicinal Chemistry Task

## Job 1: Predict QED of m05

### Target molecule
- m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- Chain length: 11
- Never synthesized - must predict from congeneric trend

### Congeneric series identified (same scaffold, different chain lengths)
| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15    | 2         | COc1ccc(cc1)C(=O)NCC |
| m24    | 3         | COc1ccc(cc1)C(=O)NCCC |
| m28    | 4         | COc1ccc(cc1)C(=O)NCCCC |
| m17    | 5         | COc1ccc(cc1)C(=O)NCCCCC |
| m22    | 6         | COc1ccc(cc1)C(=O)NCCCCCC |

### Measured QED values
| chain_len | QED    |
|-----------|--------|
| 2         | 0.7621 |
| 3         | 0.7919 |
| 4         | 0.7518 |
| 5         | 0.7498 |
| 6         | 0.7377 |

### Trend analysis
QED peaks at chain length 3, then decreases steadily. The relationship is non-linear (slight curvature).

**Degree-2 polynomial fit on all 5 points:**
- QED(x) = 0.74932 + 0.017024*x - 0.003264*x^2
- RMSE on training points: 0.01188
- **Prediction at chain 11: QED = 0.5416**

Comparison with other fits:
- Linear on all 5 points: 0.695 (RMSE 0.01308) - task says don't assume straight line
- Linear on chain 3-6 only: 0.6508 (RMSE 0.00883) - ignores the rise at chain 2-3
- Quadratic on chain 3-6 only: 0.9378 - unrealistic upward curvature from extrapolation

The quadratic on all 5 points is the best choice because:
1. It uses all available data from the series
2. It captures the non-linear shape (rise then decline)
3. The prediction is physically sensible (high logP at chain 11 -> lower QED)
4. m05's alogp = 4.956, which is well above the optimal range, supporting a lower QED

**Final prediction: QED(m05) = 0.5416**

## Job 2: Pick a backup lead compound

### Constraints
1. Measured QED >= 0.6
2. PSA >= 40 (free check)
3. Not in m05's series (free check)

### Screening process
- Used screen_candidates() to get 18 molecules with PSA >= 40
- Removed m05 series members (none passed PSA screen anyway - their PSA is 38.33)
- Remaining candidates belong to sugar series (PSA=110.02) or nitro series (PSA=72.24)
- Biphenyl series all have PSA=29.1, excluded

### Candidates measured
1. **m09** (nitro series, chain 2, alogp=1.345, MW=194.19): QED = 0.5826 - FAIL (below 0.6)
2. **m02** (nitro series, chain 3, alogp=1.735, MW=208.22): QED = 0.6046 - PASS

### Selected lead: m02
- Measured QED: 0.6046 >= 0.6 (check)
- PSA: 72.24 >= 40 (check)
- Series: nitro (O=[N+]([O-])c1ccc(cc1)C(=O)N), not m05's series (check)

## Budget usage
- 9 pages of list_pool: 9 units
- 5 QED measurements (congeners): 5 units
- 2 QED measurements (lead search): 2 units
- Total: 16 out of 17 budget used
