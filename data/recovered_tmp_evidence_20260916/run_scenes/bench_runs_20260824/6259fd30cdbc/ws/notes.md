# Notes on QED Prediction and Lead Selection

## Task Summary
- **Job 1**: Predict QED of m05 (unsynthesized, 13-carbon chain)
- **Job 2**: Pick a lead compound with QED ≥ 0.6, psa ≥ 40, NOT in m05 series
- **Budget**: 12 measurements total for both jobs

## Step 1: Identify m05 congeners (same scaffold)

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: methoxy-benzamide `COc1ccc(cc1)C(=O)N-`

Congeners in pool (same scaffold, different chain lengths):
- m15: NCC (ethyl, 2 carbons)
- m24: NCCC (propyl, 3 carbons)
- m28: NCCCC (butyl, 4 carbons)
- m17: NCCCCC (pentyl, 5 carbons)
- m22: NCCCCCC (hexyl, 6 carbons)
- m05: NCCCCCCCCCCC (13 carbons) - TARGET (not in pool, unsynthesized)

## Step 2: Measure congeners to establish trend

Measurements (5 used):
| mol_id | chain length | QED |
|--------|-------------|-----|
| m15    | 2           | 0.7621 |
| m24    | 3           | 0.7919 |
| m28    | 4           | 0.7518 |
| m17    | 5           | 0.7498 |
| m22    | 6           | 0.7377 |

**Trend analysis:**
- QED peaks at chain length 3 (m24: 0.7919)
- Then steadily declines: 0.7518 → 0.7498 → 0.7377 for chains 4, 5, 6

**Extrapolation method:**
Linear regression on the declining portion (chains 3-6):
- Slope: -0.0165 QED units per carbon
- Intercept: 0.8319
- Model: QED ≈ 0.8319 - 0.0165 × chain_length

For chain_length = 13:
QED ≈ 0.8319 - 0.0165 × 13 = 0.8319 - 0.2145 = **0.6174**

Rounded prediction: **0.62**

## Step 3: Find lead compound (QED ≥ 0.6, psa ≥ 40, not in m05 series)

Candidates with psa ≥ 40 (excluding m05 series m15, m17, m22, m24, m28):
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

Measurements (7 more used, total 12):
| mol_id | psa | QED | Meets criteria? |
|--------|-----|-----|-----------------|
| m01    | 110.02 | 0.3187 | No (QED < 0.6) |
| m02    | 72.24  | 0.6046 | **YES** ✓ |
| m03    | 110.02 | 0.3282 | No (QED < 0.6) |
| m09    | 72.24  | 0.5826 | No (QED < 0.6) |
| m14    | 110.02 | 0.3237 | No (QED < 0.6) |
| m12    | 72.24  | 0.4709 | No (QED < 0.6) |
| m27    | 110.02 | 0.3330 | No (QED < 0.6) |

**Selected lead: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` - nitro-benzamide scaffold, NOT methoxy-benzamide ✓

## Measurements summary
Total: 12/12 budget used
- Congeners: m15, m24, m28, m17, m22 (5 measurements)
- Lead candidates: m01, m02, m03, m09, m14, m12, m27 (7 measurements)

## Final answers
- **target_qed**: 0.62 (predicted for m05 via linear extrapolation)
- **lead_mol_id**: m02
- **lead_qed_measured**: 0.6046
- **congener_ids**: ["m15", "m17", "m22", "m24", "m28"]
