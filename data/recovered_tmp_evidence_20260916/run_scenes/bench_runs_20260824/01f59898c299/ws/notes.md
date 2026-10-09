# Notes

## Analysis of Pool Data

### Congeneric Series Identification

**Series A (methoxyphenyl amide): `COc1ccc(cc1)C(=O)N-`** — same scaffold as m05
- m15: NCC (2C), mw=179.22, alogp=1.445, **QED=0.7621**
- m24: NCCC (3C), mw=193.25, alogp=1.835, **QED=0.7919**
- m28: NCCCC (4C), mw=207.27, alogp=2.225, **QED=0.7518**
- m17: NCCCCC (5C), mw=221.30, alogp=2.615, **QED=0.7498**
- m22: NCCCCCC (6C), mw=235.33, alogp=3.005, **QED=0.7377**
- **m05: NCCCCCCCCCCC (11C), mw=305.46, alogp=4.956** — TARGET, never synthesized

**Series B (sugar acid amide): `OCC(O)C(O)C(O)C(=O)N-`**
- m14, m27, m01, m19, m03, m21, m18, m07, m25 (2C to 10C)

**Series C (nitrophenyl amide): `O=[N+]([O-])c1ccc(cc1)C(=O)N-`**
- m09 (2C): **QED=0.5826**
- m02 (3C): **QED=0.6046**
- m12 (4C): **QED=0.4709**
- m32 (5C): **QED=0.4683**
- m33 (6C): **QED=0.4591**
- m29 (7C): **QED=0.4446**
- m06 (9C): **QED=0.4011**

**Series D (biphenyl amide): `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`**
- Not measured

## Budget Used: 12/12
- 5 for Series A: m15, m24, m28, m17, m22
- 7 for others: m12, m32, m33, m02, m09, m06, m29
- m05 measurement confirmed probe_unavailable (no cost)

---

## Job 1: Predicting QED of m05

### Observed Series A Trend
| Chain Length | mol_id | QED    | ΔQED    |
|-------------|--------|--------|---------|
| 2C          | m15    | 0.7621 | —       |
| 3C          | m24    | 0.7919 | +0.0298 |
| 4C          | m28    | 0.7518 | -0.0401 |
| 5C          | m17    | 0.7498 | -0.0020 |
| 6C          | m22    | 0.7377 | -0.0121 |

Key observations:
- QED peaks at 3C (0.7919), then declines
- Decline accelerates from 4→5C (flat) to 5→6C (-0.012)
- The decline is driven by increasing ALOGP (+0.39/CH2), MW (+14/CH2), and ROTB (+1/CH2)

### Extrapolation to 11C (m05)

**Quadratic fit (5 points, u = chain_length - 4):**
QED = -0.003264*u² - 0.00909*u + 0.765188
At 11C (u=7): QED = 0.542

**Exponential model (ln QED linear in chain length):**
ln(QED) = -0.01197*x - 0.22862
At 11C: QED = 0.698

**Cross-series comparison (Series C):**
Series C shows accelerating decline at long chains:
- 5→6C: -0.009/C, 6→7C: -0.015/C, 7→9C: -0.022/C avg
- Decline rate roughly triples over 4 carbons

Applying similar proportional acceleration to Series A (base rate ~0.007/C at 4-6C):
- 6→9C: ~0.014-0.023/C, 9→11C: ~0.021-0.035/C
- Total decline from 6C to 11C: ~0.08-0.14
- Predicted QED(11C): 0.60-0.66

**Final prediction:** Taking a weighted consideration of the quadratic model (0.54), exponential model (0.70), and cross-series analogy (0.60-0.66), I predict **QED ≈ 0.62** for m05.

The decline is nonlinear due to compounding QED desirability penalties as ALOGP reaches 4.956 and ROTB reaches 12. The quadratic captures this acceleration but may overestimate the rate; the exponential is likely too conservative.

---

## Job 2: Lead Selection

### Filtering
1. **PSA ≥ 40**: Eliminates Series D (PSA=29.1) and Series A (PSA=38.33)
2. **Not in Series A**: Eliminates m15, m24, m28, m17, m22
3. Remaining candidates: Series B (PSA=110.02) and Series C (PSA=72.24)

### Measurements of Candidates
| mol_id | Series | Chain | PSA    | QED    | Qualifies? |
|--------|--------|-------|--------|--------|------------|
| m02    | C      | 3C    | 72.24  | 0.6046 | ✅ YES     |
| m12    | C      | 4C    | 72.24  | 0.4709 | ❌ QED<0.6 |
| m32    | C      | 5C    | 72.24  | 0.4683 | ❌ QED<0.6 |
| m33    | C      | 6C    | 72.24  | 0.4591 | ❌ QED<0.6 |
| m29    | C      | 7C    | 72.24  | 0.4446 | ❌ QED<0.6 |
| m06    | C      | 9C    | 72.24  | 0.4011 | ❌ QED<0.6 |
| m09    | C      | 2C    | 72.24  | 0.5826 | ❌ QED<0.6 |

**Selected lead: m02** — the only measured molecule meeting all three constraints.
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓
- Series C (nitrophenyl), not Series A ✓

### Why m02 works
m02 has a 3-carbon chain, giving it moderate ALOGP (1.735) and low ROTB (4), keeping it drug-like. Longer chains in Series C push ALOGP too high and reduce QED below 0.6. Series B molecules (PSA=110.02) have very negative logP values, likely giving low QED.

---

## Summary

- **m05 predicted QED**: 0.62
- **Lead compound**: m02 (measured QED = 0.6046)
- **Congeners of m05 in pool**: m15, m24, m28, m17, m22
