# Notes — Medicinal Chemistry Task

## Series Identification

### m05 series (methoxyphenyl scaffold: `COc1ccc(cc1)C(=O)N`)
Using `describe_structure`, I confirmed these pool molecules share the same scaffold as m05:

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC |
| m05 | 11 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC (target, never synthesized) |

### Other series identified
- **Sugar series** (`OCC(O)C(O)C(O)C(=O)N`): m01, m03, m07, m14, m18, m19, m21, m25, m27 — PSA=110.02
- **Nitro series** (`O=[N+]([O-])c1ccc(cc1)C(=O)N`): m02, m06, m09, m10, m12, m29, m31, m32, m33 — PSA=72.24
- **Biphenyl series** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N`): m04, m08, m11, m13, m16, m20, m23, m26, m30 — PSA=29.1 (all fail PSA≥40)

## Job 1 — QED Prediction for m05

### Measured QEDs in m05's series (5 measurements)

| mol_id | chain_len | QED |
|--------|-----------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend Analysis

The QED peaks at chain length 3 (0.7919) and then steadily declines. The relationship is clearly non-linear.

### Polynomial Fits (evaluated at chain_len=11)

| Degree | Prediction | RMSE | Notes |
|--------|-----------|------|-------|
| 1 (linear) | 0.695 | 0.01308 | Task says don't assume linear |
| 2 (quadratic, all 5 pts) | **0.5416** | 0.01188 | Best balance of fit and physical plausibility |
| 3 (cubic, all 5 pts) | 2.1323 | 0.00835 | Impossible (>1), overfitting |

**Chosen prediction: 0.5416** (degree 2, all 5 points). This captures the rise-then-fall curvature observed in the data and gives a physically reasonable extrapolation. The quadratic coefficient (-0.003264) drives the decline at longer chain lengths, consistent with increasing lipophilicity (alogp=4.956 for m05) penalizing drug-likeness.

## Job 2 — Lead Compound Selection

### Pre-screening (free constraints)
- PSA ≥ 40: eliminates all biphenyl series (PSA=29.1) and all m05-series members (PSA=38.33)
- Not in m05's series: eliminates m15, m17, m22, m24, m28
- Remaining candidates: sugar series (9 molecules, PSA=110.02) and nitro series (9 molecules, PSA=72.24)

### QED Measurements for Lead Candidates (6 measurements)

| mol_id | series | chain_len | QED | QED≥0.6? |
|--------|--------|-----------|-------|---------|
| m09 | nitro | 2 | 0.5826 | No |
| m02 | nitro | 3 | 0.6046 | **Yes** |
| m12 | nitro | 4 | 0.4709 | No |
| m32 | nitro | 5 | 0.4683 | No |
| m14 | sugar | 2 | 0.3237 | No |
| m01 | sugar | 4 | 0.3187 | No |

### Chosen Lead: **m02**
- Measured QED: **0.6046** (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Series: nitro (`O=[N+]([O-])c1ccc(cc1)C(=O)N`) — different from m05 ✓
- All three constraints satisfied.

Sugar series molecules had very low QED (~0.32) due to extreme polarity (negative logP, high PSA). The nitro series showed a peak at chain 3 (m02) with QED dropping for longer chains.

## Budget Used
- 5 measurements for m05 series characterization
- 6 measurements for lead candidate screening
- Total: 11 out of 12 budget used
