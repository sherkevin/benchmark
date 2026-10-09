# Notes — Medicinal Chemistry Analysis

## Congeneric Series Identification

**m05 SMILES:** `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N-` (methoxyphenyl amide)
- N-alkyl chain: 11 carbons

### Series A — Methoxyphenyl amide (same series as m05):
| mol_id | chain | MW | ALogP | PSA | RotB | Arom | QED |
|--------|-------|--------|-------|-------|------|------|-------|
| m15 | 2C | 179.22 | 1.445 | 38.33 | 3 | 1 | 0.7621 |
| m24 | 3C | 193.25 | 1.835 | 38.33 | 4 | 1 | 0.7919 |
| m28 | 4C | 207.27 | 2.225 | 38.33 | 5 | 1 | 0.7518 |
| m17 | 5C | 221.30 | 2.615 | 38.33 | 6 | 1 | 0.7498 |
| m22 | 6C | 235.33 | 3.005 | 38.33 | 7 | 1 | 0.7377 |
| **m05** | **11C** | **305.46** | **4.956** | **38.33** | **12** | **1** | **?** |

### Other series (not same as m05):
- **Series B** (sugar-like, `OCC(O)C(O)C(O)C(=O)N-`): PSA=110.02
- **Series C** (nitrophenyl, `O=[N+]([O-])c1ccc(cc1)C(=O)N-`): PSA=72.24
- **Series D** (biphenyl, `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): PSA=29.1

## Budget Used: 12/12 measurements (m05 was free as probe_unavailable)

## Job 1 — QED Prediction for m05

### Observed Trend in Series A
The QED rises slightly from n=2 (0.7621) to n=3 (0.7919), then monotonically declines:
- n=3→4: drop of 0.0401
- n=4→5: drop of 0.0020  
- n=5→6: drop of 0.0121
- Average decline rate (n=3-6): 0.0181 per carbon

The ALogP increases linearly by 0.390 per CH2. At n=11, ALogP=4.956 (near the QED penalty boundary at ~5) and RotB=12 (above typical limit of 10).

### Models Fitted

1. **Linear (n=2-6):** QED = 0.7950 - 0.00909n → at n=11: **0.695**
2. **Exponential decay (n=2-6):** ln(QED) = -0.2287 - 0.01196n → at n=11: **0.698**
3. **Log-linear QED vs ln(n):** QED = 0.7978 - 0.02976·ln(n) → at n=11: **0.727**
4. **Linear (n=3-6 only):** QED = 0.8319 - 0.01646n → at n=11: **0.651**
5. **Quadratic (n=2-6):** QED = 0.7493 + 0.0170n - 0.00326n² → at n=11: **0.542**
6. **Exp-quadratic (n=2-6):** ln(QED) = -0.2486 - 0.001554n² → at n=11: **0.646**
7. **Exp-quadratic (n=3-6):** ln(QED) = -0.2287 - 0.002279n² → at n=11: **0.604**

### Cross-Validation with Series C
To test extrapolation methodology, I measured a long-chain Series C member:
- Series C: m09(2C)=0.5826, m02(3C)=0.6046, m12(4C)=0.4709, m33(6C)=0.4591, m10(10C)=0.3724
- Decline rate n=4→6: 0.006/carbon; rate n=6→10: 0.022/carbon — **3.7x acceleration**
- This confirms QED decline accelerates at long chain lengths, as LogP and RotB approach boundaries.

### Final Extrapolation
The exp-quadratic model (ln(QED) = a + b·n²) is the most physically motivated:
- It captures the accelerating decline expected when ALogP approaches 5 and RotB exceeds 10
- It fits the data well (comparable RSS to simpler models)
- The full-data fit predicts 0.646; the declining-phase fit predicts 0.604

**Selected model: exp-quadratic on full data** → ln(QED) = -0.2486 - 0.001554·n²
**Predicted QED for m05 (n=11): 0.65** (rounded from 0.646)

## Job 2 — Lead Compound Selection

### Constraint Filtering (free checks first)
1. **PSA ≥ 40:** Series D (PSA=29.1) fails, Series A (PSA=38.33) fails → eliminated
2. **Not same series as m05:** Series A eliminated
3. **Remaining candidates:** Series B (PSA=110.02) and Series C (PSA=72.24)

### QED Measurements on Eligible Candidates
| mol_id | Series | Chain | QED | QED≥0.6? |
|--------|--------|-------|-------|---------|
| m09 | C | 2C | 0.5826 | No |
| m02 | C | 3C | 0.6046 | Yes |
| m12 | C | 4C | 0.4709 | No |
| m33 | C | 6C | 0.4591 | No |
| m14 | B | 2C | 0.3237 | No |
| m01 | B | 4C | 0.3187 | No |

Series B has very low QED (very negative ALogP and high PSA penalize drug-likeness).
Series C peaks at 3C (m02) with QED=0.6046.

### Chosen Lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- QED measured: 0.6046 ≥ 0.6 ✅
- PSA: 72.24 ≥ 40 ✅
- Series: C (nitrophenyl) — not same as m05 ✅
