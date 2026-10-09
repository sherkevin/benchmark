# Notes

## Task 1: Predict QED of m05

### Strategy
m05 (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so I need to predict its QED from its congeneric series (same scaffold, varying N-alkyl chain length).

### Scaffold grouping
m05 belongs to the scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11. Other members:
- m15 (chain 2), m24 (chain 3), m28 (chain 4), m17 (chain 5), m22 (chain 6)

### Measurements (5 of 12 budget used)
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Trend fitting
- **Degree-2 polynomial on all 5 points** (chosen as primary model):
  - Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
  - RMSE: 0.01188
  - **Prediction at chain_len=11: 0.5416**

- Alternative fits for comparison:
  - Linear on chains 3-6: predicts 0.6508
  - Linear on chains 4-6: predicts 0.7041
  - Quadratic on chains 3-6: predicts 0.9378 (unreliable — curves upward)

The degree-2 fit on all 5 points is the most principled: it captures the initial rise (chain 2→3) and subsequent decline, with good RMSE. The predicted decline at long chain lengths is physically reasonable given m05's high alogp (4.956) and 12 rotatable bonds.

### Prediction: **QED(m05) ≈ 0.5416**

---

## Task 2: Pick a lead molecule

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's congeneric series (scaffold `COc1ccc(cc1)C(=O)N`)

### PSA filter
- Scaffold `COc1ccc(cc1)C(=O)N` (m05's): PSA = 38.33 → excluded by constraint 3
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`: PSA = 72.24 ✓
- Scaffold `OCC(O)C(O)C(O)C(=O)N`: PSA = 110.02 ✓
- Scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: PSA = 29.1 ✗ (fails PSA ≥ 40)

### Measurements from candidate series

**Nitro-benzamide series (PSA=72.24):**
| mol_id | chain_len | QED    | QED ≥ 0.6? |
|--------|-----------|--------|------------|
| m09    | 2         | 0.5826 | ✗          |
| m02    | 3         | 0.6046 | ✓          |
| m12    | 4         | 0.4709 | ✗          |
| m29    | 7         | 0.4446 | ✗          |

**Sugar-amide series (PSA=110.02):**
| mol_id | chain_len | QED    | QED ≥ 0.6? |
|--------|-----------|--------|------------|
| m14    | 2         | 0.3237 | ✗          |
| m01    | 4         | 0.3187 | ✗          |
| m03    | 6         | 0.3282 | ✗          |

### Selection
**m02** is the only molecule satisfying all three constraints:
- Measured QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Different scaffold from m05 ✓

---

## Budget usage
Total measurements: 12 of 12 (all used)
- Task 1 series: m15, m24, m28, m17, m22 (5)
- Task 2 candidates: m09, m02, m12, m29, m14, m01, m03 (7)
