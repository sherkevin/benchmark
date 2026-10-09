# Notes: Predicting QED of m05

## Target
- Molecule: m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- 4-methoxybenzamide with an 11-carbon N-alkyl chain (undecyl)
- Scaffold: COc1ccc(cc1)C(=O)N- with varying N-alkyl chain lengths

## Search Strategy
- Searched pool for scaffold substring `COc1ccc(cc1)C(=O)N` - found 6 matches in 2 pages (cost: 2)
- Measured QED for the 5 non-target congeners (cost: 5)
- Total budget used: 7/12

## Congeneric Series Data

| Mol ID | Chain C# | MW     | ALogP | RotB | QED    |
|--------|----------|--------|-------|------|--------|
| m15    | 2        | 179.22 | 1.445 | 3    | 0.7621 |
| m24    | 3        | 193.25 | 1.835 | 4    | 0.7919 |
| m28    | 4        | 207.27 | 2.225 | 5    | 0.7518 |
| m17    | 5        | 221.30 | 2.615 | 6    | 0.7498 |
| m22    | 6        | 235.33 | 3.005 | 7    | 0.7377 |
| m05    | 11       | 305.46 | 4.956 | 12   | ???    |

## Observed Trend
- QED peaks at C3 (0.7919) then declines monotonically C3 to C6
- Per-carbon decline: C3->C4 = -0.040, C4->C5 = -0.002, C5->C6 = -0.012
- Decline expected to ACCELERATE at longer chains because:
  - ALogP increases linearly (~0.39/CH2); at C11, ALogP=4.956 (well outside drug-like range ~1-3)
  - RotB increases linearly; at C11, RotB=12 (above ~9 threshold)
  - QED desirability functions penalize these properties nonlinearly (roughly exp(-x^2))

## Model: Least-Squares Quadratic on All 5 Data Points
Using centered coordinates u = chain_length - 4:

  QED(u) = -0.003264 * u^2 - 0.00909 * u + 0.76519

### Fit quality:
- C2 (u=-2): pred 0.770, actual 0.762 (err +0.008)
- C3 (u=-1): pred 0.771, actual 0.792 (err -0.021)
- C4 (u= 0): pred 0.765, actual 0.752 (err +0.013)
- C5 (u=+1): pred 0.753, actual 0.750 (err +0.003)
- C6 (u=+2): pred 0.734, actual 0.738 (err -0.004)
- RMSE ~ 0.012

### Extrapolation to C11 (u = 7):
  QED = -0.003264*(49) - 0.00909*(7) + 0.76519
      = -0.15994 - 0.06363 + 0.76519
      = 0.542

### Why quadratic, not linear:
- Linear fit on all 5 pts gives QED ~ 0.695, ignoring curvature and acceleration
- QED desirability functions are nonlinear (Gaussian-like), so penalties compound and accelerate
- At C11, ALogP=4.956 and RotB=12 are far outside drug-like ranges

### Cross-checks with other nonlinear models:
- Quadratic on C4-C6 only: QED ~ 0.526
- Quadratic in logit space (all 5 pts): QED ~ 0.487
- Exponential decay on C3-C6: QED ~ 0.659 (less acceleration, likely underestimates penalty)
- All accelerating-decline models: QED in range 0.49-0.54

## Predicted QED for m05: 0.54
