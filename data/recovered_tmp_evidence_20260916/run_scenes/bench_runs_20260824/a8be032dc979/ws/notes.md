# Medicinal Chemistry Notes

## Data Overview

33 molecules in `data/pool.tsv` organized into 4 congeneric series (same scaffold, varying N-alkyl chain length):

- **Series A** (sugar acid amides, `OCC(O)C(O)C(O)C(=O)N-`): 9 members, PSA=110.02
- **Series B** (nitrobenzamides, `O=[N+]([O-])c1ccc(cc1)C(=O)N-`): 9 members, PSA=72.24
- **Series C** (biphenyl amides, `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): 9 members, PSA=29.1
- **Series D** (methoxybenzamides, `COc1ccc(cc1)C(=O)N-`): 6 members, PSA=38.33 ← m05's series

## Task 1: Predict QED of m05

**m05** = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11-carbon N-alkyl chain)
- MW=305.46, ALogP=4.956, PSA=38.33, RotB=12, Arom=1
- Has never been synthesized → cannot measure directly

### Measured Series D members (5 measurements):

| mol_id | Chain | MW     | ALogP | PSA   | RotB | QED    |
|--------|-------|--------|-------|-------|------|--------|
| m15    | 2C    | 179.22 | 1.445 | 38.33 | 3    | 0.7621 |
| m24    | 3C    | 193.25 | 1.835 | 38.33 | 4    | 0.7919 |
| m28    | 4C    | 207.27 | 2.225 | 38.33 | 5    | 0.7518 |
| m17    | 5C    | 221.30 | 2.615 | 38.33 | 6    | 0.7498 |
| m22    | 6C    | 235.33 | 3.005 | 38.33 | 7    | 0.7377 |

### Trend analysis:
- QED peaks at 3C (0.7919) then declines steadily for longer chains
- Per-carbon changes: 2→3: +0.030, 3→4: -0.040, 4→5: -0.002, 5→6: -0.012
- ALogP increases by ~0.39 per CH2, RotB by 1 per CH2

### Linear regression (all 5 points, chain length vs QED):
- n=5, x̄=4, ȳ=0.75866
- slope = -0.00909 per CH2
- intercept = 0.79502
- **QED(11C) = 0.79502 - 0.00909 × 11 = 0.695**

### Cross-checks with other models:
- Linear on 3C-6C only: ~0.651
- Log model (all 5 pts): ~0.726
- Quadratic (all 5 pts): ~0.542 (overfit — adjusted R² worse than linear)
- Ratio-based extrapolation: ~0.656

The linear regression on all 5 data points is the most robust approach (best adjusted R², uses all data, avoids overfitting).

### **Prediction: QED(m05) = 0.695**

---

## Task 2: Find lead molecule

Constraints: (1) measured QED ≥ 0.6, (2) PSA ≥ 40, (3) not in Series D

### PSA filter:
- Series C (PSA=29.1): **EXCLUDED** (all below 40)
- Series D (PSA=38.33): **EXCLUDED** (same series as m05)
- Series A (PSA=110.02): eligible ✓
- Series B (PSA=72.24): eligible ✓

### Series B measurements (4 measurements):

| mol_id | Chain | ALogP | QED    | QED ≥ 0.6? |
|--------|-------|-------|--------|------------|
| m09    | 2C    | 1.345 | 0.5826 | No         |
| m02    | 3C    | 1.735 | 0.6046 | **YES** ✓  |
| m12    | 4C    | 2.125 | 0.4709 | No         |
| m32    | 5C    | 2.515 | 0.4683 | No         |

### Series A measurements (3 measurements):

| mol_id | Chain | ALogP  | QED    | QED ≥ 0.6? |
|--------|-------|--------|--------|------------|
| m14    | 2C    | -2.802 | 0.3237 | No         |
| m27    | 3C    | -2.412 | 0.3330 | No         |
| m01    | 4C    | -2.022 | 0.3187 | No         |

Series A molecules all have very low QED (~0.32), likely due to extremely negative ALogP values and the polyhydroxylated scaffold.

### **Result: m02 is the lead molecule**

m02 (`O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`) satisfies all three constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Series B (nitrobenzamide), not in same series as m05 ✓

---

## Budget: 12/12 measurements used
- 5 for Series D (Task 1 trend)
- 4 for Series B (Task 2 candidates)
- 3 for Series A (Task 2 candidates)
