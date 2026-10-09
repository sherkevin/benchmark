# Notes for QED Prediction Task

## Initial Analysis

### Task Overview
1. Task 1: Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
2. Task 2: Find molecule with QED ≥ 0.6, PSA ≥ 40, and not in same congeneric series as m05
3. Budget: 12 QED measurements total for both tasks

### Data Examination
- Total of 33 molecules in pool.tsv
- m05 belongs to methoxybenzamide series: `COc1ccc(cc1)C(=O)N-[alkyl chain]`
- Other molecules in same series: m15, m17, m22, m24, m28
- Chain lengths vary: m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6), m05 (C13)

### Strategy
1. First measure QED for some members of m05's congeneric series to establish trend
2. Use trend to predict m05's QED
3. Use remaining measurements to find suitable molecule for Task 2
4. Must check PSA ≥ 40 and exclude molecules in m05's series

## Measurements Plan
Will start with measurements of m05 series molecules to understand QED trend vs alkyl chain length.

### Measurements Taken (5/12 used)
1. m15 (C2 chain): QED = 0.7621
2. m22 (C6 chain): QED = 0.7377  
3. m17 (C5 chain): QED = 0.7498
4. m28 (C4 chain): QED = 0.7518
5. m24 (C3 chain): QED = 0.7919

### Analysis of m05 Series Trend
Chain lengths and QED values:
- m24 (C3): 0.7919
- m15 (C2): Анна: 0.7621
- m28 (C4): 0.7518
- m17 (C5): 0.7498
- m22 (C6): 0.7377

m05 has C13 chain (much longer). The trend shows decreasing QED with increasing chain length beyond C3. m05's QED is likely lower than 0.7377.

### Task 2 Candidate Search
Need molecule with: QED ≥ 0.6, PSA ≥ 40, not in m05 series.

PSA analysis:
- Methoxybenzamide series (m05 series): PSA = 38.33 (< 40) → disqualifies all in this series
- Biphenyl series: PSA = 29.1 (< 40) → disqualifies all in this series
- Nitrophenyl series: PSA = 72.24 (≥ 40) → potential candidates
- Polyol series: PSA = 110.02 (≥ 40) → potential candidates

### All Measurements (12/12 used)
1. m15 (C2 chain): QED = 0.7621 (m05 series)
2. m22 (C6 chain): QED = 0.7377 (m05 series)
3. m17 (C5 chain): QED = 0.7498 (m05 series)
4. m28 (C4 chain): QED = 0.7518 (m05 series)
5. m24 (C3 chain): QED = 0.7919 (m05 series)
6. m02 (nitrophenyl): QED = 0.6046 (PSA=72.24) → Candidate!
7. m01 (polyol): QED = 0.3187 (PSA=110.02)
8. m03 (polyol): QED = 0.3282 (PSA=110.02)
9. m09 (nitrophenyl): QED = 0.5826 (PSA=72.24) < 0.6
10. m12 (nitrophenyl): QED = 0.4709 (PSA=72.24) < 0.6
11. m32 (nitrophenyl): QED = 0.4683 (PSA=72.24) < 0.6
12. m07 (polyol): QED = 0.3233 (PSA=110.02)

### Best Candidate for Task 2: m02
- QED = 0.6046 (≥ 0.6)
- PSA = 72.24 (≥ 40)
- Not in m05's congeneric series (nitrophenyl vs methoxybenzamide)

### m05 QED Prediction Rationale
Based on measured QED values in m05's congeneric series:
- Chain length vs QED: C3(0.7919) → C4(0.7518) → C5(0.7498) → C6(0.7377)
- Trend shows decreasing QED with increasing alkyl chain length
- Average decrease per carbon from C4-C6: ~0.00705
- m05 has C13 chain (7 carbons longer than C6)
- Linear extrapolation: 0.7377 - (7 × 0.00705) = 0.688
- However, decrease may accelerate with longer chains (C5-C6 decrease was 0.0121)
- Using C5-C6 rate: 0.7377 - (7 × 0.0121) = 0.653
- Considering C13 chain is very long/hydrophobic (reduces drug-likeness)
- Final prediction: 0.625 (mid-range of estimates)

### Final Answer
- target_qed: 0.625 (predicted QED for m05)
- lead_mol_id: "m02" (best candidate meeting all Task 2 criteria)
- lead_qed_measured: 0.6046 (measured QED for m02)