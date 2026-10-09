# Notes: Predicting QED of m05

## Target
- Molecule: m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Scaffold: COc1ccc(cc1)C(=O)N- (4-methoxybenzamide)
- N-alkyl chain: 11 carbons (CCCCCCCCCCC)
- Descriptors: MW=305.46, ALogP=4.956, PSA=38.33, RotB=12, Arom=1

## Search Strategy
Searched pool for scaffold substring `COc1ccc(cc1)C(=O)N` — found 6 total matches (2 pages).
One match was m05 itself (target); the other 5 are congeners with shorter chains.

## Congeners Found and Measured

| mol_id | Chain | SMILES | MW | ALogP | RotB | QED |
|--------|-------|--------|------|-------|------|-------|
| m15 | 2 | ...NCC | 179.22 | 1.445 | 3 | 0.7621 |
| m24 | 3 | ...NCCC | 193.25 | 1.835 | 4 | 0.7919 |
| m28 | 4 | ...NCCCC | 207.27 | 2.225 | 5 | 0.7518 |
| m17 | 5 | ...NCCCCC | 221.30 | 2.615 | 6 | 0.7498 |
| m22 | 6 | ...NCCCCCC | 235.33 | 3.005 | 7 | 0.7377 |

## Observed Trend
- QED peaks at chain length 3 (QED=0.7919)
- Then declines: chain 4 (0.7518), chain 5 (0.7498), chain 6 (0.7377)
- The decline is driven by increasing ALogP (toward unfavorable >3.5) and increasing RotB (toward unfavorable >7-8)
- MW stays in favorable range (179→235 measured, 305 at target)

## Model: Quadratic Fit on All 5 Data Points
Centered variable u = chain_length - 4

Normal equations yield:
**QED(u) = 0.765188 - 0.00909·u - 0.003264·u²**

Equivalently: QED(chain) = 0.749324 + 0.017022·chain - 0.003264·chain²

Fit quality (residuals):
- Chain 2: predicted 0.770, actual 0.762 (error -0.008)
- Chain 3: predicted 0.771, actual 0.792 (error +0.021)
- Chain 4: predicted 0.765, actual 0.752 (error -0.013)
- Chain 5: predicted 0.753, actual 0.750 (error -0.003)
- Chain 6: predicted 0.734, actual 0.738 (error +0.004)

The quadratic captures the inverted-U shape (peak at chain ~2.6) and subsequent decline.
The negative quadratic coefficient means the decline accelerates with chain length.

## Extrapolation to Chain 11
u = 11 - 4 = 7

QED(11) = 0.765188 - 0.00909(7) - 0.003264(49)
= 0.765188 - 0.06363 - 0.159936
= **0.542**

## Physical Justification
At chain 11, the molecule has:
- ALogP = 4.956 (near Lipinski limit of 5; QED ALogP desirability drops sharply above ~3.5)
- RotB = 12 (well above the favorable range of ≤7; QED RotB desirability is low)
- MW = 305 (still in favorable drug-like range)

The combination of high lipophilicity and many rotatable bonds substantially penalizes QED
relative to the measured congeners. The quadratic model's prediction of ~0.54 is consistent
with independent estimation via desirability function ratios:
- d_ALogP(4.96)/d_ALogP(3.01) ≈ 0.15-0.25
- d_RotB(12)/d_RotB(7) ≈ 0.30-0.43
- QED(11) ≈ QED(6) × (product_ratio)^(1/8) ≈ 0.50-0.55

## Budget Usage
- search_pool page 1: 1 unit
- search_pool page 2: 1 unit
- measure_qed × 5 (m15, m24, m28, m17, m22): 5 units
- Total: 7/12 units used

## Final Prediction
**target_qed = 0.54**
