# Notes for Molecular Drug-likeness Task

## Initial Analysis
- There are 33 molecules in the pool with various properties
- Task 1: Need to predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) which is not synthesizable
- Task 2: Find a molecule with QED ≥ 0.6, PSA ≥ 40, and not in the same congeneric series as m05
- Budget: 12 measurements total

## Congeneric Series Analysis
From the grouping, m05 belongs to the series with scaffold "COc1ccc(cc1)C(=O)N" with chain length 11.
Other members of this series: m15, m24, m28, m17, m22 (with chain lengths 2, 3, 4, 5, 6 respectively).

## Task 1 Approach
Since m05 cannot be measured directly, I need to measure other molecules in the same series and predict based on the trend.

## Measurements Made
1. Measuring m22 (same scaffold, different chain length)
2. Measuring m17 (same scaffold, different chain length)
3. Measuring m28 (same scaffold, different chain length)
4. Measuring m24 (same scaffold, different chain length)
5. Measuring m15 (same scaffold, different chain length)

Then I'll use fit_trend to predict m05's QED based on chain length vs QED values.