# Task Notes

## Job 1: Predicting QED of m05

### Identifying m05's Congeneric Series
Using `describe_structure()` on m05 revealed:
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11 carbons

I examined all SMILES in the pool and identified these molecules sharing the same scaffold:
- m15: chain_len=2
- m24: chain_len=3  
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6
- m05: chain_len=11 (target, not synthesized)

### Measuring QED of the Series
I measured QED for all available series members (5 measurements):
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

### Trend Analysis
The QED values show:
- Peak at chain_len=3 (0.7919)
- Decrease as chain length increases beyond 3 carbons
- Pattern suggests curvature, not linear relationship

### Extrapolation to m05 (chain_len=11)
Using `fit_trend()`:
- Quadratic fit (degree=2): predicted QED=0.5416 (RMSE=0.01188)
- Linear fit (degree=1): predicted QED=0.695 (RMSE=0.01308)

The quadratic fit has lower RMSE and captures the observed curvature in the data. Since m05's chain length (11) is well beyond the measured range (2-6), extrapolation is inherently uncertain. However, given the clear downward trend beyond chain_len=3 and better fit of quadratic model, I selected the quadratic prediction: **0.5416**.

## Job 2: Selecting Backup Lead Compound

### Applying Constraints
Three constraints must be satisfied:
1. Measured QED ≥ 0.6
2. psa ≥ 40 (free descriptor)
3. Not in same series as m05

### Screening Process
1. Used `screen_candidates()` to find molecules with psa ≥ 40: 18 molecules passed
2. Excluded molecules in m05's series (m15, m24, m28): 15 candidates remaining
3. Strategically measured QED across different series with remaining budget

### Measurement Results
Remaining budget after measuring m05's series: 7 measurements
Measured:
- m01 (sugar-like series): QED=0.3187 ❌
- m02 (nitroaromatic series): QED=0.6046 ✓
- m06 (nitroaromatic series): QED=0.4011 ❌
- m09 (nitroaromatic series): QED=0.5826 ❌
- m12 (nitroaromatic series): QED=0.4709 ❌
- m07 (sugar-like series): QED=0.3233 ❌
- m14 (sugar-like series): QED=0.3237 ❌

### Lead Selection
**m02** satisfies all constraints:
1. Measured QED=0.6046 (≥ 0.6) ✓
2. psa=72.24 (≥ 40) ✓
3. Different scaffold (`O=[N+]([O-])c1ccc(cc1)C(=O)N`) than m05 ✓

### Alternative Considerations
I explored the nitroaromatic series further (m06, m09, m12) but found lower QED values. The sugar-like series (m01, m07, m14) had consistently low QED (<0.4). m02 represents the best available candidate given measurement constraints.

## Budget Usage
Total measurements: 12/12
- Job 1 (m05 series): 5 measurements
- Job 2 (lead screening): 7 measurements

All measurements were necessary to establish trend for extrapolation and verify lead candidate suitability.