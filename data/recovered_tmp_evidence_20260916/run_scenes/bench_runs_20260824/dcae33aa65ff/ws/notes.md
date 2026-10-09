# Notes

## Task 1: Predict QED for m05

m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11. Since m05 cannot be measured directly, I predicted its QED from its congeneric series.

### Measurements from m05's series:
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

### Trend fitting:
- Quadratic fit (degree=2): prediction at chain_len=11 = 0.5416 (RMSE=0.01188)
- Linear fit (degree=1): prediction at chain_len=11 = 0.695 (RMSE=0.01308)

The quadratic fit has lower RMSE, so I used **0.5416** as the predicted QED for m05.

## Task 2: Find a lead molecule

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in same congeneric series as m05 (i.e., NOT scaffold `COc1ccc(cc1)C(=O)N`)

### PSA analysis:
- m05's series (COc1ccc...): psa=38.33 (all below 40, excluded anyway)
- O=[N+]... series: psa=72.24 (valid candidates)
- OCC(O)... series: psa=110.02 (valid candidates)
- c1ccc... series: psa=29.1 (below 40, excluded)

### Measurements for Task 2 candidates:
- m02 (O=[N+]... series, psa=72.24): QED = 0.6046 ✓
- m12 (O=[N+]... series, psa=72.24): QED = 0.4709 ✗
- m01 (OCC(O)... series, psa=110.02): QED = 0.3187 ✗
- m27 (OCC(O)... series, psa=110.02): QED = 0.333 ✗

### Selected lead molecule: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

## Budget used: 9 measurements (out of 12)
