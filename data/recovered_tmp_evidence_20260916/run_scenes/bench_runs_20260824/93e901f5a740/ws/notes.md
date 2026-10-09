# Notes for Medicinal Chemistry Task

## Initial Analysis

**Data Overview:**
- 33 molecules in pool.tsv
- 4 different scaffolds identified via group_by_scaffold()
- m05 belongs to scaffold "COc1ccc(cc1)C(=O)N" with chain length 11
- Other members in same series: m15 (2), m24 (3), m28 (4), m17 (5), m22 (6)

**Task 1:**
Need to predict QED for m05 using trend analysis from its congeneric series.
Budget:第七measurements total for both tasks.

**Task 2:**
Need to find one molecule with:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. Not in same congeneric series as m05 (different scaffold)

**Plan:**
1. Measure QED for some molecules in m05's series to establish trend
2. Use fit_trend() to predict m05's QED
3. Search for candidate molecules for Task 2
4. Measure candidates until finding one meeting all criteria

**Constraints:**
- Must be careful with budget (12 measurements total)
- Need to consider psa ≥ 40 requirement for Task 2

## Measurements Taken

**m05 Series Measurements:**
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377

**Budget Used:** 5 measurements (7 remaining)

## Task 1 Prediction

**m05 QED Prediction:**
- Using quadratic fit (degree=2) with chain lengths [2,3,4,5,6] and corresponding QEDs
- Prediction for chain_len 11 (m05): 0.5416
- Linear fit gives 0.695 but has higher RMSE (0.01308 vs 0.01188)
- Given decreasing trend from peak at chain_len 3, quadratic prediction seems more reasonable

**Task 1 Answer:** m05 predicted QED = 0.5416

## Task 2 Planning

Need to find molecule with:
1. QED ≥ 0.6
2. psa ≥ 40
3. Different scaffold from m05 (not in "COc1ccc(cc1)C(=O)N" series)

**Remaining Budget:** 7 measurements

**Candidate Analysis:**
- Groups with psa ≥ 40: Group 2 (psa=72.24) and Group 3 (psa=110.02)
- Group 2: m09, m02, m12, m32, m33, m29, m31, m06, m10
- Group 3: m14, m27, m01, m19, m03, m21, m18, m07, m25

**All Measurements Taken:**

**Task 1 Series (m05 scaffold):**
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377

**Task 2 Candidates (Group 2 - psa=72.24):**
- m09 (chain_len: 2): QED = 0.5826 (< 0.6)
- m02 (chain_len: 3): QED = 0.6046 (≥ 0.6) ✓
- m12 (chain_len: 4): QED = 0.4709 (< 0.6)
- m32 (chain_len: 5): QED = 0.4683 (< 0.6)
- m33 (chain_len: 6): QED = 0.4591 (< 0.6)

**Task 2 Candidates (Group 3 - psa=110.02):**
- m27 (chain_len: 3): QED = 0.333 (< 0.6)
- m01 (chain_len: 4): QED = 0.3187 (< 0.6)

**Budget Status:** All 12 measurements used

**Best Candidate Found:** m02 meets all criteria:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Different scaffold from m05 ✓

No other molecule measured has QED ≥ 0.6 except m02.

## Summary

**Task 1 Completed:**
- Predicted m05 QED using quadratic trend analysis from its congeneric series
- Used 5 measurements from same scaffold (m15, m24, m28, m17, m22)
- Prediction: QED = 0.5416

**Task 2 Completed:**
- Found m02 as lead candidate
- Measured QED = 0.6046 (≥ 0.6 requirement)
- psa = 72.24 (≥ 40 requirement)
- Different scaffold from m05 (Group 2 vs Group 1)

**All requirements satisfied with 12 measurement budget fully utilized.**
