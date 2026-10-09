# Task Notes

## Initial Analysis

From TASK.md:
- Target molecule: m05 with SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Need to predict QED for m05 (extrapolation)
- Need to find molecules in same series as m05 (same scaffold)
- Budget: 12 measurements total for both tasks
- Need to pick a lead compound with: QED ≥ 0.6, psa ≥ 40, not in m05's series

## Step 1: Understand m05's scaffold

First, let's analyze m05's structure:
SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Contains: methoxy group (COc), aromatic ring, carbonyl group, N-alkyl chain
- N-alkyl chain length: need to count carbons in NCCCCCCCCCCC

Let's use describe_structure on m05 to understand its scaffold:

describe_structure results for m05:
- scaffold: `COc1ccc(cc1)C(=O)N`
- chain_len: 11

Now I need to find all molecules in pool with same scaffold. Looking at pool.tsv, all molecules with scaffold `COc1ccc(cc1)C(=O)N`:
- m15: COc1ccc(cc1)C(=O)NCC - chain_len: 2
- m24: COc1ccc(cc1)C(=O)NCCC - chain_len: 3
- m28: COc1ccc(cc1)C(=O)NCCCC - chain_len: 4
- m17: COc1ccc(cc1)C(=O)NCCCCC - chain_len: 5
- m22: COc1ccc(cc1)C(=O)NCCCCCC - chain_len: 6
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC - chain_len: 11 (target)

So we have 5 pool molecules in m05's series (m15, m24, m28, m17, m22) with chain lengths 2, 3, 4, 5, 6 respectively. m05 has chain length 11, so we need to extrapolate beyond the measured range.

## Measurements for m05 series:
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919  
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377

Observations: QED peaks at chain length 3 (0.7919) and then decreases as chain length increases.

Now I need to fit a trend to predict m05's QED at chain length 11. Since the relationship appears non-linear (peaking then decreasing), I should use polynomial degree 2 or higher.

## Trend fitting for m05 prediction:

Using all 5 data points (chain lengths 2-6):
1. Linear fit (degree 1): predicts QED = 0.695 at chain length 11
2. Quadratic fit (degree 2): predicts QED = 0.5416 at chain length 11
3. Cubic fit (degree 3): predicts QED = 2.1323 (invalid, >1, suggests overfitting)

The quadratic fit seems most reasonable given:
- QED peaks at chain length 3 then decreases
- Extrapolating with quadratic gives prediction within valid range (0-1)
- RMSE for quadratic (0.01188) is slightly better than linear (0.01308)
- Cubic gives unrealistic extrapolation (>1) despite better RMSE on training data

I'll use the quadratic prediction of 0.5416 for m05's QED.

## Job 2: Finding lead compound

### Screening candidates with psa ≥ 40
18 molecules pass the psa ≥ 40 constraint. They belong to two main series:

1. OCC(O) series (scaffold: OCC(O)C(O)C(O)C(=O)N): m01, m03, m07, m14, m18, m19, m21, m25, m27
2. Nitro series (scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements taken:
**OCC(O) series:**
- m01 (chain_len 4): QED = 0.3187
- m14 (chain_len 2): QED = 0.3237  
- m27 (chain_len 3): QED = 0.3330
- m19 (chain_len 5): QED = 0.3244

This series consistently shows low QED values (~0.32-0.33), so none will satisfy QED ≥ 0.6.

**Nitro series:**
- m02 (chain_len 3): QED = 0.6046 ✓ (≥ 0.6)
- m09 (chain_len 2): QED = 0.5826 (< 0.6)
- m12 (chain_len 4): QED = 0.4709 (< 0.6)

### Lead compound selection
m02 satisfies all three constraints:
1. measured QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24 from pool.tsv)
3. not in m05's series ✓ (different scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N vs m05's COc1ccc(cc1)C(=O)N)

I have used all 12 measurements. m02 appears to be the best candidate found.

## Summary

### Job 1: Predict m05's QED
- m05's scaffold: `COc1ccc(cc1)C(=O)N`, chain_len: 11
- Congeners in pool: m15 (chain_len 2), m24 (3), m28 (4), m17 (5), m22 (6)
- Measured QED values: m15=0.7621, m24=0.7919, m28=0.7518, m17=0.7498, m22=0.7377
- Quadratic fit (degree 2) predicts: QED = 0.5416 for m05 (chain_len 11)

### Job 2: Select lead compound  
- Candidate must satisfy: QED ≥ 0.6, psa ≥ 40, not in m05's series
- Screened 18 molecules with psa ≥ 40
- OCC(O) series has consistently low QED (~0.32-0.33)
- Nitro series: m02 has QED=0.6046 (≥ 0.6), psa=72.24 (≥ 40), different scaffold from m05
- Selected m02 as lead compound

### Final answer
- target_qed: 0.5416
- lead_mol_id: "m02"
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"]