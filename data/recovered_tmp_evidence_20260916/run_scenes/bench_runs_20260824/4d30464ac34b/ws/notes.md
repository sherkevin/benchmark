# Notes

## Task 1: Predict QED of m05

### Congeneric series identification
Used `group_by_scaffold()` (free) to identify m05's series:
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Members: m15 (chain 2), m24 (chain 3), m28 (chain 4), m17 (chain 5), m22 (chain 6), m05 (chain 11)

### QED measurements for series members (5 budget units)
| mol_id | chain_len | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |
| m05    | 11        | ???    |

Pattern: QED peaks at chain 3, then gradually declines.

### Trend fitting (free)
- **Degree 2, all 5 points**: prediction = 0.5416, RMSE = 0.01188 ← **chosen**
- Degree 1, all 5 points: prediction = 0.695, RMSE = 0.01308
- Degree 1, chains 3-6: prediction = 0.6508, RMSE = 0.00883
- Degree 2, chains 3-6: prediction = 0.9378 (unphysical — curves upward)
- Degree 3, all 5 points: prediction = 2.1323 (unphysical — overfitting)

**Chose degree 2 on all 5 points** because:
1. Captures the inverted-U shape (rise from chain 2→3, then decline)
2. Best RMSE among physically reasonable models
3. Consistent with QED component analysis:
   - ROTB desirability drops sharply (0.99→0.53 from chain 2→6, rotb=12 at chain 11 would be very low)
   - ALOGP desirability peaks at chain 4-5 then declines (alogp=4.956 at chain 11 is too lipophilic)

**Predicted QED for m05 = 0.5416**

### QED component verification
Checked components for m15 (chain 2) and m22 (chain 6):
- m15: MW=0.33, ALOGP=0.87, ROTB=0.99, ALERTS=0.64
- m22: MW=0.68, ALOGP=1.00, ROTB=0.53, ALERTS=0.64
- Key trend: MW and ALOGP improve with chain length, but ROTB drops sharply
- At chain 11 (rotb=12, alogp=4.96): ROTB and ALOGP desirabilities would be very low, dragging QED down significantly

## Task 2: Find lead molecule

### Constraints
1. QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's congeneric series (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### Candidate screening by PSA
- Scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl): PSA = 29.1 → **all fail PSA ≥ 40**
- Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide): PSA = 72.24 ✓
- Scaffold `OCC(O)C(O)C(O)C(=O)N` (sugar-amide): PSA = 110.02 ✓

### QED measurements for Task 2 candidates
| mol_id | series | chain | PSA     | QED    | Pass? |
|--------|--------|-------|---------|--------|-------|
| m09    | nitro  | 2     | 72.24   | 0.5826 | No (QED < 0.6) |
| m02    | nitro  | 3     | 72.24   | 0.6046 | **Yes** |
| m12    | nitro  | 4     | 72.24   | 0.4709 | No (QED < 0.6) |
| m01    | sugar  | 4     | 110.02  | 0.3187 | No (QED < 0.6) |
| m32    | nitro  | 5     | 72.24   | 0.4683 | No (QED < 0.6) |

### Why m02 is the best pick
- Nitro series has ALERTS=0.2414 (structural alert for nitro group), which limits QED
- m02 (chain 3) hits the sweet spot: good MW (208), good ALOGP (1.74), good ROTB (4)
- Shorter chains in nitro series (m09, chain 2) have worse MW desirability
- Longer chains (m12, m32) have worse ROTB desirability
- Sugar series has very negative ALOGP values, dragging QED well below 0.6

### QED components for m02
MW=0.50, ALOGP=0.92, HBA=0.98, HBD=0.99, PSA=0.93, ROTB=0.97, AROM=0.83, ALERTS=0.24
The ALERTS score (0.24) is the main drag on QED.

**Selected lead: m02 with measured QED = 0.6046**

## Budget usage
- list_pool: 7 pages = 7 units
- measure_qed: 10 measurements = 10 units (m15, m24, m28, m17, m22, m09, m02, m12, m01, m32)
- Total: 17/17 units used
