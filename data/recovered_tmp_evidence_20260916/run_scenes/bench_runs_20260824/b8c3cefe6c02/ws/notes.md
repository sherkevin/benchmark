# Notes — QED Prediction and Lead Selection

## Budget Usage
- 9 `list_pool` calls (pages 1–9) to catalog all 33 molecules
- 5 `measure_qed` calls for the m05 congeneric series (m15, m24, m28, m17, m22)
- 3 `measure_qed` calls for lead candidates (m09, m02, m12)
- Total: 17/17

## Job 1: Predict QED of m05

### Identifying the Congeneric Series
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxyphenyl amide (`COc1ccc(cc1)C(=O)N-`)
- Variable: N-alkyl chain length (11 carbons for m05)

Pool members with the same scaffold (methoxyphenyl series):
| mol_id | Chain length | SMILES suffix | MW | ALogP | PSA | rotb |
|--------|-------------|---------------|------|-------|------|------|
| m15 | 2 | NCC | 179.22 | 1.445 | 38.33 | 3 |
| m24 | 3 | NCCC | 193.25 | 1.835 | 38.33 | 4 |
| m28 | 4 | NCCCC | 207.27 | 2.225 | 38.33 | 5 |
| m17 | 5 | NCCCCC | 221.30 | 2.615 | 38.33 | 6 |
| m22 | 6 | NCCCCCC | 235.33 | 3.005 | 38.33 | 7 |
| **m05** | **11** | **NCCCCCCCCCCC** | **305.46** | **4.956** | **38.33** | **12** |

All share PSA=38.33 and arom=1, confirming same scaffold.

### Measured QED Trend
| Chain length (n) | mol_id | QED |
|---|---|---|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

**Observed pattern**: QED peaks at C3 (0.7919), then declines. First differences:
- C2→C3: +0.0298
- C3→C4: −0.0401
- C4→C5: −0.0020
- C5→C6: −0.0121

The decline accelerates after the C3 peak, driven by increasing lipophilicity (ALogP rises ~0.39 per CH₂).

### Fitted Model: Least-Squares Quadratic (all 5 data points)
QED(n) = a·n² + b·n + c

Normal equations solved:
- a = −0.003264
- b = +0.017022
- c = 0.749324

**QED(n) = −0.003264·n² + 0.017022·n + 0.749324**

Fit quality (residuals):
- n=2: predicted 0.770 vs actual 0.762 (error +0.008)
- n=3: predicted 0.771 vs actual 0.792 (error −0.021)
- n=4: predicted 0.765 vs actual 0.752 (error +0.013)
- n=5: predicted 0.753 vs actual 0.750 (error +0.003)
- n=6: predicted 0.734 vs actual 0.738 (error −0.004)

### Extrapolation to m05 (n=11)
QED(11) = −0.003264(121) + 0.017022(11) + 0.749324
= −0.39494 + 0.18724 + 0.74932
= **0.542**

Cross-check with 3-point quadratic (C4–C6 only): QED(11) ≈ 0.526.
Cross-check with linear from C3–C6: QED(11) ≈ 0.651 (task says don't assume straight line).
Cross-check with geometric decay from C3: QED(11) ≈ 0.655 (also essentially linear in log-space).

The quadratic model captures the accelerating decline physically expected as ALogP approaches 5 (ALogP at C11 = 4.956). I use the 5-point quadratic as my primary model since it uses the most data.

**Predicted QED for m05: 0.54**

---

## Job 2: Lead Compound Selection

### Filtering by Free Constraints (before measuring)
Constraint 2: PSA ≥ 40
Constraint 3: Not in m05 series (exclude methoxyphenyl scaffold)

Methoxyphenyl series (PSA=38.33) is excluded by both constraints 2 and 3.
Biphenyl series (PSA=29.1) is excluded by constraint 2.

Remaining candidates:
- **Sugar series** (PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- **Nitrophenyl series** (PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Prioritizing by Drug-Like Free Descriptors
Nitrophenyl series members with the most drug-like properties (moderate ALogP, low rotb):
- m09: ALogP=1.345, MW=194, rotb=3
- m02: ALogP=1.735, MW=208, rotb=4
- m12: ALogP=2.125, MW=222, rotb=5

### Measured Results (3 remaining measurements)
| mol_id | QED | PSA | Series | Passes all 3? |
|--------|-------|------|-------------|---------------|
| m09 | 0.5826 | 72.24 | Nitrophenyl | No (QED < 0.6) |
| **m02** | **0.6046** | **72.24** | **Nitrophenyl** | **Yes** |
| m12 | 0.4709 | 72.24 | Nitrophenyl | No (QED < 0.6) |

### Chosen Lead: m02
- Measured QED = 0.6046 (≥ 0.6 ✓)
- PSA = 72.24 (≥ 40 ✓)
- Nitrophenyl series, not m05 series ✓
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` (3-carbon chain)

m02 is the only measured candidate meeting all three constraints. Its moderate ALogP (1.735) and low rotatable bond count (4) give it the best drug-like profile among those measured.
