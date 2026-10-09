# Notes for QED Prediction and Lead Selection

## Initial Analysis
- Total pool: 33 molecules
- 4 scaffold groups identified via `group_by_scaffold()`
- m05 is in scaffold group 1: "COc1ccc(cc1)C(=O)N" with chain length 11
- Other members in same series as m05: m15 (2), m24 (3), m28 (4), m17 (5), m22 (6)
- Need to predict m05 QED using trend from its congeneric series
- Budget: 12 units total for list_pool and measure_qed
- Budget used so far: 2 (pages 1-2)

## Potential Task 2 candidates (PSA ≥ 40, not in m05's series):
- m01: PSA 110.02 (scaffold 3)
- m02: PSA 72.24 (scaffold 2) 
- m03: PSA 110.02 (scaffold 3)
- m06: PSA 72.24 (scaffold 2)
- m07: PSA 110.02 (scaffold 3)

## Task 1 Data Collected (m05 series):
- m15 (chain_len 2): QED 0.7621
- m24 (chain_len 3): QED 0.7919
- m28 (chain_len 4): QED 0.7518
- m17 (chain_len 5): QED 0.7498
- m22 (chain_len 6): QED 0.7377
- m05 (chain_len 11): need to predict QED

## Task 1 Prediction:
- Using linear fit (degree 1) with all 5 points: predicted QED for m05 = 0.695
- Quadratic fit (degree 2) gives 0.5416, but seems too low due to extrapolation
- Linear extrapolation seems more reasonable given the trend

## Task 2 Results:
- Measured m02: QED 0.6046, PSA 72.24, scaffold 2 (not m05's series)
- m02 satisfies all conditions: QED ≥ 0.6, PSA ≥ 40, not in m05's series
- Measured m01: QED 0.3187 (too low)
- Other potential candidates not measured due to budget constraints

## Budget Status:
- Total budget: 12 units
- Used: 12 (5 pages + 7 measurements)
- Breakdown: 
  - Pages: 5 (pages 1-5)
  - Measurements: 7 (m15, m24, m28, m17, m22, m02, m01)

## Final Decision:
- Task 1: m05 predicted QED = 0.695
- Task 2: Lead molecule = m02 with measured QED = 0.6046

