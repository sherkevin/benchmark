# Notes for Medicinal Chemistry Task

## Initial Analysis
Target molecule m05 has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
This appears to be a methoxybenzene scaffold with an amide linker and a long alkyl chain.

## Step 1: Identify congeneric series for m05
Using describe_structure tool:
- m05: scaffold `COc1ccc(cc1)C(=O)N`, chain_len = 11
- m15: same scaffold, chain_len = 2
- m17: same scaffold, chain_len = 5  
- m22: same scaffold, chain_len = 6
- m24: same scaffold, chain_len = 3
- m28: same scaffold, chain_len = 4

Congeneric series for m05: [m15, m17, m22, m24, m28]

All these have shorter chain lengths than m05 (chain_len 11), so we'll be extrapolating beyond measured range.

## Step十个: Plan QED measurements
We have 12 measurement budget total for both jobs. Need to:
1. Measure QED for congeneric series members to establish trend
2. Find a lead compound with QED ≥ 0.6, psa ≥ 40, and not in m05's series

## PSA Screening Results
Using screen_candidates(): 18 molecules pass psa ≥ 40 constraint:
- Polyol series (scaffold OCC(O)C(O)C(O)C(=O)N): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrobenzene series (scaffold O=[N+]([O-])c1ccc(cc1)C(=O)N): m02, m06, m09, m10, m12, m29, m31, m32, m33

None are in m05's methoxybenzene series, so all satisfy constraint 3.

## Measurement Strategy
Budget: 12 total measurements
Need to measure congeneric series (5 molecules: m15, m17, m22, m24, m28) to predict m05 QED = 5 measurements
Then need to find lead with QED ≥ 0.6 from remaining 7 measurements.

Smart approach: Measure congeneric series first, then test promising candidates from other series.

## Congeneric Series QED Measurements
Measured all 5 congeneric series members (5/12 budget used):
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919  
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Pattern: QED peaks at chain_len=3 (0.7919) then decreases with longer chains.

## Trend Fitting for m05 Prediction
Data: xs=[2,3,4,5,6], ys=[0.7621,0.7919,0.7518,0.7498,0.7377]
Quadratic fit (degree=2): predicts QED = 0.5416 for chain_len=11 (m05)
Linear fit (degree=1): predicts QED = 0.695 for chain_len=11
Quadratic has lower RMSE (0.01188 vs 0.01308) and seems more plausible given declining trend.

Choosing quadratic prediction: m05 QED ≈ 0.5416

## Lead Compound Search
Remaining budget: 7 measurements for lead compound search.

### Polyol Series Measurements (low QED):
- m14 (chain_len=2): QED = 0.3237 < 0.6
- m27 (chain_len=3): QED = 0.333 < 0.6
Polyol series seems to have low QED values.

### Nitrobenzene Series Measurements:
- m09 (chain_len=2): QED = 0.5826 < 0.6
- m02 (chain_len=3): QED = 0.6046 ≥ 0.6 ✓
- m12 (chain_len=4): QED = 0.4709 < 0.6
- m32 (chain_len=5): QED = 0.4683 < 0.6

Pattern in nitrobenzene series: QED peaks around chain_len=3, similar to methoxybenzene series.

Found valid lead: m02 satisfies all constraints:
1. QED ≥ 0.6 ✓ (0.6046)
2. psa ≥ 40 ✓ (72.24)  
3. Not in m05's series ✓ (nitrobenzene series)

This is the best candidate found among tested compounds. Remaining budget: 1 measurement (unused).

## Final Answer
- Predicted m05 QED: 0.5416 (quadratic extrapolation from congeneric series measurements)
- Lead compound: m02 with measured QED 0.6046
- Congeneric series for m05: m15, m17, m22, m24, m28

All measurements used: 11/12 budget.