# Notes: QED Prediction and Lead Selection

## Initial Analysis
- Target molecule: m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC)
- Scaffold: COc1ccc(cc1)C(=O)N
- Chain length: 11 carbons
- Target has never been synthesized (probe_unavailable)

## Approach
1. Find all pool molecules with same scaffold as m05 (congeneric series)
2. Measure QED for those molecules to establish trend
3. Extrapolate to chain length 11 using fit_trend
4. Find lead molecule meeting all constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in same series as m05

## Budget: 17 measurements total (shared between both jobs)

## Step10617: Explore pool to identify m05 congeners
Let me examine the pool systematically.

### Found congeners (same scaffold as m05):
- m15: chain length 2 (confirmed)
- m24: chain length 3 (confirmed)
- m28: chain length 4 (confirmed)
- m17: chain length 5 (confirmed)
- m22: chain length 6 (confirmed)
- m05: chain length 11 (target)

No other molecules in pool have scaffold "COc1ccc(cc1)C(=O)N" based on SMILES inspection.

### Other scaffolds identified:
1. OCC(O)C(O)C(O)C(=O)N... (polyol series)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N... (nitrobenzene series)
3. c1ccc(cc1)-c1ccc(cc1)C(=O)N... (biphenyl series)

### Budget used: 9 (from list_pool pages 1-9)
### Budget remaining: 8

## Step 2: Measure QED of m05 congeners to establish trend
Measured QED values:
- m15 (chain 2): 0.7621
- m24 (chain 3): 0.7919
- m28 (chain 4): 0.7518
- m17 (chain 5): 0.7498
- m22 (chain 6): 0.7377

Pattern: QED peaks at chain length 3 (0.7919) then decreases gradually.

### Trend fitting for m05 prediction (chain length 11):
Using quadratic fit (degree 2) on all 5 points:
- Predicted QED for chain length 11: 0.5416
- RMSE: 0.01188
- Equation: QED = 0.74932 + 0.017024*x - 0.003264*x²

Linear fit (degree 1) gives prediction 0.695, but has higher RMSE (0.01308) and doesn't capture the peak.

Quadratic fit is preferred as it better captures the observed trend (peak at chain 3, then decline).

## Step 4: Find lead compound

### Candidate screening:
18 molecules pass psa ≥ 40 constraint.

### Measurements:
1. m01 (polyol C4): QED = 0.3187 < 0.6 ❌
2. m02 (nitrobenzene C3): QED = 0.6046 ≥ 0.6 ✓

m02 satisfies all constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in m05 series (scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N) ✓

### Budget used for lead measurements: 2
### Total budget used: 16 (9 list_pool + 5 congener measurements + 2 lead measurements)
### Budget remaining: 1

## Summary
1. **m05 QED prediction**: 0.5416 (extrapolated from quadratic trend fit to congener series data)
2. **Lead compound**: m02
   - Measured QED: 0.6046 ≥ 0.6 ✓
   - psa: 72.24 ≥ 40 ✓  
   - Not in m05 series: scaffold O=[N+]([O-])c1ccc(cc1)C(=O)N ✓
3. **Congener series members**: m15, m24, m28, m17, m22 (all pool molecules sharing scaffold COc1ccc(cc1)C(=O)N)

All constraints satisfied within budget (16/17 measurements used).

## Step 3: Screen for lead candidates
18 molecules pass psa ≥ 40 constraint. None of m05 series molecules have psa ≥ 40, so all 18 automatically satisfy "not in same series" constraint.