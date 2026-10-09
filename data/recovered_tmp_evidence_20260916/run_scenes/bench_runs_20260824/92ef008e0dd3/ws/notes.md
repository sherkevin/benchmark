# Notes: Predicting QED of m05

## Search Strategy
Searched for the scaffold substring `COc1ccc(cc1)C(=O)N` from the target SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. This found 6 matches (including m05 itself) across 2 pages. Cost: 2 budget units.

## Congeneric Series Found
All molecules share the scaffold `COc1ccc(cc1)C(=O)N-` and differ only in N-alkyl chain length:

| mol_id | SMILES suffix | Chain C# | MW     | ALogP | ROTB | QED (measured) |
|--------|-------------|----------|--------|-------|------|----------------|
| m15    | NCC         | 2        | 179.22 | 1.445 | 3    | 0.7621         |
| m24    | NCCC        | 3        | 193.25 | 1.835 | 4    | 0.7919         |
| m28    | NCCCC       | 4        | 207.27 | 2.225 | 5    | 0.7518         |
| m17    | NCCCCC      | 5        | 221.30 | 2.615 | 6    | 0.7498         |
| m22    | NCCCCCC     | 6        | 235.33 | 3.005 | 7    | 0.7377         |
| **m05**| **NCCCCCCCCCCC** | **11** | **305.46** | **4.956** | **12** | **?** |

Cost: 5 measurement units (m15, m24, m28, m17, m22). Total budget used: 7/12.

## Observed Trend
1. QED **peaks** at chain length 3 (m24, QED = 0.7919)
2. QED then **declines** from n=3 to n=6: 0.7919 → 0.7518 → 0.7498 → 0.7377
3. First differences: +0.030, -0.040, -0.002, -0.012
4. The decline appears to **accelerate** (second differences are negative overall)

## Fitted Model: Quadratic in ln(QED)

**Rationale**: QED is a weighted geometric mean of ADS (property satisfaction) scores. Therefore ln(QED) is a weighted sum of ln(ADS_i). As ALogP and ROTB increase linearly with chain length, their ADS scores decrease, and ln(ADS) decreases approximately quadratically. Thus ln(QED) should be approximately quadratic in chain length.

Setting x = n - 4, the least-squares quadratic fit to ln(QED) vs. x:

- a0 = -0.2680
- a1 = -0.0120  
- a2 = -0.00428

**ln(QED) = -0.2680 - 0.0120*(n-4) - 0.00428*(n-4)^2**

### Fit Quality (residuals in QED space)
- n=2: predicted 0.770, actual 0.762 (resid +0.008)
- n=3: predicted 0.771, actual 0.792 (resid -0.021)
- n=4: predicted 0.765, actual 0.752 (resid +0.013)
- n=5: predicted 0.753, actual 0.750 (resid +0.003)
- n=6: predicted 0.734, actual 0.738 (resid -0.004)

## Extrapolation to n=11

x = 11 - 4 = 7

ln(QED) = -0.2680 - 0.0120*(7) - 0.00428*(49)
        = -0.2680 - 0.0837 - 0.2095
        = -0.5612

**QED = exp(-0.5612) ≈ 0.571**

## Sanity Check
At n=11: ALogP=4.956 (near Lipinski limit of 5), ROTB=12 (exceeds rule-of-10 threshold). Both are in penalty territory, so a significant QED drop from n=6 (QED=0.738) is expected. A drop from 0.738 to ~0.57 (about 23%) is chemically reasonable.

## Prediction
**QED(m05) ≈ 0.57**
