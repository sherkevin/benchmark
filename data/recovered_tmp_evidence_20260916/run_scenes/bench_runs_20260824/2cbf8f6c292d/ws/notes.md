# Notes: QED Prediction for m05

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (N-undecyl chain, n=11 carbons)
- MW=305.46, ALogP=4.956, PSA=38.33, RotB=12, AromRings=1
- Never synthesized → must predict QED

## Search Strategy
Searched pool using scaffold substring `COc1ccc(cc1)C(=O)N` from the target SMILES. Found 6 total matches (2 pages, costing 2 budget units).

## Congeneric Series Identified
All molecules share the scaffold `COc1ccc(cc1)C(=O)N` and differ only in N-alkyl chain length:

| mol_id | SMILES | Chain (n) | MW | ALogP | RotB | QED (measured) |
|--------|--------|-----------|-------|-------|------|----------------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 | 179.22 | 1.445 | 3 | 0.7621 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 | 193.25 | 1.835 | 4 | 0.7919 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 | 207.27 | 2.225 | 5 | 0.7518 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 | 221.30 | 2.615 | 6 | 0.7498 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 | 235.33 | 3.005 | 7 | 0.7377 |
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 11 | 305.46 | 4.956 | 12 | ??? |

Budget used: 2 (search) + 5 (measurements) = 7 out of 12.

## Observed Trend
- QED peaks at n=3 (0.792), then declines monotonically
- Changes per CH2: +0.030, -0.040, -0.002, -0.012
- Average decline from n=3 to n=6: (0.7919 - 0.7377)/3 = 0.0181 per CH2
- As chain grows, MW, ALogP, and RotB all increase linearly; PSA, HBD, HBA, AromRings stay constant

## Models Fitted

### 1. Quadratic (least-squares on all 5 points, x = n-4)
QED(n) = -0.003264*(n-4)² - 0.00909*(n-4) + 0.76519
- Captures the peak at n≈2.6 and subsequent decline
- QED(11) = -0.003264*49 - 0.00909*7 + 0.76519 = **0.542**
- Residuals: n=2: +0.008, n=3: -0.021, n=4: +0.013, n=5: +0.003, n=6: -0.004

### 2. Linear (n=3 to n=6 only)
QED(n) = 0.83187 - 0.01646*n
- QED(11) = **0.651**

### 3. Log-linear (QED = a - b*ln(n), fit on n=3-6)
QED(n) = 0.86623 - 0.07369*ln(n)
- QED(11) = **0.690**

### 4. Inverse-linear (1/QED = a + b*n, all 5 pts)
1/QED(n) = 1.25580 + 0.01577*n
- QED(11) = **0.700**

## Extrapolation Reasoning

The quadratic model captures the non-linear peak-and-decline shape best (as instructed by "do not assume a straight line"), but may over-extrapolate because ADS penalty functions saturate at extreme property values (sigmoidal shape). The linear, log-linear, and inverse models all predict 0.65-0.70.

The physical basis for decline: as chain length increases from n=6 to n=11:
- ALogP goes from 3.0 to 5.0 (entering heavily penalized region)
- MW goes from 235 to 305 (moderate penalty increase)  
- RotB goes from 7 to 12 (significant penalty increase)

These compound via the geometric mean in QED, causing accelerating decline relative to the n=3-6 range.

I take a weighted compromise between the quadratic (0.54) which captures non-linearity but over-extrapolates, and the log/linear models (0.65-0.70) which assume constant or decelerating decline. Giving the quadratic model significant weight due to the task's emphasis on non-linearity and the observed accelerating drop pattern (n4-5=-0.002, n5-6=-0.012):

**Final prediction: QED(m05) = 0.60**

This represents ~19% decline from n=6 (vs ~3% decline over n=3-6), reflecting the compounded ADS penalties at high ALogP and RotB values.
