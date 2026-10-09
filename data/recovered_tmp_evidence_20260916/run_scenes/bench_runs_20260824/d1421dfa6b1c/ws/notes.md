# Notes: QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Step 1: Identify the congeneric series of m05
Target m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` and chain length 11.

Using `describe_structure`, I identified all pool molecules sharing the same scaffold:
- m15: chain_len=2
- m24: chain_len=3
- m28: chain_len=4
- m17: chain_len=5
- m22: chain_len=6

### Step 2: Measure QED of congeners
Measured QED for all 5 congeners:
- m15 (chain 2): QED = 0.7621
- m24 (chain 3): QED = 0.7919
- m28 (chain 4): QED = 0.7518
- m17 (chain 5): QED = 0.7498
- m22 (chain 6): QED = 0.7377

### Step 3: Fit trend and extrapolate
The data shows QED peaks at chain length 3, then decreases. I fit a quadratic polynomial (degree=2) through the 5 measured points:

- Quadratic fit: coefficients [0.74932, 0.017024, -0.003264] (low to high)
- RMSE: 0.01188
- Prediction at chain_len=11: **0.5416**

I also tried linear fit (degree=1) which gave 0.695 with RMSE 0.01308. The quadratic has better fit (lower RMSE), so I use the quadratic prediction.

**Predicted QED for m05: 0.5416**

---

## Job 2: Select backup lead compound

### Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in the same series as m05 (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### Step 1: Screen by psa ≥ 40
Used `screen_candidates()` — 18 molecules passed.

Note: All m05-series molecules (m15, m17, m22, m24, m28) have psa=38.33 < 40, so none pass this screen. All 18 passing candidates automatically satisfy constraint 3.

### Step 2: Measure QED of candidates
With 3 measurements remaining (budget 14/17 used), I measured:
- m12: QED = 0.4709 (fails constraint 1)
- m02: QED = 0.6046 ✓ (passes all constraints)
- m09: QED = 0.5826 (fails constraint 1)

### Selected lead: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05 scaffold ✓

---

## Summary
- **target_qed**: 0.5416
- **lead_mol_id**: m02
- **lead_qed_measured**: 0.6046
- **congener_ids**: [m15, m24, m28, m17, m22]
