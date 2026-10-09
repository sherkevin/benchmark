# Notes on QED Prediction and Lead Selection Task

## Understanding the Task

1. **Target molecule m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - has a methoxy-phenyl-amide scaffold with a C13 alkyl chain (13 carbons in the N-alkyl chain)
2. Need to find congeners (same scaffold, different chain length) in the pool
3. Measure QED for congeners, fit a trend, extrapolate to predict m05's QED
4. Pick a backup lead: QED >= 0.6, psa >= 40, NOT in m05's series

## Identifying Congeneric Series

Looking at the SMILES patterns:

**m05's scaffold**: `COc1ccc(cc1)C(=O)N-` followed by alkyl chain
- This is a 4-methoxybenzamide scaffold

Molecules with the same `COc1ccc(cc1)C(=O)N` scaffold (m05's series):
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - C13 chain (target, not measurable)
- m15: `COc1ccc(cc1)C(=O)NCC` - C2 chain
- m24: `COc1ccc(cc1)C(=O)NCCC` - C3 chain
- m28: `COc1ccc(cc1)C(=O)NCCCC` - C4 chain
- m17: `COc1ccc(cc1)C(=O)NCCCCC` - C5 chain
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` - C6 chain

So the congeners in the pool for m05's series are: **m15, m17, m22, m24, m28**

Chain lengths:
- m15: NCC = 2 carbons
- m24: NCCC = 3 carbons
- m28: NCCCC = 4 carbons
- m17: NCCCCC = 5 carbons
- m22: NCCCCCC = 6 carbons
- m05: NCCCCCCCCCCC = 13 carbons (target)

## Other Series in the Pool

1. **OCC(O)C(O)C(O)C(=O)N-** (polyol amide): m01, m03, m07, m14, m18, m19, m21, m25, m27
2. **O=[N+]([O-])c1ccc(cc1)C(=O)N-** (nitrobenzamide): m02, m06, m09, m10, m12, m29, m31, m32, m33
3. **c1ccc(cc1)-c1ccc(cc1)C(=O)N-** (biphenyl amide): m04, m08, m11, m13, m16, m20, m23, m26, m30

## Strategy

1. First, screen candidates for psa >= 40 (free)
2. Identify which candidates are NOT in m05's series
3. Measure QED for potential leads
4. Measure QED for m05 congeners to fit the trend

## Candidate Screening (psa >= 40)

18 molecules pass the psa >= 40 filter:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

None of these are in m05's series (m15, m17, m22, m24, m28 have psa = 38.33 < 40, so they don't pass).

All 18 passing molecules are candidates for the lead compound.

## Measuring m05 Congeners

Measured all 5 congeners to establish the trend:
- m15 (C2): QED = 0.7621
- m24 (C3): QED = 0.7919
- m28 (C4): QED = 0.7518
- m17 (C5): QED = 0.7498
- m22 (C6): QED = 0.7377

The trend shows a peak at C3, then gradual decline.

## Fitting the Trend

Used quadratic fit (degree=2) as the task says "Do not assume the relationship is a straight line":

**Quadratic fit (degree=2):**
- Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
- RMSE: 0.01188
- Prediction for x=13: **0.419**

For comparison, linear fit (degree=1):
- Coefficients: 0.79502 - 0.00909*x
- RMSE: 0.01308
- Prediction for x=13: 0.6768

The quadratic fit has lower RMSE (better fit) and captures the non-linear trend. Using quadratic prediction: **m05 QED = 0.419**

## Lead Compound Selection

Measured QED for candidates (all have psa >= 40 and are not in m05's series):
- m01: QED = 0.3187 (fails QED >= 0.6)
- m02: QED = 0.6046 ✓ (passes all constraints)
- m03: QED = 0.3282 (fails)
- m06: QED = 0.4011 (fails)
- m07: QED = 0.3233 (fails)
- m09: QED = 0.5826 (fails, just below 0.6)
- m10: QED = 0.3724 (fails)

**Selected lead: m02**
- QED = 0.6046 >= 0.6 ✓
- psa = 72.24 >= 40 ✓
- Not in m05's series (nitrobenzamide scaffold, not methoxybenzamide) ✓

## Final Answer

- **target_qed**: 0.419
- **lead_mol_id**: "m02"
- **lead_qed_measured**: 0.6046
- **congener_ids**: ["m15", "m17", "m22", "m24", "m28"]
