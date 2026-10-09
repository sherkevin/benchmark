# Notes: Predicting QED of m05

## Target molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxyphenyl amide with 11-carbon N-alkyl chain
- Scaffold: `COc1ccc(cc1)C(=O)N` (p-methoxybenzamide)

## Search strategy
Searched for the scaffold substring `COc1ccc(cc1)C(=O)N` in the pool. Found 6 matches across 2 pages (budget cost: 2 units).

## Congeneric series identified
All molecules share the scaffold `COc1ccc(cc1)C(=O)N` and differ only in N-alkyl chain length:

| mol_id | Chain (n C) | SMILES suffix | MW     | ALOGP | PSA   | ROTB | AROM | QED (measured) |
|--------|------------|---------------|--------|-------|-------|------|------|----------------|
| m15    | 2          | NCC           | 179.22 | 1.445 | 38.33 | 3    | 1    | 0.7621         |
| m24    | 3          | NCCC          | 193.25 | 1.835 | 38.33 | 4    | 1    | 0.7919         |
| m28    | 4          | NCCCC         | 207.27 | 2.225 | 38.33 | 5    | 1    | 0.7518         |
| m17    | 5          | NCCCCC        | 221.30 | 2.615 | 38.33 | 6    | 1    | 0.7498         |
| m22    | 6          | NCCCCCC       | 235.33 | 3.005 | 38.33 | 7    | 1    | 0.7377         |
| **m05**| **11**     | NCCCCCCCCCCC  | 305.46 | 4.956 | 38.33 | 12   | 1    | **???**        |

Budget cost for measurements: 5 units. Total budget used: 7/12.

## Observed trend
- QED **peaks at n=3** (0.7919), then **declines** as chain lengthens
- n=2→3: +0.0298 (chain too short at n=2, suboptimal lipophilicity)
- n=3→4: −0.0401
- n=4→5: −0.0020
- n=5→6: −0.0121
- Overall n=3→6: decline of 0.0542 over 3 CH₂ additions

The peak at n=3 is physically motivated: ALOGP ~1.8 is near-optimal for drug-likeness. Shorter chains are too hydrophilic; longer chains become too lipophilic.

## Model: Quadratic fit on all 5 measured points

Fitted QED(n) = a + bn + cn² by least-squares regression on all 5 congeners.

### Normal equations
Using Σn=20, Σn²=90, Σn³=440, Σn⁴=2274, ΣQED=3.7933, Σ(n·QED)=15.0823, Σ(n²·QED)=67.5065:

- c = −0.003264
- b = +0.017024
- a = 0.749321

### Fitted model
**QED(n) = 0.7493 + 0.0170·n − 0.00326·n²**

### Fit quality (residuals)
| n | Predicted | Actual  | Residual |
|---|-----------|---------|----------|
| 2 | 0.7703    | 0.7621  | +0.008   |
| 3 | 0.7710    | 0.7919  | −0.021   |
| 4 | 0.7652    | 0.7518  | +0.013   |
| 5 | 0.7528    | 0.7498  | +0.003   |
| 6 | 0.7340    | 0.7377  | −0.004   |

RMS residual ≈ 0.012 — good fit for 5 points with 3 parameters.

### Why quadratic?
- The data shows a clear peak (n=3) followed by decline → linear is inappropriate (as instructed)
- The concave-down curvature reflects compounding penalties: each additional CH₂ increases ALOGP (~+0.39) and ROTB (+1), both of which incur accelerating desirability penalties in the QED formula
- Quadratic is the simplest model capturing this curvature (3 parameters for 5 data points)

## Extrapolation to m05 (n=11)

QED(11) = 0.7493 + 0.0170(11) − 0.00326(121)
        = 0.7493 + 0.1872 − 0.3949
        = **0.5416**

### Sanity check
At n=11: ALOGP=4.96 (very high, heavily penalized in QED), ROTB=12 (high, penalized), MW=305 (moderate, acceptable). A QED of ~0.54 is consistent with a molecule that has significant lipophilicity and flexibility penalties but retains some drug-like characteristics (aromatic ring, moderate MW, low PSA).

### Alternative model (ln-QED quadratic)
Fitting ln(QED) = a + bn + cn² gives QED(11) ≈ 0.570. Both models agree on a substantial decline from the measured range.

### Cross-check (3-point quadratic on n=4,5,6)
Exact quadratic through last 3 points: QED(11) ≈ 0.526. Consistent with the 5-point fit.

## Final prediction
**QED(m05) ≈ 0.54**
