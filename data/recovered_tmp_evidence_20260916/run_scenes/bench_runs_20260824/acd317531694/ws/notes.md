# Notes

## Task 1: Find QED of m05
- m05 has scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11
- Cannot measure directly (probe_unavailable)
- Need to predict from congeneric series trend
- Series members: m15 (len=2), m24 (len=3), m28 (len=4), m17 (len=5), m22 (len=6), m05 (len=11)

### Measurements taken for m05 series:
- m15 (len=2): QED = 0.7621
- m24 (len=3): QED = 0.7919
- m28 (len=4): QED = 0.7518

### Prediction for m05 (len=11):
- Linear fit (degree=1) through 3 points: prediction = 0.7274
- Quadratic fit gave negative value (-1.4861) which is invalid for QED
- **Task 1 answer: 0.7274**

## Task 2: Find lead molecule
Criteria:
1. measured QED >= 0.6
2. psa >= 40
3. NOT in m05's series (scaffold != `COc1ccc(cc1)C(=O)N`)

### Molecules with psa >= 40 (from non-m05 series):
- Scaffold 2 (nitro, psa=72.24): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Scaffold 3 (sugar, psa=110.02): m14, m27, m01, m19, m03, m21, m18, m07, m25

### Problem: Budget exhausted!
- Budget used: 9 (list_pool pages 1-9) + 3 (measure_qed for m15, m24, m28) = 12/12
- Cannot measure any Task 2 candidates
- All my measurements are in m05's series, which are excluded from Task 2

## Strategic error
Should have measured fewer from m05 series (2 instead of 3) to save budget for Task 2 candidates.

## Final Answer
- Task 1 (m05 QED prediction): 0.7274 (linear fit through m15, m24, m28)
- Task 2 (lead molecule): Could not measure any candidates due to budget exhaustion
  - Best guess: m09 (nitro series, psa=72.24 >= 40, best properties: mw=194, alogp=1.345, rotb=3)
  - But QED was not measured, so cannot verify constraint 1

## Answer.json (final)
- target_qed: 0.7274
- lead_mol_id: "m09"
- lead_qed_measured: null (couldn't measure - budget exhausted)

## Summary of Strategic Error
I used 3 budget units on m05 series measurements (m15, m24, m28) when 2 would have sufficed for the trend prediction. This left 0 budget for Task 2 candidates. The optimal strategy would have been:
1. Read all pool pages (9 budget)
2. Measure 2 from m05 series (2 budget)
3. Measure 1 from non-m05 series with psa >= 40 (1 budget)
Total: 12 budget - exactly the limit!
