# QED Prediction and Lead Selection Analysis

## Initial Analysis of Molecular Series

Based on SMILES patterns in `data/pool.tsv`, I've identified 4 distinct scaffolds:

### Series 1: Methoxybenzamides (same as m05)
Scaffold pattern: `COc1ccc(cc1)C(=O)NC`
Molecules in this series:
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (target, C13 chain)
- m15: `COc1ccc(cc1)C(=O)NCC` (C2 chain)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain)
- m24: `COc1ccc(cc1)C(=O)NCCC` (C3 chain)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain)

### Series 2: Nitrobenzamides
Scaffold pattern: `O=[N+]([O-])c1ccc(cc1)C(=O)NC`
Molecules: m02, m06, m09, m10, m12, m29, m31, m32, m33

### Series 3: Diphenylbenzamides
Scaffold pattern: `c1ccc(cc1)-c1ccc(cc1)C(=O)NC`
Molecules: m04, m08, m11, m13, m16, m20, m23, m26, m30

### Series 4: Polyol benzamides
Scaffold pattern: `OCC(O)C(O)C(O)C(=O)NC`
Molecules: m01, m03, m07, m14, m18, m19, m21, m25, m27

## Job 1: Predict QED of m05

m05 has C13 chain length. The pool contains shorter chain versions of the same scaffold:
- m15: C2 chain
- m24: C3 chain  
- m28: C4 chain
- m17: C5 chain
- m22: C6 chain

### Measurements of Methoxybenzamide Series (Job 1):
1. m15 (C2 chain): QED = 0.7621
2. m24 (C3 chain): QED = 0.7919
3. m28 (C4 chain): QED = 0.7518
4. m17 (C5 chain): QED = 0.7498
5. m22 (C6 chain): QED = 0.7377

Chain lengths: [2, 3, 4, 5, 6]
QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

The trend shows a peak at C3 (0.7919) followed by gradual decline as chain length increases.

### Trend Analysis:
Data points: (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)

The pattern suggests QED peaks at moderate chain length (C3) then decreases as chain gets longer, possibly due to increasing lipophilicity beyond optimal range.

Looking at alogp values from pool.tsv:
- m15 (C2): alogp=1.445, QED=0.7621
- m24 (C3): alogp=1.835, QED=0.7919
- m28 (C4): alogp=2.225, QED=0.7518
- m17 (C5): alogp=2.615, QED=0.7498
- m22 (C6): alogp=3.005, QED=0.7377
- m05 (C13): alogp=4.956 (extrapolating)

The decrease from C6 (alogp=3.005, QED=0.7377) to C13 (alogp=4.956) suggests further QED decline.

## Job 2: Lead Selection

Candidate m02 measured: QED=0.6046 (≥0.6), psa=72.24 (≥40), not in methoxybenzamide series ✓

### All measurements used (12/12):
1. m15 (methoxybenzamide): 0.7621
2. m24 (methoxybenzamide): 0.7919
3. m28 (methoxybenzamide): 0.7518
4. m17 (methoxybenzamide): 0.7498
5. m22 (methoxybenzamide): 0.7377
6. m02 (nitrobenzamide): 0.6046
7. m01 (polyol benzamide): 0.3187
8. m03 (polyol benzamide): 0.3282
9. m06 (nitrobenzamide): 0.4011
10. m09 (nitrobenzamide): 0.5826
11. m10 (nitrobenzamide): 0.3724
12. m12 (nitrobenzamide): 0.4709

m02 is the only molecule found with QED ≥ 0.6 that also satisfies psa ≥ 40 and is not in methoxybenzamide series.

## Final Prediction for m05 QED

### Analysis of Trend:
Data points for methoxybenzamide series:
- C2 (m15): 0.7621
- C3 (m24): 0.7919 (peak)
- C4 (m28): 0.7518
- C5 (m17): 0.7498
- C6 (m22): 0.7377

The QED declines after C3. The decline rate appears to slow:
- C3→C4: -0.0401
- C4→C5: -0.0020
- C5→C6: -0.0121

Average decline per carbon after C3: ~0.0181

### Extrapolation to C13:
From C6 to C13: 7 additional carbons.
If decline continues at ~0.0181 per carbon: 0.7377 - (7 × 0.0181) = 0.7377 - 0.1267 = 0.6110

However, looking at nitrobenzamide series data:
- C2 (m09): 0.5826
- C4 (m12): 0.4709
- C10 (m10): 0.3724

The nitrobenzamide series shows more rapid decline with chain length. This suggests QED decline may accelerate with very long chains due to excessive lipophilicity.

m05 has alogp=4.956 (much higher than m22's 3.005). Given the significant increase in lipophilicity and the trend in nitrobenzamides, I predict m05's QED will be lower than linear extrapolation suggests.

### Prediction:
Considering:
1. Current trend shows slowing decline rate
2. But very high lipophilicity (alogp=4.956 vs 3.005 at C6)
3. Comparison with nitrobenzamide series suggests longer chains give lower QED

I predict m05 (C13) QED ≈ **0.58** (range 0.55-0.60).

This accounts for both the slowing decline in measured data and the penalizing effect of very high lipophilicity on QED.

## Final Answer Summary

### Job 1: Predicted QED for m05
**0.58**

### Job 2: Selected Lead Compound
**m02** with measured QED = 0.6046

### Congeneric Series (same as m05)
All methoxybenzamides in pool: m05, m15, m17, m22, m24, m28

All requirements satisfied:
1. m02 QED ≥ 0.6 ✓ (0.6046)
2. m02 psa ≥ 40 ✓ (72.24)
3. m02 not in same series as m05 ✓ (nitrobenzamide vs methoxybenzamide)