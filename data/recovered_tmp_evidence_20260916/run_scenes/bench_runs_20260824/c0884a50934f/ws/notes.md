# Notes for QED Prediction and Lead Selection

## Step 1: Understanding the Target Molecule m05

SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

This appears to be a benzamide derivative with:
- Methoxybenzene scaffold (`COc1ccc(cc1)`)
- Amide linkage (`C(=O)N`)
- Long alkyl chain (`CCCCCCCCCCC` - 11 carbons)

The target molecule m05 has never been synthesized, so we need to:
1. Identify its congeneric series (same scaffold, only N-alkyl chain length differs)
2. Measure QED values for other molecules in this series from the pool
3. Extrapolate to predict m05's QED (chain length 11)

## Step 2: Exploring the Pool

Let's start by examining the pool to understand what molecules are available.

**Page 1 observations:**
- m01: `OCC(O)C(O)C(O)C(=O)NCCCC` - sugar-like scaffold with 4-carbon chain
- m02: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` - nitrobenzene scaffold with 3-carbon chain
- m03: `OCC(O)C(O)C(O)C(=O)NCCCCCC` - similar to m01 but 6-carbon chain
- m04: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCC` - biphenyl scaffold with 4-carbon chain

Target m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxybenzene (anisole) with amide linkage
- Chain length: 11 carbons (`CCCCCCCCCCC`)

**Molecules in same series as m05 (methoxybenzene-amide scaffold):**
1. m15: `COc1ccc(cc1)C(=O)NCC` - 2-carbon chain
2. m24: `COc1ccc(cc1)C(=O)NCCC` - 3-carbon chain  
3. m28: `COc1ccc(cc1)C(=O)NCCCC` - 4-carbon chain
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` - 5-carbon chain
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` - 6-carbon chain
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - 11-carbon chain (target)

These all share the exact scaffold `COc1ccc(cc1)C(=O)N` with varying alkyl chain lengths.

## Step 3: Measuring QED for Congeneric Series

**QED Measurements:**
- m15 (2C): 0.7621
- m24 (3C): 0.7919
- m28 (4C): 0.7518
- m17 (5C): 0.7498
- m22 (6C): 0.7377

**Chain lengths vs QED:**
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518
- 5C: 0.7498  
- 6C: 0.7377

The QED seems to peak at 3 carbons and then gradually decreases as chain length increases. We need to extrapolate to 11 carbons (m05).

## Step 4: Extrapolating QED for m05 (11 carbons)

Using `fit_trend` with degree 2 (quadratic) polynomial:
- Chain lengths (x): [2, 3, 4, 5, 6]
- QED values (y): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Target: x = 11 (11-carbon chain)

**Prediction:** QED = 0.5416

Quadratic fit equation: 0.74932 + 0.017024*x - 0.003264*x²
RMSE: 0.01188

Alternative linear fit (degree 1) gives 0.695, but quadratic has slightly lower RMSE (0.01188 vs 0.01308) and better captures the curvature observed in the data.

**Decision:** Using quadratic extrapolation, predicted QED for m05 = 0.5416

## Step 5: Selecting Backup Lead Compound

**Constraints:**
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same series as m05 (methoxybenzene-amide scaffold)

**Process:**
1. Used `screen_candidates()` to find molecules with psa ≥ 40: 18 candidates
2. Verified none are in m05's series (all have different scaffolds)
3. With 3 measurements remaining, tested:
   - m14 (sugar-like, 2C): QED = 0.3237 (< 0.6)
   - m02 (nitrobenzene, 3C): QED = 0.6046 (≥ 0.6) ✓

**Selected Lead:** m02
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Scaffold: nitrobenzene (different from m05's methoxybenzene)
- psa: 72.24 (≥ 40)
- Measured QED: 0.6046 (≥ 0.6)
- Satisfies all 3 constraints

## Summary

**Job 1 - Predicting m05 QED:**
- Identified congeneric series: methoxybenzene-amide scaffold (m15, m24, m28, m17, m22, m05)
- Measured QED for 5 synthesized congeners (2C to 6C chains)
- Used quadratic fit (degree 2) to extrapolate to 11C chain (m05)
- **Predicted QED: 0.5416**

**Job 2 - Selecting Backup Lead:**
- Screened for psa ≥ 40: 18 candidates
- All candidates were from different scaffolds (sugar-like or nitrobenzene)
- Measured m14 (0.3237) and m02 (0.6046)
- **Selected m02** with QED = 0.6046, psa = 72.24, different scaffold

**Budget Usage:**
- list_pool: 9 measurements (pages 1-9)
- measure_qed: 7 measurements (m15, m24, m28, m17, m22, m14, m02)
- **Total: 16/17 measurements used** (1 remaining)