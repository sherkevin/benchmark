# Notes

## Identifying Congeneric Series

### m05 series (methoxybenzamide scaffold: `COc1ccc(cc1)C(=O)N-`)
- m15: NCC (2 carbons), MW=179.22, alogp=1.445, psa=38.33
- m24: NCCC (3 carbons), MW=193.25, alogp=1.835, psa=38.33
- m28: NCCCC (4 carbons), MW=207.27, alogp=2.225, psa=38.33
- m17: NCCCCC (5 carbons), MW=221.30, alogp=2.615, psa=38.33
- m22: NCCCCCC (6 carbons), MW=235.33, alogp=3.005, psa=38.33
- **m05 (TARGET)**: NCCCCCCCCCCC (11 carbons), MW=305.46, alogp=4.956, psa=38.33

### Other series identified
- Nitrobenzamide (`O=[N+]([O-])c1ccc(cc1)C(=O)N-`): m09, m02, m12, m32, m33, m29, m31, m06, m10
- Gluconamide (`OCC(O)C(O)C(O)C(=O)N-`): m14, m27, m01, m19, m03, m21, m18, m07, m25
- Biphenyl (`c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): m13, m11, m04, m30, m23, m20, m26, m08, m16

## All 12 Measurements

| # | mol_id | Series | Chain | QED |
|---|--------|--------|-------|------|
| 1 | m15 | methoxybenzamide | 2C | 0.7621 |
| 2 | m24 | methoxybenzamide | 3C | 0.7919 |
| 3 | m28 | methoxybenzamide | 4C | 0.7518 |
| 4 | m17 | methoxybenzamide | 5C | 0.7498 |
| 5 | m22 | methoxybenzamide | 6C | 0.7377 |
| 6 | m09 | nitrobenzamide | 2C | 0.5826 |
| 7 | m02 | nitrobenzamide | 3C | 0.6046 |
| 8 | m12 | nitrobenzamide | 4C | 0.4709 |
| 9 | m01 | gluconamide | 4C | 0.3187 |
| 10 | m14 | gluconamide | 2C | 0.3237 |
| 11 | m33 | nitrobenzamide | 6C | 0.4591 |
| 12 | m20 | biphenyl | 7C | 0.6780 |

m05 → probe_unavailable (confirmed, no cost)

## Job 1: m05 QED Prediction

### Observed trend (methoxybenzamide series)
Chain length vs QED:
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518
- 5C: 0.7498
- 6C: 0.7377

QED peaks at chain length 3, then declines gradually. The decline rates per CH2 are:
- 3→4: −0.040
- 4→5: −0.002
- 5→6: −0.012

### Fitted model: quadratic in chain length n

Using least-squares fit of QED = α + β·u + γ·u² where u = n − 4:

- α = 0.76519
- β = −0.00909
- γ = −0.003264

Equivalently: **QED(n) = 0.74933 + 0.01702·n − 0.003264·n²**

Residuals at data points: +0.008, −0.021, +0.013, +0.003, −0.004 (RMS ≈ 0.012)

### Extrapolation to n = 11 (m05)
QED(11) = 0.74933 + 0.01702(11) − 0.003264(121) = 0.74933 + 0.18722 − 0.39494 = **0.542**

### Why quadratic?
- Task explicitly says not to assume linearity
- The quadratic captures the peak (around n ≈ 2.6) and the gradual post-peak decline
- Physical rationale: logP increases linearly (+0.39/CH2), but QED penalty for high logP is nonlinear (accelerating), so QED decline should accelerate with chain length
- At n=11, logP=4.956 (near Lipinski limit of 5) and rotb=12 (high), both penalizing QED heavily
- Sanity check: biphenyl m20 (logP=5.054, different scaffold) has QED=0.678, confirming QED in the 0.5–0.7 range is plausible at logP≈5

## Job 2: Lead Compound Selection

### Pre-filtering (free):
- Biphenyl series: PSA = 29.1 < 40 → all excluded
- Same series as m05 → excluded
- Remaining candidates: nitrobenzamide (PSA=72.24) and gluconamide (PSA=110.02)

### Measurements on candidates:
- m09 (nitro, 2C): QED = 0.5826 < 0.6 → fails
- m02 (nitro, 3C): QED = 0.6046 ≥ 0.6 → **PASSES all 3 criteria** ✓
- m12 (nitro, 4C): QED = 0.4709 < 0.6 → fails
- m01 (gluco, 4C): QED = 0.3187 < 0.6 → fails
- m14 (gluco, 2C): QED = 0.3237 < 0.6 → fails
- m33 (nitro, 6C): QED = 0.4591 < 0.6 → fails

### Conclusion:
**m02** is the lead compound:
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Nitrobenzamide series (not m05 series) ✓

The nitrobenzamide series peaks at 3C (m02), with shorter (m09, 2C) and longer chains (m12, 4C+) all having lower QED. The gluconamide series has consistently low QED (~0.32), never reaching 0.6.

## Congener IDs (m05 series)
m15, m24, m28, m17, m22
