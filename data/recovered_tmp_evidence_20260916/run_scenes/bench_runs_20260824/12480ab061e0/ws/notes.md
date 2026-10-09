# Notes for Medicinal Chemistry Task

## Summary
- Total molecules: 33
- Budget: 17 units (9 used so far for listing all pages, 8 remaining)
- Molecules in same series as m05 (scaffold: COc1ccc(cc1)C(=O)N with varying alkyl chain length):
  - m15: C2 chain
  - m24: C3 chain  
  - m28: C4 chain
  - m17: C5 chain
  - m22: C6 chain
  - m05: C11 chain (target for prediction)

## Task 1: Predict QED for m05
Need to measure QED for some molecules in the same series to establish trend.

## Task 2: Find lead molecule
Requirements:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same series as m05

## Strategy
1. Measure QED for some molecules in m05's series to establish trend
2. Predict m05 QED from trend
3. Find candidate molecules for Task 2 (psa ≥ 40, not in m05 series)
4. Measure QED of promising candidates

## QED Measurements for m05 Series
### Data points:
- m15 (C2): 0.7621 (chain length 2)
- m24 (C3): 0.7919 (chain length 3)  
- m28 (C4): 0.7518 (chain length 4)
- m17 (C5): 0.7498 (chain length 5)
- m22 (C6): 0.7377 (chain length 6)
- m05 (C11): unknown (chain length 11, needs prediction)

### Trend analysis:
- C2→C3: +0.0298
- C3→C4: -0.0401
- C4→C5: -0.0020
- C5→C6: -0.0121

Average decrease from C3 to C6: (0.7919 - 0.7377) / 3 = 0.01807 per CH2 unit
From C6 to C11: 5 CH2 units × 0.01807 = 0.09035 decrease
Prediction 1: 0.7377 - 0.09035 = 0.64735 ≈ 0.647

Alternative: Linear regression on last 3 points (C4-C6):
C4:4→0.7518, C5:5→0.7498, C6:6→0.7377
Slope ≈ -0.01405, intercept ≈ 0.809
Prediction 2: 0.809 - 0.01405×11 = 0.65445 ≈ 0.654

Average prediction: (0.647 + 0.654) / 2 = 0.6505 ≈ 0.651

## Task 2 Candidate Search
### Tested candidates:
- m01: QED=0.3187 (<0.6) ❌
- m02: QED=0.6046 (≥0.6), psa=72.24 (≥40), scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's COc1ccc(cc1)C(=O)N) ✅

## Final Answers
### Task 1: Predicted QED for m05
- Based on trend analysis of congeneric series: 0.651
- Rationale: Average of two prediction methods (0.647 and 0.654)

### Task 2: Lead molecule
- Selected: m02
- Measured QED: 0.6046 (≥0.6)
- PSA: 72.24 (≥40)
- Not in same series as m05: Different scaffold (nitrobenzene vs methoxybenzene)

## Budget Status
- Total budget: 17 units
- Used: 16 units (9 list_pool + 7 measure_qed)
- Remaining: 1 unit