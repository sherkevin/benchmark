# Notes: QED Prediction and Lead Selection

## Job 1 — Predict the QED of m05

### Identifying the congeneric series of m05

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — scaffold `COc1ccc(cc1)C(=O)N` (methoxybenzamide) with an N-alkyl chain of 11 carbons.

Pool molecules sharing this scaffold (confirmed via `describe_structure`):

| mol_id | SMILES | chain_len |
|--------|--------|-----------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 |

**Congener IDs: m15, m24, m28, m17, m22**

### Measured QEDs for the series

| mol_id | chain_len | QED |
|--------|-----------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend analysis

The QED peaks at chain_len=3 (0.7919) then declines as chain length increases. This curvature is expected: as the alkyl chain grows, logP increases (m05 has alogP=4.956) pushing the molecule out of the drug-like sweet spot.

### Polynomial fit (degree 2)

Using `fit_trend` with all 5 points, degree=2:
- Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- Equation: QED = 0.74932 + 0.017024·x − 0.003264·x²
- RMSE on training points: 0.01188

Comparison with other degrees:
- Degree 1 (linear): predicts 0.695 at x=11 (RMSE 0.01308) — does not capture the curvature
- Degree 2 (quadratic): predicts 0.5416 at x=11 (RMSE 0.01188) — captures curvature well
- Degree 3 (cubic): predicts 2.1323 at x=11 — clearly overfitting (QED cannot exceed 1)

The quadratic is the best choice: it fits the data well, captures the non-linear trend (as instructed), and gives a physically plausible extrapolation.

### Predicted QED for m05

**QED(m05) ≈ 0.5416**

At chain_len=11: 0.74932 + 0.017024×11 − 0.003264×121 = 0.5416

---

## Job 2 — Lead Compound Selection

### Constraints

1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

### Pre-screening (free)

Using `screen_candidates()` (PSA ≥ 40) gave 18 molecules. Eliminating m05's series (methoxybenzamide, PSA=38.33) and the biphenyl series (PSA=29.1) left two series:

- **Sugar alcohol** (PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- **Nitrobenzene** (PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements for lead candidates (7 remaining budget)

| mol_id | series | chain_len | QED | Pass? |
|--------|--------|-----------|-------|-------|
| m09 | nitro | 2 | 0.5826 | No (<0.6) |
| m02 | nitro | 3 | 0.6046 | **Yes** |
| m12 | nitro | 4 | 0.4709 | No (<0.6) |
| m32 | nitro | 5 | 0.4683 | No (<0.6) |
| m33 | nitro | 6 | 0.4591 | No (<0.6) |
| m14 | sugar | 2 | 0.3237 | No (<0.6) |
| m27 | sugar | 3 | 0.3330 | No (<0.6) |

### Selected lead

**m02** — the only molecule measured that satisfies all three constraints:
- Measured QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Nitrobenzene series (not m05's methoxybenzamide series) ✓

The nitrobenzene series showed a clear trend: shorter chains gave higher QED (peaking at m02 with 3C), then declining for longer chains. The sugar alcohol series had very low QEDs due to extreme PSA (110) and very negative logP values.
