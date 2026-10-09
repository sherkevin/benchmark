# Notes: Medicinal Chemistry Analysis

## Pool Structure
33 molecules in 4 congeneric series:
1. **m05 series** (methoxybenzamide, scaffold `COc1ccc(cc1)C(=O)N`): m15(C2), m24(C3), m28(C4), m17(C5), m22(C6), m05(C11-target)
2. **Sugar series** (scaffold `OCC(O)C(O)C(O)C(=O)N`): m14(C2), m27(C3), m01(C4), m19(C5), m03(C6), m21(C7), m18(C8), m07(C9), m25(C10)
3. **Nitrobenzene series** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`): m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10)
4. **Biphenyl series** (scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`): m13(C2), m11(C3), m04(C4), m30(C5), m23(C6), m20(C7), m26(C8), m08(C9), m16(C10)

## Budget Usage
- Paging (9 pages): 9 units
- m05 series measurements (5 molecules): 5 units  
- Job 2 candidate measurements (3 molecules): 3 units
- Total: 17/17

## Job 1: m05 QED Prediction

### Measured QED values for m05 series:
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Trend
QED rises from chain 2 to 3 (0.7621 to 0.7919), then declines steadily from chain 3 to 6.
This is a non-linear (inverted-U) pattern.

### Quadratic fit (degree 2):
Coefficients: 0.74932 + 0.017024*x - 0.003264*x^2
RMSE on training points: 0.01188

At x=11 (m05): **predicted QED = 0.5416**

### Linear fit (for comparison):
At x=11: 0.695 (RMSE 0.01308)

The quadratic is preferred because:
1. The task says "do not assume the relationship is a straight line"
2. The data clearly shows curvature (rise then fall)
3. Slightly lower RMSE (0.01188 vs 0.01308)
4. m05 has very high ALogP (4.956) which QED penalizes heavily, consistent with a larger drop

### Prediction: m05 QED ~ 0.54

## Job 2: Lead Compound Selection

### PSA screen
Only sugar series (PSA 110.02) and nitrobenzene series (PSA 72.24) pass PSA >= 40.
Biphenyl (PSA 29.1) and m05 series (PSA 38.33) fail.

### Measurements on non-m05-series candidates:
| mol_id | series        | chain | QED    | PSA    |
|--------|---------------|-------|--------|--------|
| m12    | nitrobenzene  | 4     | 0.4709 | 72.24  |
| m32    | nitrobenzene  | 5     | 0.4683 | 72.24  |
| m33    | nitrobenzene  | 6     | 0.4591 | 72.24  |

All three fail QED >= 0.6. Extrapolating the nitrobenzene trend to shorter chains (C2, C3) predicts ~0.48 max, still below 0.6.

The sugar series (very negative ALogP, high PSA, no aromatic rings, many HBD) would likely have even lower QED.

Budget was exhausted before any qualifying lead could be confirmed. m12 is selected as the best available measured candidate (highest QED among PSA-passing, non-m05-series molecules).

## Strategic Lesson
Paging through all 9 pages consumed 9/17 budget. Should have allocated more budget to Job 2 candidate screening.
