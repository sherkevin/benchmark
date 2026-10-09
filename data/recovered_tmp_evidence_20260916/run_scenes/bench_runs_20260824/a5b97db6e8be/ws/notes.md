# Notes

## Task 1: Predict QED of m05

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide scaffold with 11-carbon N-alkyl chain.
Never synthesized, so `measure_qed` returns `probe_unavailable`. Must predict from congeneric series.

### Congeneric series (same scaffold, varying N-alkyl chain length)

| mol_id | chain_C | alogp | mw      | QED (measured) |
|--------|---------|-------|---------|-----------------|
| m15    | 2       | 1.445 | 179.22  | 0.7621          |
| m24    | 3       | 1.835 | 193.25  | 0.7919          |
| m28    | 4       | 2.225 | 207.27  | 0.7518          |
| m17    | 5       | 2.615 | 221.30  | 0.7498          |
| m22    | 6       | 3.005 | 235.33  | 0.7377          |
| **m05**| **11**  | 4.956 | 305.46  | **(predicted)** |

### Linear regression (chain length vs QED)

- x = [2, 3, 4, 5, 6], y = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- slope = -0.00909, intercept = 0.79502
- QED(11) = 0.79502 - 0.00909 * 11 = **0.695**

Clear downward trend: longer chains -> higher logP, higher MW, more rotatable bonds -> lower QED.

## Task 2: Find lead molecule

### Constraints
1. Measured QED >= 0.6
2. PSA >= 40
3. Not in m05's congeneric series (not a methoxybenzamide)

### Molecules with PSA >= 40 and not in m05's series

- Nitrobenzene series (PSA = 72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Sugar-like series (PSA = 110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

### Measurements (7 remaining budget)

| mol_id | scaffold       | PSA    | QED (measured) | QED >= 0.6? |
|--------|---------------|--------|-----------------|-------------|
| m12    | nitrobenzene  | 72.24  | 0.4709          | No          |
| m33    | nitrobenzene  | 72.24  | 0.4591          | No          |
| m32    | nitrobenzene  | 72.24  | 0.4683          | No          |
| m29    | nitrobenzene  | 72.24  | 0.4446          | No          |
| **m02**| **nitrobenzene** | **72.24** | **0.6046** | **Yes**   |
| m07    | sugar-like    | 110.02 | 0.3233          | No          |
| m21    | sugar-like    | 110.02 | 0.3297          | No          |

### Selected lead: m02

- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 (>= 0.6)
- PSA: 72.24 (>= 40)
- Scaffold: nitrobenzamide - different from m05's methoxybenzamide
- All three constraints satisfied.

## Budget usage
- 5 measurements for m05's congeneric series (m15, m24, m28, m17, m22)
- 7 measurements for Task 2 candidates (m12, m33, m32, m29, m02, m07, m21)
- Total: 12/12
