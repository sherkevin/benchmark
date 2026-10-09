# Notes — Medicinal Chemistry Task

## Job 1: Predict QED of m05

### Identifying the congeneric series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- N-alkyl chain length: 11 carbons

Pool molecules sharing the same scaffold (confirmed via `describe_structure`):

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC |

**Congener IDs: m15, m24, m28, m17, m22**

### Measured QED values

| mol_id | chain_len (x) | QED (y) |
|--------|---------------|---------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend

QED peaks at chain length 3 (0.7919) then decreases monotonically from C4 onward. The initial rise from C2 to C3 followed by consistent decline creates a concave-down (inverted-U) pattern, well captured by a degree-2 polynomial.

### Fitted trend (degree-2 polynomial, all 5 points)

Using `fit_trend` with degree=2:

**QED(x) = 0.74932 + 0.017024*x - 0.003264*x^2**

- RMSE on fitted points: 0.01188 (good fit)
- The negative quadratic coefficient (-0.003264) reflects increasing lipophilicity penalizing QED at longer chain lengths.

### Extrapolation to m05 (chain_len = 11)

QED(11) = 0.74932 + 0.017024*11 - 0.003264*121
        = 0.74932 + 0.18726 - 0.39494
        = **0.5416**

### Cross-check with other fits

- Degree-1 (linear) on all 5 points: QED(11) = 0.695 — likely overestimates because it ignores the curvature
- Degree-2 on C3-C6 only: QED(11) = 0.9378 — unreliable, positive x^2 coefficient causes upward curve; small subset fit is unstable for extrapolation

The degree-2 fit on all 5 data points is the most chemically reasonable: it captures the concave-down shape, has good RMSE, and the extrapolated decline is consistent with growing lipophilicity penalty.

**Predicted QED for m05: 0.5416**

---

## Job 2: Pick a backup lead compound

### Constraints
1. Measured QED >= 0.6
2. PSA >= 40 (free check from pool.tsv)
3. Not in Series D (m05's series)

### Pre-screening (free)

PSA >= 40 eliminates all Series C molecules (PSA = 29.1). Series D molecules (PSA = 38.33) also excluded. Remaining candidates:

- **Series A** (sugar amide, PSA = 110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- **Series B** (nitrobenzamide, PSA = 72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements (7 remaining budget)

| mol_id | Series | chain_len | QED | Pass? |
|--------|--------|-----------|-----|-------|
| m12 | B | 4 | 0.4709 | No (QED < 0.6) |
| m32 | B | 5 | 0.4683 | No (QED < 0.6) |
| m09 | B | 2 | 0.5826 | No (QED < 0.6) |
| m02 | B | 3 | **0.6046** | **YES** |
| m33 | B | 6 | 0.4591 | No (QED < 0.6) |
| m01 | A | 4 | 0.3187 | No (QED < 0.6) |
| m29 | B | 7 | 0.4446 | No (QED < 0.6) |

### Result

**m02** is the only measured molecule meeting all three constraints:
- Measured QED = 0.6046 >= 0.6
- PSA = 72.24 >= 40
- Series B (nitrobenzamide, scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`), not Series D

Series A molecules have very low QED (sugar-like scaffold with high PSA and many HBD/HBA drag down drug-likeness). Series B shows QED peaking around short chains (C2-C3), with m02 (C3) being the sweet spot.

**Chosen lead: m02, measured QED = 0.6046**

---

## Budget usage

- 5 measurements for Series D (m15, m24, m28, m17, m22)
- 7 measurements for Job 2 candidates (m12, m32, m09, m02, m33, m01, m29)
- Total: 12/12
