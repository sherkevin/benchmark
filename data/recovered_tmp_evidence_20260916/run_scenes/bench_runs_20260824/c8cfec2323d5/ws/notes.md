# Notes

## Task 1: Predict QED of m05

### Scaffold identification
Used `group_by_scaffold()` (free) to identify m05's congeneric series:
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Members: m15 (chain 2), m24 (chain 3), m28 (chain 4), m17 (chain 5), m22 (chain 6), m05 (chain 11, unsynthesized)

### Measurements (5 budget units)
| mol_id | chain_len | QED     |
|--------|-----------|---------|
| m15    | 2         | 0.7621  |
| m24    | 3         | 0.7919  |
| m28    | 4         | 0.7518  |
| m17    | 5         | 0.7498  |
| m22    | 6         | 0.7377  |

### QED component analysis (free)
Checked components for m15, m24, m22. As chain length grows:
- MW score increases (good)
- ALOGP score increases slightly
- ROTB score drops sharply (0.99 → 0.53)
- ALERTS score drops (0.84 → 0.64)
- HBA, HBD, PSA, AROM stay constant

### Trend fitting (free)
- **Quadratic (degree 2) on all 5 points → QED = 0.5416** (RMSE = 0.0119) ✓ chosen
- Linear (degree 1) on all 5 points → QED = 0.6950 (RMSE = 0.0131)
- Linear (degree 1) on chains 4–6 only → QED = 0.7041 (RMSE = 0.0024)
- Cubic (degree 3) → QED = 2.13 (diverges, unreliable for extrapolation)

Chose quadratic fit: it captures the accelerating decline from compounding ROTB and ALERTS penalties at long chain lengths. m05 (MW=305, ALOGP=4.956, ROTB=12) has very high lipophilicity and flexibility, consistent with a lower QED.

**Predicted QED for m05: 0.5416**

---

## Task 2: Find a lead molecule

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's congeneric series (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### Candidates measured
| mol_id | scaffold group | PSA     | QED     | Pass? |
|--------|---------------|---------|---------|-------|
| m09    | nitro-aromatic | 72.24  | 0.5826  | No (QED < 0.6) |
| m12    | nitro-aromatic | 72.24  | 0.4709  | No (QED < 0.6) |
| **m02**| nitro-aromatic | 72.24  | **0.6046** | **Yes** |
| m32    | nitro-aromatic | ?      | 0.4683  | No |
| m33    | nitro-aromatic | ?      | 0.4591  | No |
| m14    | sugar-like     | 110.02 | 0.3237  | No (QED too low) |
| m01    | sugar-like     | 110.02 | 0.3187  | No (QED too low) |

m02 satisfies all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

**Lead molecule: m02, measured QED = 0.6046**

---

## Budget usage
- 5 measurements for m05's series: m15, m24, m28, m17, m22
- 4 pages listed: pages 1–4
- 7 additional measurements: m09, m12, m02, m32, m33, m14, m01
- Total: 16 of 17 budget units used
