# Notes: QED Prediction and Lead Selection

## Task Overview
- Predict QED of m05 (never synthesized; SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC; chain length 11)
- Pick a backup lead: measured QED ≥ 0.6, psa ≥ 40, not in m05's series

## Step 1: Identify the Target's Congeneric Series

m05 scaffold (from describe_structure): `COc1ccc(cc1)C(=O)N`, chain_len = 11

Pool molecules sharing this scaffold (all verified with describe_structure):

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15    | 2         | COc1ccc(cc1)C(=O)NCC |
| m24    | 3         | COc1ccc(cc1)C(=O)NCCC |
| m28    | 4         | COc1ccc(cc1)C(=O)NCCCC |
| m17    | 5         | COc1ccc(cc1)C(=O)NCCCCC |
| m22    | 6         | COc1ccc(cc1)C(=O)NCCCCCC |

**Congener IDs: m15, m24, m28, m17, m22**

## Step 2: Other Series Identified in Pool

Four congeneric series total:

1. **m05/methoxybenzamide series** (scaffold `COc1ccc(cc1)C(=O)N`): m15, m24, m28, m17, m22 — psa = 38.33
2. **Sugar/gluconamide series** (scaffold `OCC(O)C(O)C(O)C(=O)N`): m01, m03, m07, m14, m18, m19, m21, m25, m27 — psa = 110.02
3. **Nitrobenzamide series** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`): m02, m06, m09, m10, m12, m29, m31, m32, m33 — psa = 72.24
4. **Biphenyl series** (scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`): m04, m08, m11, m13, m16, m20, m23, m26, m30 — psa = 29.1

## Step 3: QED Measurements — m05's Series

| mol_id | chain_len | QED (measured) |
|--------|-----------|----------------|
| m15    | 2         | 0.7621         |
| m24    | 3         | 0.7919         |
| m28    | 4         | 0.7518         |
| m17    | 5         | 0.7498         |
| m22    | 6         | 0.7377         |

**Observed trend**: QED peaks at chain 3 (0.7919), then steadily declines through chains 4–6. The initial rise from chain 2→3 followed by a sustained decline gives a curved (non-linear) profile.

## Step 4: Extrapolation to Chain 11 (m05)

**Quadratic fit (degree 2)** using all 5 data points:
- Polynomial: QED(x) = 0.74932 + 0.017024·x − 0.003264·x²
- RMSE on fitted points: 0.01188
- **Prediction at x = 11: QED ≈ 0.5416**

The downward-opening parabola captures the observed curvature: QED rises, peaks near chain ~2.6, then declines. Extrapolating to chain 11 predicts continued decline due to increasing lipophilicity (alogp = 4.956 for m05).

For comparison, a linear fit predicted 0.695, but it fails to capture the visible curvature (the rise from chain 2→3 that the linear model smooths over). Per the task instructions, I did not assume a straight line.

## Step 5: Lead Compound Selection

### Pre-filtering (free constraints, checked before spending measurements)

**psa ≥ 40 filter** (via screen_candidates):
- Eliminates m05 series (psa = 38.33) and biphenyl series (psa = 29.1)
- Remaining eligible series: Sugar (psa = 110.02) and Nitro (psa = 72.24)

**Not in m05's series**: Both sugar and nitro series qualify.

### Measurements of Lead Candidates

I focused on the nitro series (moderate logP, moderate PSA — more likely to have good QED):

| mol_id | chain_len | alogp | QED (measured) |
|--------|-----------|-------|----------------|
| m09    | 2         | 1.345 | 0.5826         |
| m12    | 4         | 2.125 | 0.4709         |
| m33    | 6         | 2.905 | 0.4591         |

### Result

Unfortunately, none of the measured non-m05-series molecules achieved QED ≥ 0.6. The nitro series shows declining QED with increasing chain length (0.5826 → 0.4709 → 0.4591). The shortest-chain member m09 came closest at 0.5826.

With the budget exhausted (17/17: 9 listing pages + 5 congener measurements + 3 lead measurements), I could not measure sugar-series candidates. In hindsight, I would have listed fewer pages and allocated more budget to measurements.

### Lead Chosen: **m09**

- Measured QED: 0.5826 (closest to 0.6 among measured candidates from other series)
- PSA: 72.24 (≥ 40 ✓)
- Series: Nitro series (not m05's series ✓)
- Caveat: Does not quite reach the 0.6 QED threshold

## Budget Summary

| Action | Count | Budget Used |
|--------|-------|-------------|
| list_pool pages | 9 | 9 |
| measure_qed (congeners) | 5 | 5 |
| measure_qed (lead candidates) | 3 | 3 |
| **Total** | **17** | **17/17** |

## Final Answers

- **target_qed**: 0.5416
- **lead_mol_id**: m09
- **lead_qed_measured**: 0.5826
- **congener_ids**: ["m15", "m24", "m28", "m17", "m22"]
