# Notes

## Task 1: Predict QED of m05

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain_len=11) and scaffold `COc1ccc(cc1)C(=O)N`.
It has never been synthesized, so I must predict from its congeneric series.

### Congeneric series measurements (scaffold: COc1ccc(cc1)C(=O)N)

| mol_id | chain_len | QED    | MW      | alogp  | rotb |
|--------|-----------|--------|---------|--------|------|
| m15    | 2         | 0.7621 | 179.22  | 1.445  | 3    |
| m24    | 3         | 0.7919 | 193.25  | 1.835  | 4    |
| m28    | 4         | 0.7518 | 207.27  | 2.225  | 5    |
| m17    | 5         | 0.7498 | 221.30  | 2.615  | 6    |
| m22    | 6         | 0.7377 | 235.33  | 3.005  | 7    |
| m05    | 11        | ???    | 305.46  | 4.956  | 12   |

### QED components analysis

- m15 (chain_len=2): MW=0.33, ALOGP=0.87, ROTB=0.99, ALERTS=0.84
- m22 (chain_len=6): MW=0.68, ALOGP=1.00, ROTB=0.53, ALERTS=0.64

As chain length increases: MW desirability improves (entering sweet spot), but ROTB and ALERTS decline.
For m05 (chain_len=11): alogp=4.956 (borderline high) and rotb=12 (high) will cause significant penalties.

### Trend fitting

- Linear (all 5 points, degree=1): QED=0.695 (RMSE=0.0131)
- Quadratic (all 5 points, degree=2): QED=0.5416 (RMSE=0.0119) — best fit, captures accelerating decline
- Cubic (all 5 points, degree=3): QED=2.13 — overfits, unusable
- Linear (chain 3-6 only): QED=0.6508
- Quadratic (chain 3-6 only): QED=0.9378 — curves upward, unphysical

Chose the quadratic fit with all 5 points (0.5416) because:
1. Best RMSE among physically meaningful fits
2. Negative quadratic coefficient captures accelerating penalties from high alogp and rotb
3. Physically motivated: desirability functions drop sharply for alogp~5 and rotb~12

**Predicted QED for m05: 0.5416**

## Task 2: Find lead molecule

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's congeneric series.

PSA ≥ 40 eliminates Group 4 (biphenyl, PSA=29.1) and Group 1 (m05's scaffold, PSA=38.33, also excluded by rule 3).

Candidates from Group 2 (nitrobenzamide, PSA=72.24) and Group 3 (sugar-amide, PSA=110.02):

| mol_id | scaffold_group | QED    | PSA    | QED≥0.6? |
|--------|---------------|--------|--------|----------|
| m09    | Group 2       | 0.5826 | 72.24  | No       |
| m02    | Group 2       | 0.6046 | 72.24  | Yes ✓    |
| m12    | Group 2       | 0.4709 | 72.24  | No       |
| m32    | Group 2       | 0.4683 | 72.24  | No       |
| m14    | Group 3       | 0.3237 | 110.02 | No       |
| m01    | Group 3       | 0.3187 | 110.02 | No       |
| m19    | Group 3       | 0.3244 | 110.02 | No       |

**Selected lead: m02** (QED=0.6046, PSA=72.24, scaffold=O=[N+]([O-])c1ccc(cc1)C(=O)N)
- It is the only molecule satisfying all three constraints simultaneously.

## Measurement budget

Total measurements used: 12/12
- Group 1: m15, m24, m28, m17, m22 (5 measurements)
- Group 2: m09, m02, m12, m32 (4 measurements)
- Group 3: m14, m01, m19 (3 measurements)
