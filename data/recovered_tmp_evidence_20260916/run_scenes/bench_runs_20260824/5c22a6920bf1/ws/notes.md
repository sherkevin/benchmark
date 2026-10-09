# Notes

## Task overview
- Task 1: Report QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC). m05 unsynthesized → probe_unavailable. Must predict from trend of its OWN congeneric series.
- Task 2: Pick ONE molecule with: measured QED ≥ 0.6, psa ≥ 40, NOT in m05's congeneric series.
- Budget: 12 measure_qed calls total, shared.

## Pool analysis — congeneric series (same scaffold, only N-alkyl chain length differs)

**m05's series (methoxybenzamide, COc1ccc(cc1)C(=O)N-alkyl, psa=38.33):**
| mol | chain C | mw | alogp | rotb |
|-----|---------------|-------|------|
| m15 | 2 | 179.22 | 1.445 | 3 |
| m24 | 3 | 193.25 | 1.835 | 4 |
| m28 | 4 | 207.27 | 2.225 | 5 |
| m17 | 5 | 221.30 | 2.615 | 6 |
| m22 | 6 | 235.33 | 3.005 | 7 |
| m05 | 11 | 305.46 | 4.956 | 12 |  ← target, unsynthesized

Properties vary linearly with chain length n: alogp = 1.445 + 0.39*(n-2); rotb = 3+(n-2); mw = 179.22 + 14.02*(n-2). m05 (n=11) = exact linear extrapolation. ✓ confirms n=11.

**Other series:**
- Nitrobenzamide (O=[N+]([O-])c1ccc(cc1)C(=O)N-alkyl, psa=72.24): m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10)
- Biphenylbenzamide (c1ccc(cc1)-c1ccc(cc1)C(=O)N-alkyl, psa=29.1): m13,m11,m04,m30,m23,m20,m26,m08,m16
- Sugar amide (OCC(O)C(O)C(O)C(=O)N-alkyl, psa=110.02): m14,m27,m01,m19,m03,m21,m18,m07,m25

## Task 2 candidate filter
psa ≥ 40 AND not m05's series (methoxy, psa 38.33) AND not biphenyl (psa 29.1):
- **Nitro series (psa 72.24)**: all 9 qualify on psa. Main candidates.
- **Sugar series (psa 110.02)**: all 9 qualify on psa. BUT alogp very negative (-2 to -3) + high rotb → likely LOW QED. Probably fails QED≥0.6.

So Task 2 most likely from NITRO series. Sugar is a fallback if nitro somehow fails.

## Budget plan
- Task 1: measure all 5 synthesizable methoxy members (m15,m24,m28,m17,m22) → fit QED vs n → extrapolate to n=11. (5 calls)
- Task 2: measure nitro candidates near the QED sweet spot (alogp~2-3, mw~220-260) until ≥0.6 found. (~3-4 calls)
- Total ~8-9, within 12.

## Measurements

### Task 1 — methoxy series (m05's congeneric series)
m05 → probe_unavailable (unsynthesized, as stated). Cost 0.
| mol | n | QED |
|-----|------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

Linear regression QED = a + b·n over all 5 points:
- b = Σ(dx·dy)/Σ(dx²) = −0.09090/10 = −0.009090
- a = mean_y − b·mean_x = 0.75866 + 0.03636 = 0.79502
- At n=11: QED = 0.79502 − 0.009090·11 = 0.6950  (all-points linear fit)

Sanity cross-checks:
- Properties also linear in n: alogp = 1.445 + 0.39·(n−2) → n=11 gives 4.956 ✓ (matches m05 row). mw = 179.22 + 14.02·(n−2) → n=11 gives 305.42 ✓. rotb = 3+(n−2)=12 ✓. Confirms m05 is the n=11 member.
- Declining-tail (n=3–6) linear fit → ~0.647; last-seen slope (n=5→6, −0.0121/u) → ~0.68.
- At n=11, alogp 4.956 and rotb 12 enter the actively-declining desirability zones, so true value likely at/below the all-points line. Range ~0.65–0.70.

Reported target_qed = 0.68 (central estimate robust to both linear-fit anchor 0.695 and declining-regime anchor ~0.65).

### Task 2 — candidates (psa≥40 ⇒ nitro 72.24 or sugar 110.02 series only)
| mol | series | n | psa | alogp | rotb | QED | ≥0.6? |
|-----|--------|------|------|-------|------|------|-------|
| m12 | nitro | 4 | 72.24 | 2.125 | 5 | 0.4709 | ✗ |
| m32 | nitro | 5 | 72.24 | 2.515 | 6 | 0.4683 | ✗ |
| m21 | sugar | 7 | 110.02 | −0.852 | 10 | 0.3297 | ✗ |
| m09 | nitro | 2 | 72.24 | 1.345 | 3 | 0.5826 | ✗ |
| m02 | nitro | 3 | 72.24 | 1.735 | 4 | 0.6046 | ✓ |

Sugar series (m21=0.33) clearly too weak; nitro peaks at short chains like methoxy does.
m02 (C3 nitro) satisfies ALL THREE constraints:
- measured QED 0.6046 ≥ 0.6 ✓
- psa 72.24 ≥ 40 ✓
- nitrobenzamide scaffold ≠ methoxybenzamide (m05's series) ✓

### Budget
Used: m05(0) + m15,m24,m28,m17,m22 (5) + m12,m32,m21,m09,m02 (5) = 10 of 12. 2 remaining, unused.

## Final answers
- target_qed (m05) = 0.68  (predicted from methoxy-series trend)
- lead_mol_id = m02
- lead_qed_measured = 0.6046  (real measure_qed call)
