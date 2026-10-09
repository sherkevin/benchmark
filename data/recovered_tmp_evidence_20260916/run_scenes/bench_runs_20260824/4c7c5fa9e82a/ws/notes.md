# Notes: QED Prediction and Lead Selection

## Job 1 — Predicting QED of m05

### Identifying the congeneric series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. Using `describe_structure`:
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- Chain length: 11

Pool molecules sharing the same scaffold:
| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC |

**Congener IDs: m15, m24, m28, m17, m22**

### Measured QED values

| mol_id | chain_len | QED |
|--------|-----------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend

QED rises slightly from chain=2 to chain=3 (0.7621→0.7919), then declines steadily through chains 4–6 (0.7518→0.7498→0.7377). The relationship is not linear — there is curvature.

### Fitted model (degree-2 polynomial, all 5 points)

Coefficients (low to high): [0.74932, 0.017024, -0.003264]
- RMSE on fitted points: 0.01188

### Extrapolation to chain=11 (m05)

**Predicted QED = 0.5416**

This is a significant extrapolation (from chain range 2–6 to 11), so reliability is limited. However:
- Degree 1 (linear, all 5 points) gave 0.695 — likely overestimates because it doesn't capture the curvature
- Degree 3 (cubic, all 5 points) gave 2.1323 — clearly overfitting and unphysical
- The degree-2 fit is the best balance: captures the peak-and-decline pattern without overfitting

Physical rationale: m05 has logP=4.956 (very lipophilic), which is penalized by QED. The declining trend is consistent with increasing lipophilicity as the chain grows.

## Job 2 — Lead Compound Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's series (scaffold ≠ COc1ccc(cc1)C(=O)N)

### Pre-screening (free)

Used `screen_candidates()` to find molecules with PSA ≥ 40: 18 molecules passed.
Removed m05-series molecules (m15, m17, m22, m24, m28 — all have PSA=38.33, already below threshold).

Remaining candidates fell into two series:
- **Sugar series** (scaffold: OCC(O)C(O)C(O)C(=O)N, PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- **Nitro series** (scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N, PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements (7 remaining budget)

| mol_id | series | QED | Pass QED≥0.6? |
|--------|--------|------|---------------|
| m09 | nitro | 0.5826 | No |
| m12 | nitro | 0.4709 | No |
| m02 | nitro | 0.6046 | **Yes** |
| m33 | nitro | 0.4591 | No |
| m32 | nitro | 0.4683 | No |
| m14 | sugar | 0.3237 | No |
| m01 | sugar | 0.3187 | No |

### Chosen lead: m02

- **QED = 0.6046** (measured, ≥ 0.6) ✓
- **PSA = 72.24** (≥ 40) ✓
- **Series**: nitro (O=[N+]([O-])c1ccc(cc1)C(=O)N), NOT the m05 series ✓

The sugar series molecules had very low QED (0.31–0.32) due to extremely high PSA and negative logP. The nitro series was more promising, with shorter-chain members having higher QED. m02 (chain=3, MW=208.22, logP=1.735) was the best candidate found, just passing the 0.6 threshold.

## Budget used: 12/12 measurements
