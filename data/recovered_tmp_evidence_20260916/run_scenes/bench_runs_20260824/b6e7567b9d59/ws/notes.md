# Notes: QED Prediction for m05

## Target
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide with 11-carbon N-alkyl chain (undecyl)
- Never synthesized → must predict QED by extrapolation

## Search Strategy
Searched pool using scaffold substring `COc1ccc(cc1)C(=O)N` from the target SMILES.
- Page 1 (budget 1): found m05, m15, m17, m22 (4 of 6 matches)
- Page 2 (budget 2): found m24, m28 (remaining 2 matches)
- Total: 6 matches, 5 congeners besides the target m05

## Congeneric Series Data

| mol_id | N-alkyl  | n (C atoms) | MW     | ALogP | PSA   | RotB | Arom | QED    |
|--------|----------|-------------|--------|-------|-------|------|------|--------|
| m15    | ethyl    | 2           | 179.22 | 1.445 | 38.33 | 3    | 1    | 0.7621 |
| m24    | propyl   | 3           | 193.25 | 1.835 | 38.33 | 4    | 1    | 0.7919 |
| m28    | butyl    | 4           | 207.27 | 2.225 | 38.33 | 5    | 1    | 0.7518 |
| m17    | pentyl   | 5           | 221.30 | 2.615 | 38.33 | 6    | 1    | 0.7498 |
| m22    | hexyl    | 6           | 235.33 | 3.005 | 38.33 | 7    | 1    | 0.7377 |
| m05    | undecyl  | 11          | 305.46 | 4.956 | 38.33 | 12   | 1    | ???    |

Budget used: 2 (searches) + 5 (QED measurements) = 7 of 12.

## Observed Trend
- QED peaks at n=3 (propyl, QED=0.7919) — ALogP ≈ 1.8 is near optimal
- QED declines for n≥3 as ALogP and RotB grow beyond desirable ranges
- Step-by-step changes: +0.030 (2→3), −0.040 (3→4), −0.002 (4→5), −0.012 (5→6)
- Only PSA, aromatic rings, HBA, HBD stay constant; MW, ALogP, RotB increase linearly with n

## Model Fitted: Quadratic on ln(QED)

**Physical motivation:** QED = exp(Σ wᵢ·ln(dᵢ)/Σ wᵢ), a weighted geometric mean of desirability functions dᵢ. Since the changing descriptors (ALogP, RotB, MW) are each linear in n, and each ln(dᵢ) is approximately quadratic in its descriptor near the peak, ln(QED) should be approximately quadratic in n.

**Least-squares fit:** ln(QED) = a·n² + b·n + c

Data: n = [2, 3, 4, 5, 6], ln(QED) = [−0.2717, −0.2333, −0.2853, −0.2880, −0.3043]

Normal equations solved:
- a = −0.004306
- b = +0.02249
- c = −0.28895

**Model:** ln(QED) = −0.004306·n² + 0.02249·n − 0.28895

**Goodness of fit (QED scale):**
- n=2: predicted 0.770, actual 0.762 (error +0.008)
- n=3: predicted 0.771, actual 0.792 (error −0.021)
- n=4: predicted 0.765, actual 0.752 (error +0.013)
- n=5: predicted 0.753, actual 0.750 (error +0.003)
- n=6: predicted 0.734, actual 0.738 (error −0.004)

Peak of the fitted curve at n = −b/(2a) = 2.62, consistent with observed peak near n=3.

## Extrapolation to m05 (n=11)

ln(QED) = −0.004306 × 121 + 0.02249 × 11 − 0.28895
        = −0.5210 + 0.2474 − 0.2890
        = −0.5626

QED = exp(−0.5626) ≈ **0.57**

## Cross-check: Direct Quadratic on QED

QED = −0.003264·n² + 0.01702·n + 0.74934
QED(11) = −0.3949 + 0.1872 + 0.7493 = 0.54

Both quadratic models agree within ~0.03, giving a consistent range of 0.54–0.57.

## Why 0.57 Is Physically Reasonable

At n=11, the molecule has:
- ALogP ≈ 4.96 — approaching the QED penalty zone upper boundary (~5.6), strongly penalized
- RotB = 12 — well above the desirable range (≤9), heavily penalized
- MW = 305 — still in the drug-like range (150–500), minimal penalty
- PSA = 38.33 — low, favorable

The accelerating decline captured by the quadratic reflects compounding penalties from ALogP and RotB as the chain grows.

## Final Answer
**Predicted QED for m05: 0.57**
Congener IDs: m15, m24, m28, m17, m22
