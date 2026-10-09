# Notes

## Task 1: Predict QED of m05

### Congeneric series of m05
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — a methoxybenzamide with an 11-carbon N-alkyl chain.
Scaffold: `COc1ccc(cc1)C(=O)N-`

Series members (differ only in N-alkyl chain length):
| mol_id | chain | mw     | alogp | psa    | rotb | QED (measured) |
|--------|-------|--------|-------|--------|------|-----------------|
| m15    | 2C    | 179.22 | 1.445 | 38.33  | 3    | 0.7621          |
| m24    | 3C    | 193.25 | 1.835 | 38.33  | 4    | 0.7919          |
| m28    | 4C    | 207.27 | 2.225 | 38.33  | 5    | 0.7518          |
| m17    | 5C    | 221.30 | 2.615 | 38.33  | 6    | 0.7498          |
| m22    | 6C    | 235.33 | 3.005 | 38.33  | 7    | 0.7377          |
| m05    | 11C   | 305.46 | 4.956 | 38.33  | 12   | ??? (predicted) |

### Trend analysis
- QED peaks at 3C (0.7919) then declines monotonically
- From 3C to 6C: decline rate = (0.7377 - 0.7919)/(6-3) = -0.0181 per CH2
- Linear regression (all 5 points): slope = -0.0091/CH2, predicting QED ~ 0.70 at 11C
- Linear regression (3C-6C only): slope = -0.0181/CH2, predicting QED ~ 0.65 at 11C
- Log-linear ratio method: geometric mean decay = 0.9766/CH2, predicting QED ~ 0.66 at 11C

### Prediction
The decline accelerates as alogp and rotb increase. Using the 3C-6C trend as most reliable for extrapolation:
**Predicted QED for m05 ≈ 0.65**

## Task 2: Find a lead molecule

### Constraints
1. Measured QED >= 0.6
2. PSA >= 40
3. Not in m05 congeneric series

### Candidates measured
Biphenyl series: all have PSA=29.1 (fail constraint 2)
m05 series: all have PSA=38.33 (fail constraint 2)

Nitrobenzamide series (PSA=72.24):
- m09 (2C): QED=0.5826 — fails (QED < 0.6)
- m02 (3C): QED=0.6046 — PASSES all constraints
- m12 (4C): QED=0.4709 — fails
- m32 (5C): QED=0.4683 — fails
- m33 (6C): QED=0.4591 — fails
- m29 (7C): QED=0.4446 — fails

Sugar alcohol series (PSA=110.02):
- m01: QED=0.3187 — fails (QED too low)

### Selected lead: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 >= 0.6 (check)
- PSA: 72.24 >= 40 (check)
- Nitrobenzamide series, NOT in m05 methoxybenzamide series (check)

## Budget
- 5 measurements for m05 series (m15, m24, m28, m17, m22)
- 6 measurements for Task 2 candidates (m09, m02, m12, m32, m33, m29)
- 1 measurement for sugar alcohol check (m01)
- 1 free probe (m05 = probe_unavailable)
- Total: 12/12 used
