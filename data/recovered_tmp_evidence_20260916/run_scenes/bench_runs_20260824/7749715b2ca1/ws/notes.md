# Task notes

## Pool analysis (33 molecules)

Scaffold families (vary N-alkyl chain length only):

1. **Methoxybenzamide** (COc1ccc(cc1)C(=O)N + alkyl) — psa=38.33, arom=1:
   - m15 (C2, MW 179.22, alogp 1.445, rotb 3)
   - m24 (C3, MW 193.25, alogp 1.835, rotb 4)
   - m28 (C4, MW 207.27, alogp 2.225, rotb 5)
   - m17 (C5, MW 221.30, alogp 2.615, rotb 6)
   - m22 (C6, MW 235.33, alogp 3.005, rotb 7)
   - m05 (C11, MW 305.46, alogp 4.956, rotb 12) ← TARGET, never synthesized

2. **Nitrobenzamide** (O=[N+]([O-])c1ccc(cc1)C(=O)N + alkyl) — psa=72.24, arom=1:
   - m09 (C2, MW 194.19, alogp 1.345, rotb 3)
   - m12 (C4, MW 222.24, alogp 2.125, rotb 5)
   - m32 (C5, MW 236.27, alogp 2.515, rotb 6)
   - m33 (C6, MW 250.30, alogp 2.905, rotb 7)
   - m02 (C3, MW 208.22, alogp 1.735, rotb 4)
   - m29 (C7), m31 (C8), m06 (C9), m10 (C10)

3. **Biphenyl benzamide** (c1ccc(cc1)-c1ccc(cc1)C(=O)N + alkyl) — psa=29.1 (excluded, <40)

4. **Sugar/polyol benzamide** (OCC(O)C(O)C(O)C(=O)N + alkyl) — psa=110.02, arom=0:
   - m14 (C2), m27 (C3), m01 (C4), m19 (C5), m03 (C6), m21 (C7), m18 (C8), m07 (C9), m25 (C10)

## m05's congeneric series (Task 1 prediction source)
Same scaffold = 4-methoxybenzamide, differ only in N-alkyl chain length:
{m15 (C2), m24 (C3), m28 (C4), m17 (C5), m22 (C6), m05 (C11)}.
m05 never synthesized → must extrapolate QED from the other 5 members.

## Task 2 candidates (psa>=40 AND not methoxy series)
- Nitro series (psa 72.24): m09,m12,m32,m33,m02,m29,m31,m06,m10
- Sugar series (psa 110.02): m14,m27,m01,m19,m03,m21,m18,m07,m25
- Excluded: methoxy series (in m05 series), biphenyl (psa 29.1 <40)

QED driver: optimal MW~210, alogp~2.8, psa~50-70, rotb~2, arom~1-3.
Nitro short-chain (m32 C5 MW236 alogp2.5; m33 C6 MW250 alogp2.9) most promising.

## Budget: 12 measurements total.
Plan: measure methoxy series (5) + nitro candidates m32,m33 (2) = 7.
