# Notes on QED Prediction and Lead Selection

## Job 1: Predicting QED for m05

### Congeneric Series Identification
Target molecule m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxybenzamide (`COc1ccc(cc1)C(=O)N`)
- Chain length: 12 carbons (`CCCCCCCCCCCC`)

Congeners in the pool (same scaffold, varying chain length):
1. m15: `COc1ccc(cc1)C(=O)NCC` - 2 carbons
2. m24: `COc1ccc(cc1)C(=O)NCCC` - 3 carbons  
3. m28: `COc1ccc(cc1)C(=O)NCCCC` - 4 carbons
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` - 5 carbons
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` - 6 carbons
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - 12 carbons (target)

Note: The pool only contains short-chain members (2-6 carbons), so we need to extrapolate beyond the measured range to predict m05 with 12 carbons.

### Measurement Plan
Will measure QED for all available congeners (m15, m24, m28, m17, m22) to establish the trend of QED vs. chain length.

### QED Measurements for Congeners
1. m15 (2-carbon chain): QED = 0.7621
2. m24 (3-carbon chain): QED = 0.7919
3. m28 (4-carbon chain): QED = 0.7518
4. m17 (5-carbon chain): QED = 0.7498
5. m22 (6-carbon chain): QED = 0.7377

### Trend Analysis
Chain length vs QED:
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518
- 5C: 0.7498
- 6C: 0.7377

The trend shows an initial increase from 2C to 3C, followed by a gradual decrease as chain length increases further. The peak QED occurs at 3 carbons.

For prediction of m05 (12C), we need to extrapolate beyond 6C. The decreasing trend suggests QED will continue to decrease with longer chains.

## Job 2: Selecting a Backup Lead Compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40 (free descriptor)
3. not in the same series as m05 (methoxybenzamide)

### Candidate Screening by PSA
- Methoxybenzamide series: psa = 38.33 (all < 40, also excluded by constraint 3)
- Nitrobenzamide series: psa = 72.24 (all ≥ 40)
- Polyol series: psa = 110.02 (all ≥ 40)
- Biphenyl series: psa = 29.1 (all < 40)

So viable candidates are from nitrobenzamide or polyol series only.

### Available Measurements Remaining
Budget used: 14 of 17, so 3 measurements remaining.

### Strategy
Need to find at least one molecule with QED ≥ 0.6. Given limited measurements, should measure molecules that seem promising based on:
1. Chain length trends observed in methoxybenzamide series (peak at 3C)
2. But each scaffold has its own curve, so cannot assume same trend.

Will measure a few candidates from both series.

### QED Measurements for Lead Candidates
1. m09 (nitrobenzamide, 2C): QED = 0.5826 (< 0.6)
2. m14 (polyol, 2C): QED = 0.3237 (< 0.6)
3. m02 (nitrobenzamide, 3C): QED = 0.6046 (≥ 0.6) ✓

### Selected Lead Compound
m02 satisfies all constraints:
- Measured QED = 0.6046 ≥ 0.6
- psa = 72.24 ≥ 40 (from free descriptors)
- Not in same series as m05 (nitrobenzamide vs methoxybenzamide)

## Final Prediction for m05 QED

### Trend Analysis Methods Considered
1. Linear extrapolation from last 3 points (4C, 5C, 6C): slope = -0.00705 per carbon
   Prediction: 0.7377 - (6 × 0.00705) = 0.6954

2. Linear extrapolation from all points after peak (3C, 4C, 5C, 6C): average slope = -0.01807 per carbon
   Prediction: 0.7377 - (6 × 0.01807) = 0.6293

3. Considering the significant increase in lipophilicity with 12-carbon chain (alogp = 4.956 vs 2-3 for shorter chains), QED typically penalizes high lipophilicity.

### Final Prediction
Using method 2 which accounts for the steeper initial drop and seems more appropriate for extrapolation far beyond measured range: **0.629**

Rationale: The 12-carbon chain is very lipophilic (alogp = 4.956), and QED typically decreases with excessive lipophilicity. The trend shows clear decrease after 3C, and with double the chain length of the longest measured congener (6C), a significant drop to ~0.629 is reasonable.