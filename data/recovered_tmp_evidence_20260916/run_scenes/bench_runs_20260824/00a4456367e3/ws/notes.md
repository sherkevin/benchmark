# Task notes — predict m05 QED & pick a backup lead

## Budget
- Total budget 17, **shared** by `list_pool` and `measure_qed` (the list_pool tool
  description and the per-call `budget_used` counter both confirm paging costs budget).
- Paged through 9 pages (4/page × 9 = 33 entries) → **9 budget used, 8 left**.
- Allocate remaining 8: Job 1 = 5 measurements (series A short chain), Job 2 = 3.

## Catalogue — 4 congeneric series (scaffold = aryl-C(=O)N–R, R = N-alkyl chain)

m05 target: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`  → scaffold **p-anisoyl** `COc1ccc(cc1)C(=O)N`, chain = **C11**.

### Series A — p-anisoyl (SAME as m05) `COc1ccc(cc1)C(=O)N-(CH2)n-CH3`
| mol | n (carbons) | mw | alogp | psa | rotb | arom |
|-----|----|------|-------|------|------|------|
| m15 | 2  | 179.22 | 1.445 | 38.33 | 3 | 1 |
| m24 | 3  | 193.25 | 1.835 | 38.33 | 4 | 1 |
| m28 | 4  | 207.27 | 2.225 | 38.33 | 5 | 1 |
| m17 | 5  | 221.30 | 2.615 | 38.33 | 6 | 1 |
| m22 | 6  | 235.33 | 3.005 | 38.33 | 7 | 1 |
| **m05** | **11** | 305.46 | 4.956 | 38.33 | 12 | 1 |  ← TARGET (unmade)

Pool holds only short-chain (C2–C6) members of m05's series → **extrapolate** to C11.

### Series B — p-nitrobenzoyl `O2N-c6h4-C(=O)N-(CH2)n` (psa 72.24, all)
m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10)

### Series C — biphenyl-4-carbonyl `Ph-Ph-C(=O)N-(CH2)n` (psa 29.1, all)
m13(C2), m11(C3), m04(C4), m30(C5), m23(C6), m20(C7), m26(C8), m08(C9), m16(C10)
→ **psa 29.1 < 40 → fails Job 2 constraint 2**, excluded from lead search.

### Series D — gluconoyl `HOCH2-(CHOH)3-C(=O)N-(CH2)n` (psa 110.02, all; alogp very negative)
m14(C2), m27(C3), m01(C4), m19(C5), m03(C6), m21(C7), m18(C8), m07(C9), m25(C10)

## Job 2 candidate pre-filter (psa≥40 AND not series A)
Eligible: all of series B (psa 72.24) and series D (psa 110.02).
Best-looking by descriptors (QED likes alogp~2-3, mw 200-300, rotb<8):
- nitro C4 m12 (alogp 2.125, mw 222, rotb 5) ← sweet spot
- nitro C2 m09 (alogp 1.345, mw 194, rotb 3)
- nitro C5 m32 (alogp 2.515, mw 236, rotb 6)
- nitro C6 m33 (alogp 2.905, mw 250, rotb 7)
Sugar series D: alogp very negative + high HBD/HBA → likely low QED, deprioritised.

## Measurements taken (budget 9 paging + 6 measures = 15 used, 2 left)

### Job 1 — m05 series (p-anisoyl), C = N-alkyl chain length
| mol | C | QED | alogp | rotb |
|-----|---|-----|-------|------|
| m15 | 2 | 0.7621 | 1.445 | 3 |
| m24 | 3 | 0.7919 | 1.835 | 4 | ← peak
| m28 | 4 | 0.7518 | 2.225 | 5 |
| m17 | 5 | 0.7498 | 2.615 | 6 |
| m22 | 6 | 0.7377 | 3.005 | 7 |
m05 = C11 (target, unmade; alogp 4.956, rotb 12). Pool only has C2–C6 → extrapolate.

Trend: gentle rise to peak at C3, then gentle decline while alogp stays in the
near-optimal 1.4–3.0 band and rotb stays ≤7. From C6→C11 the chain crosses two
QED penalty thresholds — rotb 7→12 (d_ROTB drops sharply past ~10) and alogp
3.0→4.956 (d_ALOGP leaves its plateau near logP 5) — so the decline must
**accelerate** beyond the measured range; a straight-line extrapolation from the
gentle C3–C6 slope would over-state m05.

Three extrapolation routes (genuine extrapolation, not interpolation):
1. **Quadratic LS fit** of the 5 pts, y=a+b·x+c·x² with x=C−4:
   a=0.7652, b=−0.00909, c=−0.003264. At C=11 (x=7): 0.7652−0.0636−0.1599 = **0.542**.
   (Captures the C3 peak/curvature, but a parabola over-states decline far out —
   it crosses 0 near C18, unphysical, so treat 0.54 as a loose lower bound.)
2. **Linear from the C3 peak** (C3→C6 slope −0.0181/C): 0.7919−8×0.0181 = **0.647**.
   (Uses the steepest measured slope → upper bound; ignores plateau-exit steepening.)
3. **Property / geometric-mean (most physical):** QED = (Π d_i)^(1/8). From m22(C6)
   to m05(C11) only d_ALOGP (logP 3.0→4.96, ~0.92→~0.68) and d_ROTB (7→12,
   ~0.62→~0.23) leave their plateaus; the other six d-terms (MW,HBA,HBD,PSA,
   AROM,ALERTS) are ~constant. Ratio = [(0.68·0.23)/(0.92·0.62)]^(1/8)
   ≈ 0.274^(1/8) ≈ 0.85 → 0.7377×0.85 ≈ **0.63**. The ^1/8 dilution is what
   keeps the drop modest even though two d's fall hard. (Observed C4→C6 only
   −0.007/C because both d's were still on-plateau; C6→C11 steepens, but the
   geometric mean caps the damage.) Bracketing d_ROTB(12) 0.2–0.55 and
   d_ALOGP(4.96) 0.55–0.72 gives a property range **0.60–0.67.**

Quadratic 0.54 (loose lower), linear 0.65 (upper), property 0.60–0.67 (physical).
The quadratic is known to over-extrapolate; the property route matches the linear
upper bound's neighborhood and is the most grounded.
→ **target_qed = 0.63** (central forecast; honest range 0.58–0.66).

### Job 2 — lead hunt (psa≥40 AND not series A) — ALL TOP PICKS FAILED ≥0.6
- m12 (nitro C4: alogp 2.125, mw 222, rotb 5 — near-optimal descriptors) →
  **QED 0.4709, FAILS ≥0.6.** Descriptors look drug-like yet QED is poor: the
  p-NO2 aryl carries a heavy QED penalty (high PSA 72 vs 38, +2 HBA, and a
  toxicophore/alert term) that tanks the geometric mean. That penalty is constant
  across the whole nitro series, so nitro congeners are likely all capped ~0.47–0.52
  (anisoyl peak was only +0.04 above its C4, so nitro C2/C3 ≈ 0.48–0.52, still <0.6).
- m25 (sugar C10, alogp 0.318, rotb 13) → **QED 0.3148, FAILS.**
- m07 (sugar C9, alogp −0.072, rotb 12) → **QED 0.3233, FAILS.**
  Long-chain sugars are sunk by rotb 12–13 (deep in the d_ROTB penalty zone) on top
  of PSA 110 and HBD 5.

Budget is now **17/17 exhausted** (confirmed: m02 returned `budget_exhausted`),
so the untested borderline candidates cannot be measured:
  - short-chain sugars m14/m27/m01 (rotb 5–7, better than the long-chain sugars I
    measured) — component estimate ≈ 0.55, plausibly the intended ≥0.6 lead, but
    I spent my sugar measurements on the wrong (longest-chain) end of the series.
  - nitro short-chain m09(C2)/m02(C3) — likely ~0.48–0.52, below 0.6.

Every measured molecule with QED ≥ 0.6 (m15/m24/m28/m17/m22) is in series A
(m05's own series) and so fails constraint 3. Every measured molecule outside
series A (m12/m25/m07) fails constraint 1. So **no measured molecule satisfies
all three constraints.** Best available lead = the highest-QED measured
non-series-A, psa≥40 molecule: **m12, QED 0.4709** (satisfies constraints 2 & 3,
fails constraint 1). This is an honest limitation of a budget-exhausted run, not
a hidden satisfying pick.
