# Notes for QED Prediction Task

## Job 1: Predict QED for m05

### Step 1: Identify congeneric series with m05
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11-carbon alkyl chain)

Scaffold: methoxy-phenyl-amide (`COc1ccc(cc1)C(=O)N`)

Congeners in pool (same scaffold, different chain lengths):
- m15: `COc1ccc(cc1)C(=O)NCC` (2 carbons) - QED: 0.7621
- m24: `COc1ccc(cc1)C(=O)NCCC` (3 carbons) - QED: 0.7919
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4 carbons) - QED: 0.7518
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons) - QED: 0.7498
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons) - QED: 0.7377

### Step 2: Trend analysis
Chain length vs QED:
- 2 carbons: 0.7621
- 3 carbons: 0.7919 (peak)
- 4 carbons: 0.7518
- 5 carbons: 0.7498
- 6 carbons: 0.7377

Pattern: QED peaks at 3 carbons, then declines as chain lengthens.

### Step 3: Extrapolation to 11 carbons (m05)
Linear fit on declining portion (x≥3):
Slope = (0.7377 - 0.7919) / (6 - 3) = -0.0181 per carbon

At x=11: 0.7919 + (-0.0181)(8) = 0.7919 - 0.1448 = 0.6471

However, the task warns against assuming linear relationship. The drop pattern:
- 3→4: -0.0401
- 4→5: -0.0020  
- 5→6: -0.0121

The relationship is not perfectly linear or quadratic. Given:
- Extrapolation is 5 carbons beyond measured range (significant distance)
- Long hydrophobic chains typically reduce drug-likeness
- The trend shows consistent decline after the peak

I predict **m05 QED = 0.58** (conservative estimate accounting for extrapolation uncertainty)

## Job 2: Backup Lead Selection

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (from pool.tsv)
3. NOT in same series as m05 (different scaffold)

### Measurements performed:
- m01: QED=0.3187, psa=110.02 - FAIL (QED < 0.6)
- m02: QED=0.6046, psa=72.24 - PASS ALL CONSTRAINTS ✓
- m09: QED=0.5826, psa=72.24 - FAIL (QED < 0.6)
- m14: QED=0.3237, psa=110.02 - FAIL (QED < 0.6)

### Selected lead: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED measured: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold: nitro-phenyl-amide (different from m05's methoxy-phenyl-amide ✓)

## Summary
- Congeners of m05 in pool: m15, m17, m22, m24, m28
- Predicted QED for m05: 0.58
- Lead compound: m02 with measured QED 0.6046
