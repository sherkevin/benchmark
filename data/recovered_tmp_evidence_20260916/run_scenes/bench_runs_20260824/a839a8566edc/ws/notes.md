# Notes

## Task Summary
- Task 1: Find QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - cannot measure directly, must predict from congeneric series
- Task 2: Find one molecule with QED >= 0.6, PSA >= 40, NOT in same congeneric series as m05
- Budget: 17 units total for list_pool (1 per page) and measure_qed (1 per measurement)

## Plan
1. List all molecules in the pool (33 molecules / 4 per page = ~9 pages)
2. Identify m05's congeneric series (same scaffold, different N-alkyl chain lengths)
3. Measure QED for molecules NOT in m05's series to find one meeting criteria
4. Predict m05's QED from its congeneric series trend

## Progress

### Catalogue Analysis (Budget used: 9 for listing)
Total molecules: 33

**m05 congeneric series** (scaffold: COc1ccc(cc1)C(=O)N-):
- m15: NCC (2C chain), PSA=38.33
- m24: NCCC (3C chain), PSA=38.33
- m28: NCCCC (4C chain), PSA=38.33
- m17: NCCCCC (5C chain), PSA=38.33
- m22: NCCCCCC (6C chain), PSA=38.33
- m05: NCCCCCCCCCCC (13C chain), PSA=38.33

### QED Measurements (Budget used: 8 more, total 17 - EXHAUSTED)
- m01: QED = 0.3187 (PSA=110.02) - too low
- m02: QED = 0.6046 (PSA=72.24) - MEETS ALL CRITERIA!
- m06: QED = 0.4011 (PSA=72.24) - too low
- m09: QED = 0.5826 (PSA=72.24) - just under 0.6
- m12: QED = 0.4709 (PSA=72.24) - too low
- m15: QED = 0.7621 (in m05 series)
- m17: QED = 0.7498 (in m05 series)
- m22: QED = 0.7377 (in m05 series)

### Task 2 Solution
**m02** satisfies all criteria:
1. QED = 0.6046 >= 0.6 ✓
2. PSA = 72.24 >= 40 ✓
3. NOT in m05 series (different scaffold: nitro-phenyl vs methoxy-phenyl) ✓

### Task 1: Predicting m05 QED from congeneric series trend
Congeneric series data:
- n=2 (m15): QED = 0.7621
- n=5 (m17): QED = 0.7498
- n=6 (m22): QED = 0.7377

Trend analysis:
- Drop from n=2 to n=5: 0.0123 over 3 carbons = ~0.0041/carbon
- Drop from n=5 to n=6: 0.0121 over 1 carbon = ~0.0121/carbon

The rate of decrease is accelerating. Using the most recent rate (n=5 to n=6):
- From n=6 to n=13: 7 carbons × 0.0121 ≈ 0.0847 drop
- Predicted QED for m05: 0.7377 - 0.0847 ≈ 0.653

Linear regression estimate: QED ≈ 0.70 (but this may overestimate due to non-linear trend)

Final prediction: ~0.65 (using the local trend from the longest measured chains)

## Final Answer
- **target_qed**: 0.65 (predicted for m05 from congeneric series trend)
- **lead_mol_id**: m02
- **lead_qed_measured**: 0.6046

## Budget Summary
- list_pool: 9 pages × 1 = 9 units
- measure_qed: 8 measurements × 1 = 8 units
- Total: 17/17 units (fully utilized)
