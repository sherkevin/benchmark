# Notes for QED Prediction Task

## Initial Analysis of Data

Examining `data/pool.tsv` to identify molecules in the same series as `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`).

### Scaffold Identification
The scaffold for m05 appears to be: methoxybenzamide with an N-alkyl chain.
Pattern: `COc1ccc(cc1)C(=O)N` followed by C atoms (alkyl chain).

Looking for molecules with similar SMILES patterns:

1. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12 C's in chain)
2. m15: `COc1ccc(cc1)C(=O)NCC` (2 C's in chain)
3. m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 C's in chain)
4. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 C's in chain)
5. m24: `COc1ccc(cc1)C(=O)NCCC` (3 C's in chain)
6. m28: `COc1ccc(cc1)C(=O)NCCCC` (4 C's in chain)

These all appear to share the same scaffold: methoxybenzamide with varying N-alkyl chain lengths.

## Job 1: Predicting QED for m05

### Congener Series Identification
The molecules in the same series as m05 (same scaffold) appear to be:
- m15: C2 chain
- m24: C3 chain  
- m28: C4 chain
- m17: C5 chain
- m22: C6 chain
- m05: C12 chain (target, not in pool)

Note: m05 has C12 chain, so we need to measure the shorter chain members (m15, m24, m28, m17, m22) to extrapolate to C12.

### Measurement Results for Congeneric Series

I've measured all congeneric series members (same scaffold as m05):

1. m15 (C2 chain): QED = 0.7621
2. m24 (C3 chain): QED = 0.7919  
3. m28 (C4 chain): QED = 0.7518
4. m17 (C5 chain): QED = 0.7498
5. m22 (C6 chain): QED = 0.7377

Chain length (C atoms in N-alkyl chain) vs QED:
- C2: 0.7621
- C3: 0.7919
- C4: 0.7518
- C5: 0.7498
- C6: 0.7377

Target m05 has C12 chain (12 carbon atoms).

### Trend Analysis
The QED values show a pattern: initially increases from C2 to C3 (0.7621 → 0.7919), then decreases steadily from C3 to C6 (0.7919 → 0.7377). This suggests a non-linear relationship, possibly peaking around C3 chain length.

### Mathematical Analysis for Extrapolation
Data points (chain length, QED):
- (2, 0.7621)
- (3, 0.7919)
- (4, 0.7518)
- (5, 0.7498)
- (6, 0.7377)

Observing the trend:
1. From C3 to C6: QED decreases from 0.7919 to 0.7377 over 3 carbon atoms
2. Average decrease per carbon atom: (0.7919 - 0.7377) / 3 = 0.01807
3. From C6 to C12 (target): 6 more carbon atoms
4. If linear continuation: predicted decrease = 6 × 0.01807 = 0.1084
5. Predicted QED for C12: 0.7377 - 0.1084 = 0.6293

However, looking at the pattern from C4 to C6:
- C4→C5: 0.7518 → 0.7498 (Δ = -0.0020)
- C5→C6: 0.7498 → 0.7377 (Δ = -0.0121)

The rate of decrease appears to be accelerating. This suggests a non-linear decay pattern.

### Alternative Fitting Approaches
Given the limited data points and the need to extrapolate beyond C6, I should consider:
1. Linear fit using C3-C6 data (post-peak region)
2. Quadratic fit to capture curvature
3. Exponential decay model

Since we only have 3 points for the decreasing trend (C4, C5, C6), a linear extrapolation from C6 to C12 might be the most reasonable given the data limitations.

## Job 2: Finding Backup Lead Compound

### Candidate Identification
Requirements:
1. Measured QED ≥ 0.6
2. psa ≥ 40 (from pool.tsv)
3. Not in same series as m05 (methoxybenzamide scaffold)

From pool.tsv, molecules with psa ≥ 40:
- psa = 110.02: m01, m03, m07, m14, m18, m19, m21, m25, m27
- psa = 72.24: m02, m06, m09, m10, m12, m29, m31, m32, m33

All these have different scaffolds (polyol benzamide or nitrobenzamide), so none are in m05 series.

### Measurement Results for Lead Candidates

I measured several candidates with psa ≥ 40:

1. m01 (psa=110.02): QED = 0.3187 (too low)
2. m02 (psa=72.24): QED = 0.6046 (≥ 0.6, meets criteria!)
3. m03 (psa=110.02): QED = 0.3282 (too low)
4. m06 (psa=72.24): QED = 0.4011 (too low)
5. m07 (psa=110.02): QED = 0.3233 (too low)
6. m09 (psa=72.24): QED = 0.5826 (just below 0.6)
7. m10 (psa=72.24): QED = 0.3724 (too low)

Best candidate found: m02 with QED = 0.6046 (≥ 0.6), psa = 72.24 (≥ 40), and nitrobenzamide scaffold (not in m05 series).

### Final Prediction for m05 QED

After analyzing the trend in the congeneric series:
- Measured QED values: C2=0.7621, C3=0.7919, C4=0.7518, C5=0.7498, C6=0.7377
- Trend shows peak at C3, then decreasing values
- m05 has C12 chain (12 carbons), much longer than measured range (C2-C6)
- Using linear extrapolation from decreasing region (C3-C6): average decrease per carbon = (0.7919 - 0.7377)/3 = 0.01807
- From C6 to C12: 6 additional carbons × 0.01807 = 0.1084 decrease
- Predicted QED for m05 (C12): 0.7377 - 0.1084 = 0.6293

Considering the accelerating decrease pattern (C4→C5: -0.0020, C5→C6: -0.0121), a conservative estimate might be lower, but 0.6293 seems reasonable based on available data.

### Final Conclusions

1. Predicted QED for m05: ~0.629
2. Lead compound: m02 (QED=0.6046, psa=72.24, not in m05 series)
3. Congeneric series members: m15, m24, m28, m17, m22