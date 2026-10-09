# Notes on Medicinal Chemistry Task

## Task 1: Predict QED for m05

Target molecule m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

### Strategy:
1. Identify molecules in the same congeneric series as m05 (same scaffold, only N-alkyl chain length differs)
2. Measure QED for several molecules in this series
3. Analyze trend to predict m05's QED

### m05's congeneric series:
All molecules with scaffold `COc1ccc(cc1)C(=O)N` followed by alkyl chain:
- m15: `COc1ccc(cc1)C(=O)NCC` (C2 chain)
- m24: `COc1ccc(cc1)C(=O)NCCC` (C3 chain)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C11 chain)

### Measurement plan:
We have 12 total measurements available. Need to balance between Task 1 (predict m05 QED) and Task 2 (find lead molecule).

For Task 1, I should measure several molecules from m05's series to establish trend. Let's start with 5-6 measurements from this series.

### QED measurements for m05's series:
1. m15 (C2 chain): QED = 0.7621
2. m24 (C3 chain): QED = 0.7919
3. m28 (C4 chain): QED = 0.7518
4. m17 (C5 chain): QED = 0.7498
5. m22 (C6 chain): QED = 0.7377

Chain lengths (carbon atoms in alkyl chain):
- m15: 2 carbons
- m24: 3 carbons  
- m28: 4 carbons
- m17: 5 carbons (from SMILES `NCCCCC`)
- m22: 6 carbons
- m05: 11 carbons

QED values show a decreasing trend with increasing chain length after an initial peak at C3 (m24: 0.7919).

### Trend analysis for m05:
The QED decreases as chain length increases beyond C3. m05 has a very long chain (C11), so its QED should be lower than m22 (C6, 0.7377).

From the data:
- C3 (m24): 0.7919 (peak)
- C4 (m28): 0.7518 (-0.0401)
- C5 (m17): 0.7498 (-0.0020)
- C6 (m22): 0.7377 (-0.0121)

Average decrease per carbon beyond C3: ~0.0181 per carbon.
m05 is 8 carbons beyond C3 (C11 vs C3), so estimated decrease: ~0.1448
Estimated m05 QED: 0.7919 - 0.1448 = 0.6471

Given the nonlinear trend and m05's unfavorable properties (high mw=305.46, high alogp=4.956, many rotb=12), I estimate m05 QED ≈ 0.65.

### Task 2: Finding lead molecule
Requirements:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in m05's congeneric series

#### Candidates measured:
- m02: QED=0.6046, psa=72.24, not in m05 series ✓ QUALIFIES
- m09: QED=0.5826 (<0.6) ✗
- m01: QED=0.3187 (<0.6) ✗
- m14: QED=0.3237 (<0.6) ✗
- m12: QED=0.4709 (<0.6) ✗
- m29: QED=0.4446 (<0.6) ✗
- m32: QED=0.4683 (<0.6) ✗

m02 is the only molecule found that satisfies all three constraints.

### Budget usage:
Total budget: 12 measurements
Used: 12/12
- Task 1 series: 5 measurements (m15, m24, m28, m17, m22)
- Task 2 candidates: 7 measurements (m02, m09, m01, m14, m12, m29, m32)

## Final Answers:
1. Task 1: Estimated m05 QED = 0.65
   - Based on decreasing trend in congeneric series: 0.7919 (C3) → 0.7377 (C6)
   - m05 has C11 chain (5 carbons longer than C6)
   - Estimated additional decrease: ~0.09, giving ~0.65

2. Task 2: Selected m02 with QED = 0.6046
   - Satisfies all constraints:
     - Measured QED = 0.6046 ≥ 0.6 ✓
     - psa = 72.24 ≥ 40 ✓  
     - Not in m05's series (nitrobenzene scaffold vs methoxybenzene) ✓