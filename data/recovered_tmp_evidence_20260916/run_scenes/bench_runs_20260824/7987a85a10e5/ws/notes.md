# Notes for QED Prediction and Lead Selection

## Job 1: Predicting QED of m05

### Step 1: Identify m05 congeners
- m05 scaffold: "COc1ccc(cc1)C(=O)N" (chain length 11)
- Found congeners in pool:
  - m15: chain length 2
  - m24: chain length 3
  - m28: chain length 4
  - m17: chain length 5
  - m22: chain length 6

### Step 2: Measure QED of congeners
Measured all 5 congeners found in pool:
.#### Step 2: Measure QED of congeners
Measured all 5 congeners found in pool:
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

Budget used: 17 out of 17 (all budget used).

### Step 4: Lead compound selection
Requirements:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free)
3. not in same series as m05

Using `screen_candidates()` found 18 molecules with psa ≥ 40.
Measured QED of potential leads:
- m01: QED = 0.3187 (< 0.6)
- m02: QED = 0.6046 (≥ 0.6) ✓
- m09: QED = 0.5826 (< 0.6)

m02 satisfies all constraints:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Scaffold "O=[N+]([O-])c1ccc(cc1)C(=O)N" ≠ "COc1ccc(cc1)C(=O)N" ✓

## Final Answers
### Job 1: m05 QED prediction
- Congeners in pool: m15, m24, m28, m17, m22
- Measured QEDs: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377
- Quadratic fit predicts QED = 0.5416 for chain length 11 (m05)
- Selected quadratic over linear due to curvature in data (peak at chain length 3)

### Job 2: Lead compound selection
- Selected m02 as lead
- QED measured: 0.6046 ≥ 0.6
- psa = 72.24 ≥ 40
- Different series from m05 (nitrobenzene vs methoxybenzene scaffold)

### Step 3: Fit trend and extrapolate
Chain lengths: [2, 3, 4, 5, 6]
QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Trend analysis:
1. Linear fit (degree 1): QED = 0.79502 - 0.00909*x
   - Predicts QED(11) = 0.695
   - RMSE = 0.01308

2. Quadratic fit (degree 2): QED = 0.74932 + 0.017024*x - 0.003264*x²
   - Predicts QED(11) = 0.5416
   - RMSE = 0.01188 (slightly better fit)

The data shows a peak at chain length 3 (0.7919) followed by decrease. Quadratic captures this curvature better. Extrapolating quadratic to chain length 11 gives QED ≈ 0.5416.