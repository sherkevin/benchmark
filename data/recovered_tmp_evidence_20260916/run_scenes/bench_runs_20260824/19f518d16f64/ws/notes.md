# Notes

## Task Overview
- Task 1: Find QED of m05 (congeneric series prediction needed since direct measurement unavailable)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, NOT in same congeneric series as m05

## Budget
- 17 units total for list_pool and measure_qed calls
- Each page costs 1, each measurement costs 1

## Plan
1. List the pool to understand the molecules available
2. Identify m05's congeneric series (same scaffold, different N-alkyl chain length)
3. Measure QED for molecules in m05's series to predict m05's QED
4. Find candidate molecules meeting all three criteria
5. Measure QED for the chosen lead molecule

## Progress

### Step 1: Listed all 9 pages of the pool (33 molecules total)
- Budget used: 9 units

### Step 2: Identified m05's congeneric series
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (methoxy-benzamide with 11-carbon alkyl chain)

Congeneric series (same scaffold `COc1ccc(cc1)C(=O)N-`):
- m15 (NCC, 2C), m24 (NCCC, 3C), m28 (NCCCC, 4C), m17 (NCCCCC, 5C), m22 (NCCCCCC, 6C), m05 (11C)

### Step 3: Measured QED for m05's congeneric series
- m15: 0.7621
- m24: 0.7919
- m28: 0.7518
- m17: 0.7498
- m22: 0.7377

Trend: Peak at 3C (0.7919), then declining. Extrapolating to 11C with ~0.018/carbon decline rate:
QED(m05) ≈ 0.7919 - 8*0.018 ≈ 0.65, but considering high alogP penalty (4.956), estimate ~0.58

### Step 4: Found candidate for Task 2
Criteria: QED >= 0.6, psa >= 40, NOT in m05's series

Measured candidates:
- m32: QED = 0.4683 (fails QED >= 0.6)
- m12: QED = 0.4709 (fails QED >= 0.6)
- m02: QED = 0.6046 ✓

### Step 5: Verified m02 meets all criteria
1. QED = 0.6046 >= 0.6 ✓
2. psa = 72.24 >= 40 ✓
3. m02 SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` - nitro-benzamide scaffold, different from m05's methoxy-benzamide ✓

## Final Answers
- Task 1 (m05 QED prediction): 0.58
- Task 2 (lead molecule): m02 with measured QED = 0.6046

## Budget Summary
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 units used
