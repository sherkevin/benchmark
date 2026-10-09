# Notes: Predicting QED of m05

## Target
- Molecule: m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: methoxybenzamide (COc1ccc(cc1)C(=O)N-)
- N-alkyl chain: 11 carbons
- m05 has never been synthesized, so QED must be predicted by extrapolation.

## Strategy
1. Search pool using scaffold substring to find congeneric series members
2. Measure QED of found congeners
3. Fit trend of QED vs chain length
4. Extrapolate to 11-carbon chain

## Search Log

Searched for `COc1ccc(cc1)C(=O)N` — found 6 total matches (2 pages).
Budget cost: 2 search pages + 5 measurements = 7 units used.

## Congeneric Series Data

| mol_id | Chain (n) | MW | ALogP | PSA | RotB | QED |
|--------|-----------|-------|-------|-------|------|------|
| m15 | 2 | 179.22 | 1.445 | 38.33 | 3 | 0.7621 |
| m24 | 3 | 193.25 | 1.835 | 38.33 | 4 | 0.7919 |
| m28 | 4 | 207.27 | 2.225 | 38.33 | 5 | 0.7518 |
| m17 | 5 | 221.30 | 2.615 | 38.33 | 6 | 0.7498 |
| m22 | 6 | 235.33 | 3.005 | 38.33 | 7 | 0.7377 |
| m05 | 11 | 305.46 | 4.956 | 38.33 | 12 | ??? |

## Observed Trend

- QED peaks at n=3 (0.7919), then declines steadily.
- The peak at n=3 is physically motivated: ALogP=1.835 is near the QED optimum (~2), whereas n=2 has ALogP=1.445 which is below optimum.
- From n=3→6, QED drops by 0.054 over 3 carbons (avg -0.018/carbon).
- The decline should accelerate for longer chains because ALogP and RotB penalties increase (sigmoidal desirability functions in QED move into steeper regions).
- At n=11: ALogP=4.956 (far above optimum), RotB=12 (above Lipinski limit of 10), MW=305 (still acceptable).

## Model: Quadratic Fit on All 5 Points

Fitted QED(n) = a*n^2 + b*n + c using least squares on all 5 measured congeners.

Normal equations solved:
- a = -0.003264
- b = 0.017024
- c = 0.749321

**QED(n) = -0.003264 * n^2 + 0.017024 * n + 0.749321**

Verification (predicted vs actual):
- n=2: 0.770 vs 0.762 (error +0.008)
- n=3: 0.771 vs 0.792 (error -0.021)
- n=4: 0.765 vs 0.752 (error +0.013)
- n=5: 0.753 vs 0.750 (error +0.003)
- n=6: 0.734 vs 0.738 (error -0.004)

RMS fit error: 0.012 — good fit.

The quadratic captures:
1. The initial rise (ALogP approaching optimum)
2. The subsequent accelerating decline (ALogP and RotB penalties increasing)
3. The negative quadratic coefficient is physically consistent with bounded desirability functions

## Extrapolation to n=11

QED(11) = -0.003264 * 121 + 0.017024 * 11 + 0.749321
= -0.394944 + 0.187264 + 0.749321
= **0.542**

This prediction is consistent with:
- ALogP=4.956 being well into the QED penalty zone (desirability drops sharply for ALogP > 3)
- RotB=12 exceeding the Lipinski-preferred range
- A roughly 0.20 drop from the n=6 value, driven by compounding property penalties
- Cross-check: simple physical estimation using expected desirability changes for ALogP and RotB gives QED ≈ 0.50-0.55

## Final Prediction

**QED(m05) ≈ 0.54**

Congener IDs: m15, m24, m28, m17, m22
