# Notes — QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Target molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11
- Never synthesized → `measure_qed` returns `probe_unavailable`

### Congeneric series identification
Using `describe_structure`, I identified all pool molecules sharing scaffold `COc1ccc(cc1)C(=O)N`:

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC |
| m05 | 11 | (target, unsynthesized) |

### Measured QED values
| mol_id | chain_len | QED |
|--------|-----------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend
QED rises from chain 2 to chain 3 (peak at 0.7919), then steadily declines through chains 4–6. This is clearly non-linear (rise-then-fall).

### Extrapolation (degree-2 polynomial)
Fit: QED(x) = 0.74932 + 0.017024·x − 0.003264·x²  
RMSE on fitted points: 0.01188  

Predicted QED at chain_len = 11: **0.5416**

For comparison, a linear fit gives 0.695 — but the data clearly shows curvature (peak at chain ~3), and the task says not to assume a straight line. The quadratic captures the physical intuition that very long alkyl chains degrade drug-likeness.

---

## Job 2: Pick a backup lead

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in m05's series (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### Screening
- Used `screen_candidates()` → 18 molecules pass PSA ≥ 40
- All are from the sugar series (PSA=110.02) or nitro series (PSA=72.24)
- None are in m05's series (which has PSA=38.33)
- Constraint 3 is automatically satisfied for all PSA-passing candidates

### Prioritization
The sugar series has very negative alogp (−2.8 to +0.3) and very high PSA (110), likely penalizing QED. The nitro series has moderate alogp (1.3–4.5) and PSA (72.24), more favorable for QED.

Within the nitro series, short-chain members have the best drug-like properties:
- m09 (chain 2): MW=194, alogp=1.3, rotb=3
- m02 (chain 3): MW=208, alogp=1.7, rotb=4
- m12 (chain 4): MW=222, alogp=2.1, rotb=5

### Measurements
- **m02**: QED = 0.6046 ✓ (passes all 3 constraints)
- m12: QED = 0.4709 ✗ (fails QED ≥ 0.6)

### Chosen lead: **m02**
- Measured QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Series: nitro (`O=[N+]([O-])c1ccc(cc1)C(=O)N`), NOT m05's series ✓

---

## Budget usage
- list_pool pages: 9 (pages 1–9)
- measure_qed calls: 7 (m15, m24, m28, m17, m22, m02, m12)
- Total: 16 out of 17
