# Notes: QED Prediction and Lead Selection

## Task Overview
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC, C11 chain) which cannot be measured
- Select a backup lead compound with QED ≥ 0.6, psa ≥ 40, not in m05's series

## Step 1: Identify Congeneric Series

Parsed all 33 molecules to identify scaffolds. m05 has scaffold `COc1ccc(cc1)C(=O)N-` (methoxy-phenyl amide).

**Congeners of m05 in the pool** (same scaffold, different chain lengths):
| mol_id | SMILES | chain length |
|--------|--------|-------------|
| m15 | COc1ccc(cc1)C(=O)NCC | C2 |
| m24 | COc1ccc(cc1)C(=O)NCCC | C3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | C4 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | C5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | C6 |

## Step 2: Measure Congeners to Establish Trend

Measured all 5 congeners:
| mol_id | chain length | QED |
|--------|-------------|-----|
| m15 | C2 | 0.7621 |
| m24 | C3 | 0.7919 |
| m28 | C4 | 0.7518 |
| m17 | C5 | 0.7498 |
| m22 | C6 | 0.7377 |

**Observed trend**: QED peaks at C3 (0.7919), then declines monotonically from C3→C6.

## Step 3: Fit Trend and Extrapolate to C11

For extrapolation, focused on the declining portion (C3-C6) since C11 is beyond the peak.

Linear regression on C3-C6 data:
- x = [3, 4, 5, 6], y = [0.7919, 0.7518, 0.7498, 0.7377]
- Fit: QED ≈ 0.8319 - 0.01646 × chain_length
- For C11: QED ≈ 0.8319 - 0.01646×11 = **0.651**

Alternative check: From C6, extrapolate 5 carbons at average decline rate:
- C3→C6 drop: 0.7919 - 0.7377 = 0.0542 over 3 carbons = 0.0181/carbon
- C11 estimate: 0.7377 - 5×0.0181 = 0.647

**Final prediction: QED(m05) ≈ 0.65**

## Step 4: Select Backup Lead Compound

**Constraints**: QED ≥ 0.6, psa ≥ 40, not in m05's series

**Filtering by psa ≥ 40**:
- Series 1 (methoxy-phenyl, m05's series): psa = 38.33 — FAILS
- Series 2 (sugar-like): psa = 110.02 — PASSES
- Series 3 (nitro-phenyl): psa = 72.24 — PASSES
- Series 4 (biphenyl): psa = 29.1 — FAILS

**Candidates**: Series 2 (m01, m03, m07, m14, m18, m19, m21, m25, m27) and Series 3 (m02, m06, m09, m10, m12, m29, m31, m32, m33)

**Measurements** (budget: 17 total, 14 used for congeners):
- m01 (Series 2): QED = 0.3187 — FAILS (< 0.6)
- m02 (Series 3): QED = 0.6046 — PASSES!
- m12 (Series 3): QED = 0.4709 — FAILS

**Selected lead: m02**
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC (nitro-phenyl series, not m05's series) ✓

## Budget Summary
- 5 measurements for m05 congeners (m15, m24, m28, m17, m22)
- 3 measurements for lead screening (m01, m02, m12)
- Total: 8/17 measurements used
