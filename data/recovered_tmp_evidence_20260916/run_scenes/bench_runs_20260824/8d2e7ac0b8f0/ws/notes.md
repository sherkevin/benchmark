# Notes: Predicting QED of m05

## Task
Predict the QED of molecule m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) which has never been synthesized.

## Step 1: Search for congeners
- Searched for scaffold substring COc1ccc(cc1)C(=O)N (cost: 2 pages, 2 budget units)
- Found 6 molecules total in the same congeneric series

### Series members (all share scaffold COc1ccc(cc1)C(=O)N-, differ only in N-alkyl chain length):

| mol_id | N-alkyl chain | MW     | AlogP | ROTB |
|--------|--------------|--------|-------|------|
| m15    | 2C (ethyl)   | 179.22 | 1.445 | 3    |
| m24    | 3C (propyl)  | 193.25 | 1.835 | 4    |
| m28    | 4C (butyl)   | 207.27 | 2.225 | 5    |
| m17    | 5C (pentyl)  | 221.30 | 2.615 | 6    |
| m22    | 6C (hexyl)   | 235.33 | 3.005 | 7    |
| m05    | 11C (undecyl)| 305.46 | 4.956 | 12   |

Per-CH2 increments: delta-MW ~14.03, delta-AlogP ~0.39, delta-ROTB = 1.

## Step 2: QED Measurements (5 measurements, 5 budget units)

| mol_id | Chain length (n) | QED (measured) |
|--------|-----------------|-----------------|
| m15    | 2               | 0.7621          |
| m24    | 3               | 0.7919          |
| m28    | 4               | 0.7518          |
| m17    | 5               | 0.7498          |
| m22    | 6               | 0.7377          |

Total budget used: 2 (search) + 5 (measure) = 7 of 12

## Step 3: Trend Analysis

Observed pattern:
- QED peaks at n=3 (0.7919)
- QED decreases from n=3 to n=6
- Drops: n3->4: -0.0401, n4->5: -0.002, n5->6: -0.012

Fitted model: Quadratic on ln(QED)
ln(QED) = -0.0043*n^2 + 0.02244*n - 0.28882

Fit quality:
n=2: predicted 0.770, actual 0.762 (error +0.008)
n=3: predicted 0.771, actual 0.792 (error -0.021)
n=4: predicted 0.765, actual 0.752 (error +0.013)
n=5: predicted 0.753, actual 0.750 (error +0.003)
n=6: predicted 0.734, actual 0.738 (error -0.004)

Physical validation: Modeled d_ALOGP and d_ROTB as Gaussian desirability functions.
For any reasonable AlogP peak position (2.0-3.0), the extrapolated QED converges to ~0.570.

## Step 4: Extrapolation to m05 (n=11)

ln(QED)(11) = -0.0043*121 + 0.02244*11 - 0.28882 = -0.56228
QED(m05) = exp(-0.56228) = 0.570

Cross-check with other models:
- Linear fit (all 5 pts): 0.695 (too high, ignores nonlinear penalties)
- Quadratic on QED (all 5 pts): 0.542 (consistent)
- Exponential decay from peak: 0.656 (conservative upper bound)
- Quadratic on last 3 pts: 0.526 (aggressive lower bound)

Final prediction: QED(m05) = 0.57
