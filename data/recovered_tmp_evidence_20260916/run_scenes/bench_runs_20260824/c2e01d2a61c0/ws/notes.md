# Notes: predict m05 QED and pick a backup lead

## Pool structure — congeneric series (same scaffold, only N-alkyl chain length differs)

Identified by reading SMILES; each scaffold = an amide `Ar-C(=O)-NH-(CH2)n` where Ar differs.

### Scaffold A — p-methoxybenzamide: `COc1ccc(cc1)C(=O)N` + alkyl chain  (THIS IS m05's series)
- m15 `COc1ccc(cc1)C(=O)NCC`           n=2  (NCC)
- m24 `COc1ccc(cc1)C(=O)NCCC`          n=3  (NCCC)
- m28 `COc1ccc(cc1)C(=O)NCCCC`         n=4  (NCCCC)
- m17 `COc1ccc(cc1)C(=O)NCCCCC`        n=5  (NCCCCC)
- m22 `COc1ccc(cc1)C(=O)NCCCCCC`       n=6  (NCCCCCC)
- m05 `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`  n=11 (target, NEVER made)

**This is the target series.** Pool only holds n=2..6; m05 is n=11 → extrapolate beyond range.

### Scaffold B — p-nitrobenzamide: `O=[N+]([O-])c1ccc(cc1)C(=O)N` + alkyl (psa 72.24, not target series)
- m09 n=2, m12 n=4, m32 n=5, m33 n=6, m29 n=7, m31 n=8, m06 n=9, m10 n=10

### Scaffold C — polyol amide: `OCC(O)C(O)C(O)C(=O)N` + alkyl (psa 110.02, not target series)
- m14 n=2, m27 n=3, m01 n=4, m19 n=5, m03 n=6, m21 n=7, m18 n=8, m07 n=9, m25 n=10

### Scaffold D — biphenylbenzamide: `c1ccc(cc1)-c1ccc(cc1)C(=O)N` + alkyl (psa 29.1 — fails psa>=40)
- m13 n=2, m11 n=3, m04 n=4, m30 n=5, m23 n=6, m20 n=7, m26 n=8, m08 n=9, m16 n=10

## Job 1 — Scaffold A measurements (real measure_qed calls)

| n  | mol_id | QED     |
|----|--------|---------|
| 2  | m15    | 0.7621  |
| 3  | m24    | 0.7919  |
| 4  | m28    | 0.7518  |
| 5  | m17    | 0.7498  |
| 6  | m22    | 0.7377  |

Peak at n=3, then monotone decline. Not a straight line; the downward leg is concave (slope steepens slightly then eases).

### Trend fit & extrapolation to n=11 (m05)
The decline for n>=3 looks roughly linear. Slope over n=3..6: (0.7377-0.7919)/(6-3) = -0.01807 per CH2.
Naive linear carry to n=11: 0.7919 + (-0.01807)*(11-3) = 0.7919 - 0.1446 = 0.6474.

But QED properties saturate/curve (don't extrapolate a straight line blindly). Cross-check with 2-point fits:
- n=3..5 slope: (0.7498-0.7919)/2 = -0.0211/CH2 → n=11: 0.7919 - 0.0211*8 = 0.6231
- n=5..6 slope: (0.7377-0.7498)/1 = -0.0121/CH2 → slower late decline → n=11: 0.7377 - 0.0121*5 = 0.6772
- n=4..6 slope: (0.7377-0.7518)/2 = -0.0071/CH2 → n=11: 0.7518 - 0.0071*7 = 0.7021

The decline rate is decelerating (slope -0.0211 → -0.0121 over the later pairs). Extrapolating the decelerating
decline: long-chain members keep dropping but more slowly. A reasonable decelerating extrapolation to n=11 lands
around 0.66–0.70. Average of the linear endpoints: (0.623, 0.677, 0.702) ≈ 0.67.

Also alogp grows ~+0.5/CH2; m05 alogp=4.956 (well past 5). QED ALERTs penalize ALogP>~3.5 and high rotb/PSA.
m05 has rotb=12 and alogp 4.956 → strong penalty expected, pulling QED below the linear extrapolation of n<=6.

Balancing the decelerating-decline trend with the worsening lipophilicity/rotb penalties at n=11, predicted
QED ≈ **0.65**. (Range of plausible: ~0.60–0.70; pick 0.65.)

## Job 2 — lead constraints (pre-filter before spending measurements)
1. measured QED >= 0.6   (must measure)
2. psa >= 40            (free)
3. not scaffold A       (free)

psa>=40 → only scaffold B (72.24) and scaffold C (110.02) survive. Both are not scaffold A. Good.
So lead ∈ {B ∪ C} = 17 candidates. Need QED>=0.6.

Budget after Job1: 12-5 = 7 measurements left.

### Lead search measurements (real measure_qed calls)
Strategy: probe a spread across n in B and C; short chains tend to score higher (less lipophilic/fewer rotb).

Scaffold B (nitro, psa 72.24):
| n  | mol_id | QED     |
|----|--------|---------|
| 2  | m09    | 0.5826  |
| 3  | m02    | 0.6046  |  ← ≥0.6 ✓
| 4  | m12    | 0.4709  |
| 5  | m32    | 0.4683  |
| 6  | m33    | 0.4591  |

Scaffold C (polyol, psa 110.02):
| n  | mol_id | QED     |
|----|--------|---------|
| 6  | m03    | 0.3282  |
| 7  | m21    | 0.3297  |

Scaffold C capped ~0.33 — very negative alogp + huge PSA → low QED, no ≥0.6 hit possible.
Scaffold B peaks at n=3 (m02, alogp 1.735 ≈ the QED ALogP sweet spot ~1.8), then declines with chain.
m02 = 0.6046 satisfies all three constraints:
  - measured QED 0.6046 >= 0.6 ✓
  - psa 72.24 >= 40 ✓
  - scaffold B (nitro), not scaffold A (m05's methoxy series) ✓

## Final
- target_qed (m05, n=11): 0.65
- lead_mol_id: m02 (QED 0.6046)
- congener_ids (scaffold A pool members, same series as m05): m15, m24, m28, m17, m22
  (m05 itself is not in the pool — it is the target.)
