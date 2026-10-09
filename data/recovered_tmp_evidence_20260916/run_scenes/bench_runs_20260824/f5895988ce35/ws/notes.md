# Notes: QED Prediction for m05 and Lead Selection

## Task Overview
1. Predict QED for m05 (never synthesized, probe_unavailable)
2. Select a backup lead compound from the pool with: measured QED ≥ 0.6, psa ≥ 40, not in m05's series

## Identifying m05's Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: para-methoxy-benzamide (`COc1ccc(cc1)C(=O)N-`)
- Alkyl chain: 13 carbons (NCCCCCCCCCCC)

Pool molecules in the same series (same scaffold, different chain lengths):
- m15: NCC (2 carbons)
- m24: NCCC (3 carbons)
- m28: NCCCC (4 carbons)
- m17: NCCCCC (5 carbons)
- m22: NCCCCCC (6 carbons)

## Measurements for Congeneric Series (Job 1)

Measured all 5 congeners to establish the trend:

| mol_id | chain length | QED     |
|--------|--------------|---------|
| m15    | 2            | 0.7621  |
| m24    | 3            | 0.7919  |
| m28    | 4            | 0.7518  |
| m17    | 5            | 0.7498  |
| m22    | 6            | 0.7377  |

**Trend observed:**
- QED peaks at chain length 3 (0.7919)
- After the peak, QED declines as chain length increases
- Decline from C3 to C6: 0.0542 over 3 carbons (avg ~0.018 per carbon)
- The decline is not linear: C3→C4 (-0.0401), C4→C5 (-0.0020), C5→C6 (-0.0121)

**Extrapolation to m05 (chain length 13):**
- m05 has a 13-carbon chain, far beyond the measured range (C2-C6)
- Using exponential decay model from peak: QED ≈ 0.7919 * exp(-0.0237 * 10) ≈ 0.625
- Using linear extrapolation from C6: QED ≈ 0.7377 - 0.018*7 ≈ 0.611
- Considering non-linear decline and increased lipophilicity for very long chains

**Prediction:** QED for m05 ≈ 0.60

## Lead Compound Selection (Job 2)

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in m05's series (not para-methoxy-benzamide)

Molecules with psa ≥ 40 (from pool.tsv):
- Series B (nitro-benzamide): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Series D (polyol-amide): m01, m03, m07, m14, m18, m19, m21, m25, m27

Measurements for potential leads:

| mol_id | series | psa    | QED     | meets all constraints? |
|--------|--------|--------|---------|------------------------|
| m01    | D      | 110.02 | 0.3187  | No (QED < 0.6)         |
| m02    | B      | 72.24  | 0.6046  | **YES**                |
| m09    | B      | 72.24  | 0.5826  | No (QED < 0.6)         |
| m14    | D      | 110.02 | 0.3237  | No (QED < 0.6)         |
| m12    | B      | 72.24  | 0.4709  | No (QED < 0.6)         |
| m32    | B      | 72.24  | 0.4683  | No (QED < 0.6)         |
| m27    | D      | 110.02 | 0.3330  | No (QED < 0.6)         |
| m33    | B      | 72.24  | (not measured - budget exhausted) |

**Selected lead: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Series B (nitro-benzamide), not m05's series ✓

## Budget Usage
- 5 measurements for congeneric series (m15, m24, m28, m17, m22)
- 7 measurements for lead candidates (m01, m02, m09, m14, m12, m32, m27)
- Total: 12 measurements (budget exhausted)
