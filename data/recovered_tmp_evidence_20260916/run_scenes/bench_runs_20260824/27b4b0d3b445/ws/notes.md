# Notes for Medicinal Chemistry Task

## Understanding the Problem

- Pool contains 33 molecules with descriptors but no QED values
- Need to complete two tasks with shared budget of 12 QED measurements
- Task 1: Predict QED of m05 (chain_len 11 in COc1ccc(cc1)C(=O)N scaffold series)
- Task 2: Find molecule with QED ≥ 0.6, PSA ≥ 40, and not in same series as m05

## Strategy

1. First, measure QED of several molecules in m05's series to establish trend
2. Use fit_trend to predict m05's QED
3. Then search for candidate molecules for Task 2
4. Prioritize molecules with PSA ≥ 40 as potential candidates

## Initial Observations

From group_by_scaffold:
- m05's series (scaffold COc1ccc(cc1)C(=O)N): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
- Need to measure some of these to predict m05's QED

## Budget Planning
- 12 total measurements available
- Need to reserve some for Task 2 candidates
- Let's start with measuring m05 series members

## Measurements Taken So Far
1. m15 (chain_len 2): QED = 0.7621
2. m24 (chain_len 3): QED = 0.7919  
3. m28 (chain_len 4): QED = 0.7518
4. m17 (chain_len 5): QED = 0.7498
5. m22 (chain_len 6): QED = 0.7377

Budget remaining: 7 measurements (5 used)

## Initial Prediction for m05
- Using chain lengths 2-5: predicted QED = 0.2689 (seems low due to extrapolation)
- Using chain lengths 2-6 (quadratic): predicted QED = 0.5416
- Using chain lengths 2-6 (linear): predicted QED = 0.695

## Task 2 Analysis
- Need molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's series
- Checking PSA values from data:
  - Series 1 (m05's): PSA = 38.33 (< 40) - disqualifies all in this series
  - Series 2 (O=[N+]([O-])...): PSA = 72.24 (≥ 40) - potential candidates
  - Series 3 (OCC(O)C(O)C(O)C(=O)N): PSA = 110.02 (≥ 40) - potential candidates  
  - Series 4 (c1ccc(cc1)-c1ccc(cc1)C(=O)N): PSA = 29.1 (< 40) - disqualifies all
- So candidates must come from Series 2 or 3
- Need to measure QED of molecules from these series until finding one with QED ≥ 0.6

## Measurements for Task 2 Candidates
### Series 2 measurements:
- m02: QED = 0.6046 ✓ (meets criteria: ≥ 0.6, PSA ≥ 40, not m05 series)
- m12: QED = 0.4709 ✗ (< 0.6)
- m09: QED = 0.5826 ✗ (< 0.6)
- m32: QED = 0.4683 ✗ (< 0.6)

### Series 3 measurements:
- m14: QED = 0.3237 ✗ (< 0.6)
- m27: QED = 0.3330 ✗ (< 0.6)
- m01: QED = 0.3187 ✗ (< 0.6)

## Best Candidate Found
- m02 meets all criteria: QED = 0.6046 ≥ 0.6, PSA = 72.24 ≥ 40, not in m05's series
- No other molecule measured meets all criteria
- Budget used: all 12 measurements

## Final Decisions
### Task 1: m05 QED Prediction
- Using quadratic fit (degree=2) with chain lengths 2-6
- Predicted QED: 0.5416
- Rationale: Quadratic fit reasonable given decreasing trend, linear prediction (0.695) seems too high for chain_len 11

### Task 2: Lead Molecule
- Selected: m02
- Measured QED: 0.6046
- Validates all criteria: 
  1. QED = 0.6046 ≥ 0.6 ✓
  2. PSA = 72.24 ≥ 40 ✓  
  3. Not in same series as m05 (different scaffold) ✓