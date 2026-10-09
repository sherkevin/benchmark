# Notes

## Analysis of Pool Data

### m05 target
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: p-methoxyphenyl amide `COc1ccc(cc1)C(=O)N-`
- N-alkyl chain: undecyl (11 carbons)
- MW: 305.46, alogp: 4.956, psa: 38.33

## Job 1: Congeneric Series Identification and QED Prediction

### Series A (same scaffold as m05) — Methoxyphenyl amides: `COc1ccc(cc1)C(=O)N-`
| mol_id | chain | MW | alogp | psa | rotb | QED (measured) |
|--------|-------|--------|-------|-------|------|----------------|
| m15 | 2C (ethyl) | 179.22 | 1.445 | 38.33 | 3 | 0.7621 |
| m24 | 3C (propyl) | 193.25 | 1.835 | 38.33 | 4 | 0.7919 |
| m28 | 4C (butyl) | 207.27 | 2.225 | 38.33 | 5 | 0.7518 |
| m17 | 5C (pentyl) | 221.30 | 2.615 | 38.33 | 6 | 0.7498 |
| m22 | 6C (hexyl) | 235.33 | 3.005 | 38.33 | 7 | 0.7377 |
| **m05** | **11C (undecyl)** | **305.46** | **4.956** | **38.33** | **12** | **PREDICT** |

congener_ids: m15, m24, m28, m17, m22

### Trend Analysis
QED peaks at 3C (0.7919) then declines:
- 2C→3C: +0.0298 (rise to peak)
- 3C→4C: −0.0401
- 4C→5C: −0.0020
- 5C→6C: −0.0121

Overall decline from 3C to 6C: 0.7919 → 0.7377, Δ = −0.0542 over 3 carbons.

### Models Fitted
1. **Linear fit on 3C–6C** (4 points): slope = −0.01646 per carbon
   - QED(11) = 0.8319 − 0.01646×11 ≈ **0.651**

2. **Linear fit on 4C–6C** (3 points): slope = −0.00705 per carbon
   - QED(11) = 0.7377 − 0.00705×5 ≈ **0.703**

3. **Quadratic fit on all 5 points**: y = 0.7493 + 0.01702x − 0.003264x²
   - QED(11) ≈ **0.542** (downward-opening parabola, over-extrapolates)

4. **Quadratic fit on 3C–6C**: y = 0.8561 − 0.02792x + 0.001273x²
   - QED(11) ≈ **0.703** (upward-opening, deceleration model)

5. **Exponential decay (asymptote C=0.5) on 3C–6C**: k ≈ 0.0687
   - QED(11) ≈ **0.668**

6. **alogP-based linear fit** (QED vs alogp for 3C–6C): slope = −0.0463 per logP unit
   - ΔQED = −0.0463 × (4.956 − 3.005) = −0.090
   - QED(11) ≈ **0.647**

### Model Selection
The quadratic-all-5 overestimates the decline (downward-opening parabola diverges to −∞). The pure linear 4C–6C model ignores the steeper early decline. The most physically reasonable models are:
- Linear 3C–6C: 0.651
- Exponential decay: 0.668
- alogP-based: 0.647

These cluster around **0.65**. The task says not to assume linearity; the exponential model (which allows for asymptotic behavior) and the alogP model (which captures the underlying physical driver) both support ~0.65.

As chain length increases beyond 6C, alogP reaches ~5 (well above the QED-optimal range of ~1–3) and ROTB reaches 12 (also penalized). The decline is expected to continue but may decelerate as penalty functions saturate.

### **Predicted QED for m05: 0.65**

---

## Job 2: Lead Selection

### Constraint Screening (before spending measurements)

**Constraint 2: PSA ≥ 40**
- Series A (methoxyphenyl): PSA = 38.33 ✗ (all fail)
- Series B (polyol): PSA = 110.02 ✓
- Series C (nitrophenyl): PSA = 72.24 ✓
- Series D (biphenyl): PSA = 29.1 ✗ (all fail)

**Constraint 3: Not in same series as m05**
- Series A excluded ✗
- Series B, C, D pass ✓

**Remaining candidates: Series B and Series C**

Series B has very negative alogP (−2.8 to +0.3) and high PSA (110), suggesting low QED. Series C has moderate alogP (1.3–4.5) and reasonable PSA (72), making it the best bet for QED ≥ 0.6.

### Measurements of Series C candidates
| mol_id | chain | alogp | QED | QED ≥ 0.6? |
|--------|-------|-------|-------|-----------|
| m09 | 2C | 1.345 | 0.5826 | ✗ |
| m02 | 3C | 1.735 | **0.6046** | **✓** |
| m12 | 4C | 2.125 | 0.4709 | ✗ |
| m32 | 5C | 2.515 | 0.4683 | ✗ |
| m33 | 6C | 2.905 | 0.4591 | ✗ |
| m29 | 7C | 3.295 | 0.4446 | ✗ |
| m31 | 8C | 3.685 | 0.4252 | ✗ |

### Lead Selection
**m02** is the only molecule meeting all three constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Series C (nitrophenyl), not same as m05's Series A ✓

SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` (3C nitrophenyl amide)

## Measurement Budget Used
- 5 measurements for Series A (Job 1)
- 7 measurements for Series C (Job 2)
- Total: 12 / 12
