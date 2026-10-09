# Notes: QED Prediction for m05 and Lead Selection

## Task Overview
- Predict QED for m05 (unmeasurable, chain length 13)
- Select a lead compound with: QED ≥ 0.6, psa ≥ 40, not in m05's series

## Step 1: Identify m05's Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-phenyl amide (`COc1ccc(cc1)C(=O)N`)
- Chain length: 13 carbons

Same series molecules in pool (same scaffold, different chain lengths):
| mol_id | SMILES | chain length |
|--------|--------|--------------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 |

## Step 2: Measure Congeneric Series (5 measurements)

| mol_id | chain | QED |
|--------|-------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

**Trend observed:** QED peaks at chain length 3 (0.7919), then declines steadily.

## Step 3: Extrapolate to m05 (chain 13)

Linear regression on declining portion (chains 3-6):
- Fit: QED = 0.8319 - 0.01646 × chain_length
- For chain 13: QED = 0.8319 - 0.01646 × 13 = **0.618**

Alternative: linear regression on all 5 points gives QED ≈ 0.677

Given the instruction to not assume linearity, and considering that very long chains may cause accelerating decline in QED (due to high logP), I predict:

**m05 QED prediction: 0.62**

## Step 4: Lead Compound Selection

**Constraints:**
1. QED ≥ 0.6 (must measure)
2. psa ≥ 40 (free from table)
3. Not in m05's series (methoxy-phenyl)

**Candidates with psa ≥ 40, excluding m05 series:**
- Series D (polyol): m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)
- Series B (nitro-phenyl): m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)

**Measurements (7 remaining after congeneric series):**
| mol_id | QED | meets QED≥0.6? |
|--------|-----|----------------|
| m01 | 0.3187 | ✗ |
| m02 | 0.6046 | ✓ |
| m09 | 0.5826 | ✗ |
| m14 | 0.3237 | ✗ |
| m32 | 0.4683 | ✗ |
| m12 | 0.4709 | ✗ |
| m33 | 0.4591 | ✗ |

**Result:** Only m02 satisfies QED ≥ 0.6

**Verification for m02:**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Series: nitro-phenyl (not methoxy-phenyl) ✓

**Selected lead: m02**

## Budget Summary
- 5 measurements for congeneric series (m15, m17, m22, m24, m28)
- 7 measurements for lead candidates (m01, m02, m09, m12, m14, m32, m33)
- Total: 12/12 measurements used
