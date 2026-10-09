# Notes: QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Identifying the m05 congeneric series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - a methoxy-phenyl amide with a 13-carbon alkyl chain.

The congeners in the pool with the same scaffold (methoxy-phenyl amide, `COc1ccc(cc1)C(=O)N-`) but different chain lengths are:
- m15: `COc1ccc(cc1)C(=O)NCC` (ethyl, 2 carbons)
- m24: `COc1ccc(cc1)C(=O)NCCC` (propyl, 3 carbons)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (butyl, 4 carbons)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (pentyl, 5 carbons)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (hexyl, 6 carbons)

### Measurements taken for the series

| Mol ID | Chain Length | QED |
|--------|-------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend analysis

The QED shows a peak at chain length 3 (m24, QED=0.7919), then declines as chain length increases:
- From n=3 to n=4: -0.0401
- From n=4 to n=5: -0.0020
- From n=5 to n=6: -0.0121

Overall decline from n=3 to n=6: 0.0542 over 3 carbons ≈ 0.018 per carbon.

### Extrapolation to m05 (chain length 13)

m05 has a 13-carbon chain, which is 7 carbons longer than the longest measured congener (m22, n=6).

Using linear extrapolation from the observed trend:
- Starting from n=6 (QED=0.7377), declining at ~0.018 per carbon
- Estimated QED at n=13: 0.7377 - 0.018 × 7 ≈ 0.61

However, the relationship is likely non-linear. As chain length increases:
- Molecular weight increases (m05: 305.46)
- logP increases (m05: alogP=4.956)

Both high MW and high logP are penalized in QED. The optimal ranges are MW 200-300 and logP 1-3. m05's alogP of 4.956 is well outside the optimal range, which would significantly reduce QED.

Considering the non-linear nature of QED and the substantial increase in lipophilicity, I estimate m05's QED to be approximately **0.58**.

## Job 2: Select a backup lead compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40 (from pool.tsv)
3. not in the same series as m05

### Filtering by psa ≥ 40

The m05 series all have psa=38.33, so they are automatically excluded.

Candidates with psa ≥ 40:
- Nitro-phenyl series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar-derived series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

### Measurements taken for lead candidates

| Mol ID | Series | psa | QED | Passes? |
|--------|--------|-----|-----|---------|
| m01 | sugar | 110.02 | 0.3187 | No (QED < 0.6) |
| m02 | nitro | 72.24 | 0.6046 | **Yes** |
| m03 | sugar | 110.02 | 0.3282 | No (QED < 0.6) |
| m09 | nitro | 72.24 | 0.5826 | No (QED < 0.6) |
| m12 | nitro | 72.24 | 0.4709 | No (QED < 0.6) |
| m14 | sugar | 110.02 | 0.3237 | No (QED < 0.6) |
| m27 | sugar | 110.02 | 0.333 | No (QED < 0.6) |

### Selected lead

**m02** is the only molecule I measured that satisfies all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05 series (nitro-phenyl scaffold) ✓

m02 has SMILES `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` - a nitro-phenyl amide with a propyl chain.

## Budget usage

Total measurements: 12 (all used)
- m05 series: 5 measurements (m15, m24, m28, m17, m22)
- Lead candidates: 7 measurements (m01, m02, m03, m09, m12, m14, m27)
