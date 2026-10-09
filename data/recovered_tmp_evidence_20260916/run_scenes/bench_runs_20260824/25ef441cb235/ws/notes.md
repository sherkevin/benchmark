# QED Prediction Notes for m05

## Target
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (N-alkyl chain = 11 carbons)
- Scaffold: methoxybenzamide (COc1ccc(cc1)C(=O)N-)
- MW=305.46, ALogP=4.956, PSA=38.33, RotB=12, Arom=1

## Search Strategy
Searched the pool with the scaffold substring `COc1ccc(cc1)C(=O)N` — found 6 total matches across 2 pages (cost: 2 budget units). One match was m05 itself (the target). The remaining 5 are congeners differing only in N-alkyl chain length.

## Congeneric Series Data
| mol_id | chain (n) | MW | ALogP | RotB | QED (measured) |
|--------|-----------|------|-------|------|-----------------|
| m15 | 2 | 179.22 | 1.445 | 3 | 0.7621 |
| m24 | 3 | 193.25 | 1.835 | 4 | 0.7919 |
| m28 | 4 | 207.27 | 2.225 | 5 | 0.7518 |
| m17 | 5 | 221.30 | 2.615 | 6 | 0.7498 |
| m22 | 6 | 235.33 | 3.005 | 7 | 0.7377 |

Budget used: 5 measurements + 2 search pages = 7/8.

## Observed Trend
- QED peaks at n=3 (0.7919), then declines steadily:
  - n=2→3: +0.030 (increase to peak)
  - n=3→4: −0.040 (sharp initial drop)
  - n=4→5: −0.002 (near plateau)
  - n=5→6: −0.012 (continued decline)
- Overall from n=3 to n=6: QED drops by 0.054 over 3 carbons.
- The decline is driven primarily by increasing ALogP (each CH2 adds ~0.39 to ALogP) and increasing RotB, both of which are penalized by the QED desirability functions.

## Model Fitting: Quadratic Polynomial
Fit a quadratic QED(n) = a*n^2 + b*n + c to all 5 measured points via least-squares.

Solving the 3x3 normal equations system:
- a = -0.003264
- b = +0.017024
- c = +0.74932

**QED(n) = -0.003264*n^2 + 0.017024*n + 0.74932**

**Fit quality (residuals):**
- n=2: predicted 0.770, actual 0.762 (error +0.008)
- n=3: predicted 0.771, actual 0.792 (error -0.021)
- n=4: predicted 0.765, actual 0.752 (error +0.013)
- n=5: predicted 0.753, actual 0.750 (error +0.003)
- n=6: predicted 0.734, actual 0.738 (error -0.004)
- RMS error ~ 0.012

The quadratic peaks at n ~ 2.6 (between n=2 and n=3), consistent with the observed peak. It captures the non-linear decline driven by the exponential nature of QED desirability functions acting on linearly increasing ALogP and RotB.

## Extrapolation to n=11 (m05)
QED(11) = -0.003264 * 121 + 0.017024 * 11 + 0.74932
         = -0.39494 + 0.18726 + 0.74932
         = 0.542

### Cross-check: Quadratic in ln(QED)
Since QED is a geometric mean of exponential desirability functions, ln(QED) should be quadratic in n. Fitting ln(QED) = a*n^2 + b*n + c gives:
- a = -0.00428, b = +0.02230, c = -0.28865
- ln(QED(11)) = -0.00428*121 + 0.02230*11 - 0.28865 = -0.561
- QED(11) = exp(-0.561) = 0.570

The two quadratic models agree reasonably (0.54 vs 0.57). I use the direct QED-quadratic value of 0.54 as the primary prediction.

### Physical sanity check
At n=11, ALogP=4.956 is very high for a drug-like molecule (the measured congeners range 1.4-3.0). The QED heavily penalizes high lipophilicity. A drop from 0.738 (at n=6, ALogP=3.0) to ~0.54 (at n=11, ALogP=5.0) is physically plausible given the non-linear desirability penalties on ALogP and RotB.

## Prediction
**target_qed = 0.54**
