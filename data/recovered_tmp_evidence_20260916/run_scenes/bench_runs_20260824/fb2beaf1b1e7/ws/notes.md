# Notes on QED Prediction Task

## Identifying Congeneric Series for m05

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxybenzene (`COc1ccc(cc1)`)
- Amide linkage: `C(=O)N`
- Long alkyl chain: `CCCCCCCCCCC` (appears to be 12 carbons)
- rotb = 12

Same series (methoxybenzene scaffold) molecules in pool:
1. m15: `COc1ccc(cc1)C(=O)NCC` (2 carbons) - rotb=3, psa=38.33
2. m24: `COc1ccc(cc1)C(=O)NCCC` (3 carbons) - rotb=4, psa=38.33  
3. m28: `COc1ccc(cc1)C(=O)NCCCC` (4 carbons) - rotb=5, psa=38.33
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons) - rotb=6, psa=38.33
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons) - rotb=7, psa=38.33

Pattern: rotb = chain_length + 1? 
- m15: chain_length=2, rotb=3
- m24: chain_length=3, rotb=4
- m28: chain_length=4, rotb=5
- m17: chain_length=5, rotb=6
- m22: chain_length=6, rotb=7

So m05 with rotb=12 likely has chain_length=11 carbons.

## Strategy for Job 1 (Predict m05 QED)

1. Measure QED for these 5 congeneric molecules to establish trend
2. Fit curve to see how QED changes with chain length
3. Extrapolate to m05 (chain_length=11)

We have 12 measurements total for both jobs. Need to budget carefully.

## Strategy for Job 2 (Find lead compound)

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (from pool.tsv)
3. not in same series as m05 (methoxybenzene series)

First filter by psa ≥ 40:
- Check pool.tsv for molecules with psa ≥ 40
- Then exclude methoxybenzene series (all have psa=38.33)
- Then measure promising candidates

Potential scaffolds with higher psa:
- Nitrobenzene series: all have psa=72.24
- Polyol series: all have psa=110.02
- Biphenyl series: all have psa=29.1 (too low)

So candidates must come from nitrobenzene or polyol series.

## Measurement Plan

Total budget: 12 measurements

For Job 1: Need to measure at least some of the 5 methoxybenzene series to establish trend. Could measure all 5 = 5 measurements.

For Job 2: Need to find lead with QED ≥ 0.6. Should measure promising candidates from nitrobenzene/polyol series.

Will start with Job 1 measurements.

## QED Measurements for Methoxybenzene Series

Measured QED values:
1. m15 (2 carbons): QED = 0.7621
2. m24 (3 carbons): QED = 0.7919
3. m28 (4 carbons): QED = 0.7518
4. m17 (5 carbons): QED = 0.7498
5. m22 (6 carbons): QED = 0.7377

Chain lengths (estimated from rotb pattern):
- m15: chain_length = 2 (rotb=3)
- m24: chain_length = 3 (rotb=4)
- m28: chain_length = 4 (rotb=5)
- m17: chain_length = 5 (rotb=6)
- m22: chain_length = 6 (rotb=7)
- m05: chain_length = 11 (rotb=12, from pattern rotb = chain_length + 1)

Observations:
- QED peaks at chain_length=3 (m24: 0.7919)
- Then decreases as chain length increases
- Trend appears to be downward after length=3

Budget status: 5 measurements used, 7 remaining.

## Job 2: Searching for Lead Compound

Constraints:
1. QED ≥ 0.6 (must measure)
2. psa ≥ 40 (from pool.tsv)
3. Not in methoxybenzene series

Filter: psa ≥ 40 eliminates methoxybenzene (psa=38.33) and biphenyl (psa=29.1).
Candidates: nitrobenzene series (psa=72.24) and polyol series (psa=110.02).

Measured candidates:
- m12 (nitrobenzene, chain_length=4): QED=0.4709 < 0.6
- m32 (nitrobenzene, chain_length=5): QED=0.4683 < 0.6
- m01 (polyol, chain_length=4): QED=0.3187 < 0.6
- m19 (polyol, chain_length=5): QED=0.3244 < 0.6
- m09 (nitrobenzene, chain_length=2): QED=0.5826 < 0.6
- m10 (nitrobenzene, chain_length=11): QED=0.3724 < 0.6
- m02 (nitrobenzene, chain_length=3): QED=0.6046 ≥ 0.6 ✓

m02 satisfies all constraints:
1. QED=0.6046 ≥ 0.6 ✓
2. psa=72.24 ≥ 40 ✓
3. Nitrobenzene series, not methoxybenzene ✓

Lead compound: m02

Budget: All 12 measurements used.

## Final m05 QED Prediction

Data for methoxybenzene series:
- n=2 (m15): QED=0.7621
- n=3 (m24): QED=0.7919 (peak)
- n=4 (m28): QED=0.7518
- n=5 (m17): QED=0.7498
- n=6 (m22): QED=0.7377

Pattern: QED peaks at n=3, then decreases.

For extrapolation to n=11 (m05):
- Using linear fit from n=3 to n=6: slope = (0.7377-0.7919)/(6-3) = -0.01807
- QED at n=11 = 0.7919 + (-0.01807)*(11-3) = 0.7919 - 0.1446 = 0.6473 ≈ 0.647

Rationale: The trend shows clear decrease after n=3. Linear extrapolation is reasonable given limited data. Longer chains likely continue decreasing due to increased rotatable bonds and lipophilicity.

Alternative calculation using average decrease per carbon from n=4 onward:
- Average decrease per carbon: [(-0.0020) + (-0.0121)]/2 = -0.00705
- From n=6 to n=11: 5 carbons × -0.00705 = -0.03525
- QED = 0.7377 - 0.03525 = 0.70245

But this assumes constant small decrease, which may underestimate penalty for very long chains. The linear fit from peak (n=3) accounts for overall downward trend.

Given QED penalizes long flexible chains, prediction of 0.647 is reasonable.

## Final Answer
- Target QED (m05): 0.647
- Lead compound: m02 (QED=0.6046)
- Congeneric series for m05: m15, m24, m28, m17, m22