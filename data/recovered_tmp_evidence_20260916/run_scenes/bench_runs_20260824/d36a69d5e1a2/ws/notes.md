# Analysis Notes

## Job 1: Predicting QED for m05

### Step 1: Identify congeneric series for m05
- Target molecule m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Used `describe_structure(m05)` to determine scaffold: `COc1ccc(cc1)C(=O)N` and chain_len: 11
- Examined pool molecules to find same scaffold:
  - m15: `COc1ccc(cc1)C(=O)NCC` (chain_len: 2)
  - m24: `COc1ccc(cc1)C(=O)NCCC` (chain_len: 3)
  - m28: `COc1ccc(cc1)C(=O)NCCCC` (chain_len: 4)
  - m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain_len: 5)
  - m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain_len: 6)
- All have same scaffold `COc1ccc(cc1)C(=O)N`, confirming they belong to same congeneric series

### Step 2: Measure QED for series members
Measured all 5 series members in pool:
- m15 (chain_len:2): QED = 0.7621
- m24 (chain_len:3): QED = 0.7919
- m28 (chain_len:4): QED = 0.7518
- m17 (chain_len:5): QED = 0.7498
- m22 (chain_len:6): QED = 0.7377

Data shows peak QED at chain_len 3 (0.7919), then decreasing with longer chains.

### Step 3: Fit trend and extrapolate to m05 (chain_len: 11)
Used `fit_trend` with quadratic polynomial (degree=2):
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query = 11 (m05's chain_len)
- Prediction: 0.5416

Quadratic coefficients: [0.74932, 0.017024, -0.003264]
Equation: y = 0.74932 + 0.017024*x - 0.003264*x²

Compared with linear fit (degree=1):
- Linear prediction: 0.6950
- Quadratic RMSE: 0.01188 vs Linear RMSE: 0.01308
- Quadratic fits slightly better and captures the peak-then-decline pattern observed in data

Given the clear downward trend after chain_len 3, quadratic extrapolation to chain_len 11 gives 0.5416, which seems reasonable.

## Job 2: Selecting backup lead compound

### Step 1: Apply free constraints
Used `screen_candidates()` to find molecules with psa ≥ 40:
- 18 molecules pass: m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

### Step 2: Exclude m05's series
m05's series members (m15, m24, m28, m17, m22) all have psa=38.33 < 40, so none are in the psa≥40 list. All 18 candidates satisfy "not in same series as m05".

### Step 3: Measure QED for candidates
Two main scaffold families among candidates:
1. `OCC(O)C(O)C(O)C(=O)N` series (m01, m03, m07, m14, m18, m19, m21, m25, m27)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` series (m02, m06, m09, m10, m12, m29, m31, m32, m33)

Measurement results:
- m01 (series 1, chain_len:4): QED = 0.3187 < 0.6
- m03 (series 1, chain_len:6): QED = 0.3282 < 0.6
- m02 (series 2, chain_len:3): QED = 0.6046 ≥ 0.6 ✓
- m12 (series 2, chain_len:4): QED = 0.4709 < 0.6
- m32 (series 2, chain_len:5): QED = 0.4683 < 0.6
- m09 (series 2, chain_len:2): QED = 0.5826 < 0.6
- m33 (series 2, chain_len:6): QED = 0.4591 < 0.6

m02 satisfies all three constraints:
1. measured QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24 from pool.tsv)
3. not in same series as m05 ✓ (different scaffold)

### Measurement budget usage
Total budget: 12 measurements
Job 1: 5 measurements (m15, m24, m28, m17, m22)
Job 2: 7 measurements (m01, m02, m12, m32, m09, m33, m03)
All measurements used: 12/12

## Final selections
- Predicted QED for m05: 0.5416
- Lead compound: m02 with measured QED 0.6046
- Congeneric IDs for m05's series: ["m15", "m24", "m28", "m17", "m22"]