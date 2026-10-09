# Notes

## Data Overview
- 33 molecules in pool.tsv, 4 scaffold groups identified via group_by_scaffold()
- m05 belongs to Scaffold A: COc1ccc(cc1)C(=O)N (methoxy-benzamide), chain_len=11
- Scaffold A members: m15(ch=2), m24(ch=3), m28(ch=4), m17(ch=5), m22(ch=6), m05(ch=11)

## Task 1: Predict QED of m05

### Measurements on Scaffold A (congeneric series)
| mol_id | chain_len | QED     |
|--------|-----------|---------|
| m15    | 2         | 0.7621  |
| m24    | 3         | 0.7919  |
| m28    | 4         | 0.7518  |
| m17    | 5         | 0.7498  |
| m22    | 6         | 0.7377  |

### Trend Analysis
- QED peaks at chain_len=3 (0.7919), then declines
- Quadratic fit (degree 2) on all 5 points: QED(11) = 0.5416 (RMSE=0.012)
- Linear fit (degree 1): QED(11) = 0.695 (RMSE=0.013) - ignores curvature
- Cubic fit (degree 3): QED(11) = 2.13 - clearly overfitting, discarded
- Linear on last 4 points: 0.651; linear on last 3: 0.704

### QED Component Analysis (Scaffold A)
Key changes chain 2 to 6:
- MW desirability: 0.33 to 0.68 (increasing)
- ALOGP desirability: 0.87 to 1.00 (increasing, plateau)
- ROTB desirability: 0.99 to 0.53 (dropping fast - main penalty)
- ALERTS: 0.84 to 0.64 (drops at chain 4, then flat)
- HBA, HBD, PSA, AROM: constant

For m05 (chain=11): MW=305.46, ALOGP=4.956, ROTB=12
- ROTB=12 very high, desirability likely near 0
- ALOGP=4.956 high, desirability ~0.68
- Quadratic prediction of 0.542 is physically consistent

### Predicted QED for m05: 0.54 (from quadratic fit)

## Task 2: Find lead (QED >= 0.6, PSA >= 40, not in m05 series)

PSA >= 40 means only Scaffold B (PSA=72.24) or Scaffold C (PSA=110.02).

| mol_id | scaffold | chain | QED    | PSA    | Pass?  |
|--------|----------|-------|--------|--------|--------|
| m09    | B        | 2     | 0.5826 | 72.24  | No     |
| m02    | B        | 3     | 0.6046 | 72.24  | Yes    |
| m12    | B        | 4     | 0.4709 | 72.24  | No     |
| m33    | B        | 6     | 0.4591 | 72.24  | No     |
| m14    | C        | 2     | 0.3237 | 110.02 | No     |
| m01    | C        | 4     | 0.3187 | 110.02 | No     |
| m03    | C        | 6     | 0.3282 | 110.02 | No     |

### Selected Lead: m02
- QED = 0.6046 >= 0.6 (check)
- PSA = 72.24 >= 40 (check)
- Scaffold B, not in m05's series (check)

## Budget: 12/12 measurements used
