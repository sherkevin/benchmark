# Notes

## Congeneric Series Identification

### Target molecule m05
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11 carbons in N-alkyl chain)
- Scaffold: methoxybenzene amide `COc1ccc(cc1)C(=O)N-`

### Series A (m05 series) — methoxybenzene amide:
- m15: NCC (2C), mw=179, alogp=1.445
- m24: NCCC (3C), mw=193, alogp=1.835
- m28: NCCCC (4C), mw=207, alogp=2.225
- m17: NCCCCC (5C), mw=221, alogp=2.615
- m22: NCCCCCC (6C), mw=235, alogp=3.005
- **m05: NCCCCCCCCCCC (11C), mw=305, alogp=4.956** (TARGET, never synthesized)

### Series B — nitrobenzene amide `O=[N+]([O-])c1ccc(cc1)C(=O)N-`:
- m09 (2C), m02 (3C), m12 (4C), m32 (5C), m33 (6C), m29 (7C), m31 (8C), m06 (9C), m10 (10C)
- All have psa=72.24

### Series C — gluconamide `OCC(O)C(O)C(O)C(=O)N-`:
- m14 (2C), m27 (3C), m01 (4C), m19 (5C), m03 (6C), m21 (7C), m18 (8C), m07 (9C), m25 (10C)
- All have psa=110.02

### Series D — biphenyl amide `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`:
- m13 (2C), m11 (3C), m04 (4C), m30 (5C), m23 (6C), m20 (7C), m26 (8C), m08 (9C), m16 (10C)
- All have psa=29.1 → **fails psa≥40 constraint, eliminated from Job 2**

## Measurement Plan
Budget: 12 total
- Job 1: Measure 5 congeners of m05 series (m15, m24, m28, m17, m22) → 5 measurements
- Job 2: Need molecule with QED≥0.6, psa≥40, not in Series A
  - Series D eliminated (psa=29.1 < 40)
  - Series B (psa=72.24) and Series C (psa=110.02) are candidates
  - Series B has moderate logP (1.3-4.5) → more drug-like
  - Series C has very negative logP (-2.8 to 0.3) → likely lower QED
  - Remaining budget: 7 measurements for Job 2 candidates

## Measurement Results

### Series A (m05 congeners) - Job 1:
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

**Trend**: QED peaks at 3C (0.7919), then declines as chain length increases.

### Job 2 Candidates:
**Series B (nitrobenzene):**
- m09 (2C): QED = 0.5826 → fails (QED < 0.6)
- m02 (3C): QED = 0.6046 → **PASSES** (QED≥0.6, psa=72.24≥40, not in Series A) ✓
- m12 (4C): QED = 0.4709 → fails (QED < 0.6)
- m32 (5C): QED = 0.4683 → fails (QED < 0.6)

**Series C (gluconamide):**
- m14 (2C): QED = 0.3237 → fails (QED < 0.6)
- m27 (3C): QED = 0.333 → fails (QED < 0.6)
- m01 (4C): QED = 0.3187 → fails (QED < 0.6)

## Analysis & Predictions

### Job 1: QED Prediction for m05 (11C chain)

Fitted a quadratic model to the Series A data: y = a + bx + cx²

Using least squares regression on points (2, 0.7621), (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377):

**Quadratic fit**: y = 0.74932 + 0.017024x - 0.003264x²

Verification at known points:
- x=2: predicted 0.770, actual 0.762 (error +0.008)
- x=3: predicted 0.771, actual 0.792 (error -0.021)
- x=4: predicted 0.765, actual 0.752 (error +0.013)
- x=5: predicted 0.753, actual 0.750 (error +0.003)
- x=6: predicted 0.734, actual 0.738 (error -0.004)

**Extrapolation to m05 (x=11)**:
y = 0.74932 + 0.017024(11) - 0.003264(121)
y = 0.74932 + 0.187264 - 0.394944
**y = 0.542**

The quadratic model captures the accelerating decline in QED as chain length increases, consistent with increasing lipophilicity penalties (m05 has alogp=4.956, near the Lipinski limit of 5).

### Job 2: Lead Compound Selection

**Selected lead: m02**
- QED (measured): 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Series: B (nitrobenzene), not in Series A ✓
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC

m02 is the only molecule measured that satisfies all three constraints. Series B shows a peak QED at 3C (m02), with shorter and longer chains having lower QED. Series C molecules all have very low QED due to highly negative logP values.

## Final Answer
- **target_qed (m05)**: 0.542
- **lead_mol_id**: m02
- **lead_qed_measured**: 0.6046
- **congener_ids**: ["m15", "m24", "m28", "m17", "m22"]
