# Notes — Medicinal Chemistry Task

## Job 1: Predict QED of m05

### Identifying the congeneric series of m05

m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` with scaffold `COc1ccc(cc1)C(=O)N` (para-methoxybenzamide) and N-alkyl chain length 11.

Pool molecules sharing this scaffold (confirmed via `describe_structure`):
- m15: chain_len=2, QED=0.7621
- m24: chain_len=3, QED=0.7919
- m28: chain_len=4, QED=0.7518
- m17: chain_len=5, QED=0.7498
- m22: chain_len=6, QED=0.7377

**congener_ids: m15, m24, m28, m17, m22**

### Observed trend

QED rises from chain 2→3 (0.7621→0.7919), then steadily declines through chains 4, 5, 6 (0.7518, 0.7498, 0.7377). The relationship is clearly non-linear — there is a peak near chain 3 followed by a monotonic decline.

### Fitting and extrapolation

**Quadratic fit (degree=2)** using all 5 measured points:
- Polynomial: QED(x) = 0.74932 + 0.017024·x − 0.003264·x²
- RMSE on fitted points: 0.01188
- Predicted QED at chain 11: **0.5416**

For comparison, a linear fit (degree=1) gives:
- QED(x) = 0.79502 − 0.00909·x
- RMSE: 0.01308
- Predicted QED at chain 11: 0.6950

The quadratic has lower RMSE and captures the observed curvature (peak then decline). The task explicitly warns against assuming linearity. The quadratic prediction is the more faithful extrapolation of the observed trend.

**Predicted QED for m05: 0.5416**

---

## Job 2: Pick a backup lead

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free check)
3. Not in m05's series (free check)

### Screening

Used `screen_candidates()` to get all molecules with PSA ≥ 40, then excluded the 5 members of m05's series. This left candidates from two other series:
- **Nitrobenzene series** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`, PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- **Sugar/polyol series** (scaffold `OCC(O)C(O)C(O)C(=O)N`, PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

### Measurements (remaining budget: 7)

Prioritized the nitrobenzene series (moderate PSA, reasonable logP values):
- m12 (chain 4, alogp=2.125): QED = 0.4709 ✗
- m32 (chain 5, alogp=2.515): QED = 0.4683 ✗
- m33 (chain 6, alogp=2.905): QED = 0.4591 ✗
- **m02 (chain 3, alogp=1.735): QED = 0.6046 ✓**

### Lead selection

**m02** satisfies all three constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzene series, different from m05's methoxybenzamide) ✓

**Lead: m02, measured QED = 0.6046**

### Budget summary
- Job 1: 5 measurements (m15, m24, m28, m17, m22)
- Job 2: 4 measurements (m12, m32, m33, m02)
- Total: 9 of 12 used
