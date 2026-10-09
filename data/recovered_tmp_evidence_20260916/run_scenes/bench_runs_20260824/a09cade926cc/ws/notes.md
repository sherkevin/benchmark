# Notes — Medicinal Chemistry Task

## Job 1: Predict QED of m05

### Identifying the congeneric series

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. Using `describe_structure`:
- **Scaffold**: `COc1ccc(cc1)C(=O)N` (methoxy-benzamide)
- **Chain length**: 11 carbons

Pool molecules sharing the same scaffold (confirmed via `describe_structure`):

| mol_id | chain_len | SMILES |
|--------|-----------|--------|
| m15 | 2 | COc1ccc(cc1)C(=O)NCC |
| m24 | 3 | COc1ccc(cc1)C(=O)NCCC |
| m28 | 4 | COc1ccc(cc1)C(=O)NCCCC |
| m17 | 5 | COc1ccc(cc1)C(=O)NCCCCC |
| m22 | 6 | COc1ccc(cc1)C(=O)NCCCCCC |

**Congeners**: m15, m24, m28, m17, m22

### Measured QED values

| mol_id | chain_len | QED |
|--------|-----------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Observed trend

QED peaks around chain length 3 (0.7919), then steadily declines:
- 2→3: +0.030 (rise)
- 3→4: −0.040 (sharp drop)
- 4→5: −0.002 (nearly flat)
- 5→6: −0.012 (gentle decline)

The relationship is clearly non-linear — there is a peak followed by a decline.

### Extrapolation

Fitted a **degree-2 polynomial** (quadratic) through all 5 points:

`QED(chain) = 0.74932 + 0.017024 * chain − 0.003264 * chain²`

- RMSE on training points: 0.01188
- Predicted QED at chain = 11: **0.5416**

Comparison with other degrees:
- Degree 1 (linear): predicts 0.695 — but ignores the clear curvature (peak at chain 3)
- Degree 3 (cubic): predicts 2.13 — clearly overfitting (value >1, nonsensical)

The quadratic is the best model: it captures the non-linear trend (peak then decline), has the lowest RMSE among sensible models, and gives a physically meaningful prediction within [0, 1].

**Predicted QED for m05: 0.5416**

---

## Job 2: Pick a backup lead compound

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check)
3. Not in m05's series (free check)

### Pre-filtering (free)

Used `screen_candidates()` — 18 molecules pass PSA ≥ 40. These belong to two series:
- **Sugar series** (scaffold `OCC(O)C(O)C(O)C(=O)N`, PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- **Nitro-benzamide series** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`, PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

Neither series shares m05's scaffold, so all 18 pass constraint 3.

### QED measurements on candidates

Focused on the nitro-benzamide series (moderate PSA, 1 aromatic ring, reasonable logP):

| mol_id | PSA | alogp | QED measured | Passes? |
|--------|-----|-------|-------------|---------|
| m09 | 72.24 | 1.345 | 0.5826 | No (QED < 0.6) |
| m02 | 72.24 | 1.735 | **0.6046** | **Yes** |
| m12 | 72.24 | 2.125 | 0.4709 | No |
| m32 | 72.24 | 2.515 | 0.4683 | No |
| m33 | 72.24 | 2.905 | 0.4591 | No |

### Chosen lead: **m02**

- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: 0.6046 (≥ 0.6 ✓)
- PSA: 72.24 (≥ 40 ✓)
- Series: nitro-benzamide (≠ m05's methoxy-benzamide ✓)

All three constraints satisfied. m02 is the optimal choice — it has the highest measured QED among all candidates tested.

### Measurement budget used: 10 of 12
- 5 for m05's congeners (m15, m24, m28, m17, m22)
- 5 for lead candidates (m09, m02, m12, m32, m33)
