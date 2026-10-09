# Notes — Medicinal Chemistry Task

## Job 1: Predict QED of m05

### Target molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- N-alkyl chain length: **11 carbons**
- Never synthesized → cannot measure directly

### Congeneric series (same scaffold, chain length varies)
| mol_id | chain_len | QED (measured) |
|--------|-----------|----------------|
| m15    | 2         | 0.7621         |
| m24    | 3         | 0.7919         |
| m28    | 4         | 0.7518         |
| m17    | 5         | 0.7498         |
| m22    | 6         | 0.7377         |

### Observed trend
QED rises slightly from chain 2→3 (peak at 0.7919), then steadily declines through chains 4–6. The relationship is **not linear** — it has a peak then a downward curve.

### Fitted model (degree-2 polynomial)
```
QED(x) = 0.74932 + 0.017024·x − 0.003264·x²
```
- RMSE on training points: 0.01188
- This was the best fit: degree-1 (linear) had RMSE 0.01308 and gave 0.695; degree-3 overfit (RMSE 0.008 but predicted QED=2.13 at x=11, clearly unphysical).

### Extrapolation to chain_len = 11
**Predicted QED for m05 = 0.5416**

This is an extrapolation well beyond the measured range (max chain = 6), so the prediction carries uncertainty. The quadratic model predicts a continued decline in QED as the alkyl chain grows, consistent with the increasing lipophilicity (alogp = 4.956) penalizing drug-likeness.

---

## Job 2: Pick a backup lead compound

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check from pool.tsv)
3. NOT in m05's series (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### Screening (free)
18 molecules passed PSA ≥ 40. After excluding m05's series (m15, m17, m22, m24, m28), candidates fell into two other series:

**Nitrobenzene series** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`, PSA=72.24):
m02, m06, m09, m10, m12, m29, m31, m32, m33

**Sugar-polyol series** (scaffold `OCC(O)C(O)C(O)C(=O)N`, PSA=110.02):
m01, m03, m07, m14, m18, m19, m21, m25, m27

### Measurements (7 remaining budget)

**Nitrobenzene series:**
| mol_id | chain | QED    | Pass? |
|--------|-------|--------|-------|
| m09    | 2     | 0.5826 | No    |
| m02    | 3     | 0.6046 | **Yes** |
| m12    | 4     | 0.4709 | No    |
| m32    | 5     | 0.4683 | No    |

**Sugar-polyol series:**
| mol_id | chain | QED    | Pass? |
|--------|-------|--------|-------|
| m14    | 2     | 0.3237 | No    |
| m27    | 3     | 0.3330 | No    |
| m01    | 4     | 0.3187 | No    |

### Decision
**m02** is the only molecule meeting all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Not in m05's series (nitrobenzene scaffold) ✓

The sugar-polyol series has uniformly low QED (~0.32) due to high PSA and many H-bond donors/acceptors. The nitrobenzene series shows a QED peak around chain length 3 (m02), with QED dropping for longer chains.

---

## Budget usage
- Job 1: 5 measurements (m15, m24, m28, m17, m22)
- Job 2: 7 measurements (m09, m02, m12, m32, m14, m27, m01)
- Total: 12/12
