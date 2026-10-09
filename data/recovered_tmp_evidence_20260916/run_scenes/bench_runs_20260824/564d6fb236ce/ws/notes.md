# Notes: QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - a methoxy-phenyl amide with a 13-carbon alkyl chain.

The congeneric series shares the same scaffold `COc1ccc(cc1)C(=O)N-` and differs only in alkyl chain length. From the pool, I identified these congeners:

| mol_id | SMILES | Chain Length | QED (measured) |
|--------|--------|--------------|----------------|
| m15    | COc1ccc(cc1)C(=O)NCC | 2 (ethyl) | 0.7621 |
| m24    | COc1ccc(cc1)C(=O)NCCC | 3 (propyl) | 0.7919 |
| m28    | COc1ccc(cc1)C(=O)NCCCC | 4 (butyl) | 0.7518 |
| m17    | COc1ccc(cc1)C(=O)NCCCCC | 5 (pentyl) | 0.7498 |
| m22    | COc1ccc(cc1)C(=O)NCCCCCC | 6 (hexyl) | 0.7377 |
| m05    | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 13 (tridecyl) | ? (target) |

### Measurements Used

I measured all 5 congeners in the pool:
- m15: QED = 0.7621 (measurement #1)
- m17: QED = 0.7498 (measurement #2)
- m22: QED = 0.7377 (measurement #3)
- m24: QED = 0.7919 (measurement #4)
- m28: QED = 0.7518 (measurement #5)

### Trend Analysis

The QED trend vs chain length shows:
- Peak at chain 3 (m24): QED = 0.7919
- Decline from chain 3 to 6: 0.7919 → 0.7377 (drop of 0.0542 over 3 carbons)
- Average decline rate: ~0.018 QED per carbon

Chain-by-chain changes:
- 3→4: -0.0401
- 4→5: -0.0020
- 5→6: -0.0121

### Extrapolation to Chain 13

Using linear regression on the declining portion (chains 3-6):
- Slope ≈ -0.0165 QED per carbon
- Intercept ≈ 0.832

QED(13) ≈ 0.832 - 0.0165 × 13 ≈ 0.618

However, the relationship is not purely linear. As chain length increases:
- MW increases (m05 MW = 305.46, above optimal 200-300)
- ALogP increases (m05 ALogP = 4.956, well above optimal ~2-3)
- Rotatable bonds increase (m05 rotb = 12, high)

These factors impose additional penalties on QED beyond the simple chain-length trend.

Considering both the extrapolated trend (~0.62) and the property penalties for m05's extreme values, I estimate:

**Predicted QED for m05: 0.58**

## Job 2: Select Backup Lead Compound

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (from pool.tsv)
3. NOT in the same series as m05 (not methoxy-phenyl amide)

### Filtering by PSA ≥ 40

From pool.tsv, molecules with PSA ≥ 40:
- Sugar amide series (PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitro-phenyl series (PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

All of these are NOT in the m05 series (methoxy-phenyl), so they pass constraint 3.

### Measurements for Lead Selection

I measured candidates with favorable descriptors (moderate MW, alogp near optimal):
- m12: QED = 0.4709 (measurement #6) - FAILS constraint 1
- m32: QED = 0.4683 (measurement #7) - FAILS constraint 1
- m02: QED = 0.6046 (measurement #8) - PASSES all constraints!

### Selected Lead: m02

m02 properties:
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- MW: 208.22
- ALogP: 1.735
- PSA: 72.24 (≥ 40 ✓)
- Measured QED: 0.6046 (≥ 0.6 ✓)
- Series: nitro-phenyl amide (NOT m05 series ✓)

All three constraints satisfied.

### Budget Summary
- Total measurements: 8 out of 12
- Job 1 (congeners): 5 measurements (m15, m17, m22, m24, m28)
- Job 2 (lead selection): 3 measurements (m12, m32, m02)
