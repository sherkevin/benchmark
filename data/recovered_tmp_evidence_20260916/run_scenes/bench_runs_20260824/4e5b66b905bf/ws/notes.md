# Task Analysis Notes

## Understanding the Task

We have two jobs:
1. Predict QED for m05 (target molecule) which has scaffold `COc1ccc(cc1)C(=O)N` and chain length 11
2. Pick a lead compound that satisfies:
   - measured QED ≥ 0.6
   - psa ≥ 40 (free from data)
   - not in same series as m05

We have 12 total measurement budget to use across both jobs.

## Initial Observations

Target molecule m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
Chain length: 11 carbons

## Identifying Congeners of m05

Using `describe_structure`, I have identified all molecules in the pool that share the same scaffold as m05:

1. m15: COc1ccc(cc1)C(=O)NCC - scaffold: `COc1ccc(cc1)C(=O)N`, chain_len: 2
2. m24: COc1ccc(cc1)C(=O)NCCC - scaffold: `COc1ccc(cc1)C(=O)N`, chain_len: 3
3. m28: COc1ccc(cc1)C(=O)NCCCC - scaffold: `COc1ccc(cc1)C(=O)N`, chain_len: 4
4. m17: COc1ccc(cc1)C(=O)NCCCCC - scaffold: `COc1ccc(cc1)C(=O)N`, chain_len: 5
5. m22: COc1ccc(cc1)C(=O)NCCCCCC - scaffold: `COc1ccc(cc1)C(=O)N`, chain_len: 6

These 5 molecules are all in the same congeneric series as m05. m05 has chain length 11, which is longer than any of these pool members, so we need to extrapolate beyond the measured range.

## Job 1: Predicting m05 QED

I have measured QED for all 5 congeners of m05:

| mol_id | chain_len | QED measured |
|--------|-----------|--------------|
| m15    | 2         | 0.7621       |
| m24    | 3         | 0.7919       |
| m28    | 4         | 0.7518       |
| m17    | 5         | 0.7498       |
| m22    | 6         | 0.7377       |

Chain lengths: [2, 3, 4, 5, 6]
QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

### Trend Fitting

Fitting polynomial of degree 2 (quadratic) as suggested (not assuming straight line):
- Prediction at chain length 11: **0.5416**
- Coefficients: [0.74932, 0.017024, -0.003264]
- RMSE: 0.01188

For comparison:
- Degree 1 (linear): prediction 0.6950
- Degree点数3: prediction 2.1323 (nonsensical, >1)

Degree 2 seems most reasonable given the clear curvature in the data (peak at chain length 3 then decrease). The prediction 0.5416 is plausible as continued downward trend.

**Predicted QED for m05: 0.5416**

## Job 2: Finding Lead Compound

From `screen_candidates()`, 18 molecules have psa ≥ 40:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

None of these are in m05's series (m15, m24, m28, m17, m22), so all 18 satisfy constraint 3.

Now need to measure QED for candidates to find one with QED ≥ 0.6.

Total budget: 12 measurements total, 5 already used, **7 remaining**.

Two main scaffold families in candidates:
1. Hydroxy-rich: OCC(O)C(O)C(O)C(=O)N... (m01, m03, m07, m14, m18, m19, m21, m25, m27)
2. Nitro series: O=[N+]([O-])c1ccc(cc1)C(=O)N... (m02, m06, m09, m10, m12, m29, m31, m32, m33)

### QED Measurements for Lead Candidates

Used remaining 7 measurements:

1. m01 (hydroxy-rich): **0.3187** (< 0.6)
2. m02 (nitro series): **0.6046** (≥ 0.6) ✓
3. m06 (nitro series): **0.4011** (< 0.6)
4. m09 (nitro series): **0.5826** (< 0.6)
5. m03 (hydroxy-rich): **0.3282** (< 0.6)
6. m10 (nitro series): **0.3724** (< 0.6)
7. m12 (nitro series): **0.4709** (< 0.6)

### Lead Compound Selection

**Selected lead: m02**
- QED: 0.6046 (≥ 0.6, measured)
- psa: 72.24 (≥ 40, from pool.tsv)
- Not in m05 series: scaffold is `O=[N+]([O-])c1ccc(cc1)C(=O)N`, different from m05's scaffold `COc1ccc(cc1)C(=O)N`

m02 satisfies all three constraints and has the highest QED among measured candidates.

## Summary

### Job 1: Predicting m05 QED
- Identified 5 congeners of m05: m15, m24, m28, m17, m22
- Measured their QED values: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- Fitted quadratic trend (degree 2) to chain lengths [2, 3, 4, 5, 6]
- Extrapolated to chain length 11 (m05): **0.5416**

### Job 2: Selecting Lead Compound
- 18 candidates with psa ≥ 40 from screen_candidates()
- None are in m05 series
- Measured 7 candidates from different scaffolds
- Found m02 has QED 0.6046 (≥ 0.6)
- m02 satisfies all constraints:
  1. Measured QED = 0.6046 ≥ 0.6
  2. psa = 72.24 ≥ 40
  3. Not in m05 series (different scaffold)

### Measurements Used
Total 12 measurements:
- 5 for m05 series congeners
- 7 for lead candidate screening

All budget used efficiently.