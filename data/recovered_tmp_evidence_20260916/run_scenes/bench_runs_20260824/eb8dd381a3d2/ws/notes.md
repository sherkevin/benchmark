# Medicinal Chemistry Task Notes

## Overview
Task requires:
1. Predict QED for m05 (target molecule) using trend analysis of its congeneric series
2. Find a molecule from the pool that satisfies all three constraints:
   - QED ≥ 0.6
   - PSA ≥ 40
   - Not in the same congeneric series as m05

Budget: 12 QED measurements total for both tasks.

## Step 1: Understanding the Data
Looking at pool.tsv, I can see:
- 33 molecules with SMILES structures and descriptors (mw, alogp, psa, rotb, arom)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12-carbon N-alkyl chain)
- Need to find congeneric series of m05 (same scaffold, varying N-alkyl chain length)

## Step 2: Group by Scaffold
Used group_by_scaffold() - found 4 scaffold groups:
1. COc1ccc(cc1)C(=O)N - m05's group (6 members)
   - m15 (chain_len: 2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N (9 members)
3. OCC(O)C(O)C(O)C(=O)N (9 members)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N (9 members)

m05 is in scaffold group 1 with chain length 11.

## Step 3: Task 1 - Predicting m05 QED
Measured 5 molecules from m05's series:
1. m15 (chain_len=2): QED=0.7621
2. m24 (chain_len=3): QED=0.7919
3. m28 (chain_len=4): QED=0.7518
4. m17 (chain_len=5): QED=0.7498
5. m22 (chain_len=6): QED=0.7377

Trend analysis:
- Quadratic fit (degree=2): predicts QED=0.5416 for chain_len=11
- Linear fit (degree=1): predicts QED=0.695 for chain_len=11

Given the curvilinear trend (peak at chain_len=3 then decline), quadratic fit seems more appropriate. Predicted QED for m05: 0.5416

Budget used: 5 measurements so far, 7 remaining.

## Step 4: Task 2 - Finding Suitable Molecule
Need molecule with: QED ≥ 0.6, PSA ≥ 40, not in m05's series.

PSA analysis:
- Scaffold 1 (m05's series): PSA=38.33 (< 40)
- Scaffold 2: PSA=72.24 (≥ 40)
- Scaffold 3: PSA=110.02 (≥ 40)
- Scaffold 4: PSA=29.1 (< 40)

Candidates from Scaffold 2 or 3.

Measured Scaffold 3 molecules (polyol series):
- m14: QED=0.3237 (too low)
- m27: QED=0.3330 (too low)
- m01: QED=0.3187 (too low)

Scaffold 3 seems to have consistently low QED.

Measured Scaffold 2 molecules (nitrobenzene series):
- m09: QED=0.5826 (< 0.6)
- m02: QED=0.6046 (≥ 0.6) ✓

**Found suitable molecule: m02**
- QED: 0.6046 (measured)
- PSA: 72.24 (from data)
- Series: Scaffold 2 (not m05's Scaffold 1)

Total measurements used: 10 (2 remaining).

## Step 5: Final Results
**Task 1**: Predicted QED for m05 = 0.5416 (using quadratic fit on 5 measured congeners)

**Task 2**: Selected molecule m02 with:
- Measured QED: 0.6046 (≥ 0.6)
- PSA: 72.24 (≥ 40)
- Series: Scaffold 2 (different from m05's Scaffold 1)

All constraints satisfied.

**Final answer.json filled with correct values.**