# Notes — QED prediction & lead selection

## Catalogue (33 molecules), grouped by scaffold

Scaffold A — `COc1ccc(cc1)C(=O)N` (methoxy-benzamide) — **m05's series**
- m15  chain=2  psa=38.33  arom=1
- m24  chain=3  psa=38.33  arom=1
- m28  chain=4  psa=38.33  arom=1
- m17  chain=5  psa=38.33  arom=1
- m22  chain=6  psa=38.33  arom=1
- m05  chain=11 (TARGET, never synthesized, psa=38.33)

Scaffold B — `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide)
- m09  chain=2  psa=72.24  arom=1
- m12  chain=4  psa=72.24  arom=1
- m32  chain=5  psa=72.24  arom=1
- m33  chain=6  psa=72.24  arom=1
- m29  chain=7  psa=72.24  arom=1
- m31  chain=8  psa=72.24  arom=1
- m02  chain=3  psa=72.24  arom=1
- m10  chain=10 psa=72.24  arom=1
- m06  chain=9  psa=72.24  arom=1

Scaffold C — `OCC(O)C(O)C(O)C(=O)N` (sugar-benzamide, non-aromatic)
- m14  chain=2  psa=110.02  arom=0
- m27  chain=3  psa=110.02  arom=0
- m01  chain=4  psa=110.02  arom=0
- m19  chain=5  psa=110.02  arom=0
- m21  chain=7  psa=110.02  arom=0
- m18  chain=8  psa=110.02  arom=0
- m03  chain=6  psa=110.02  arom=0
- m07  chain=9  psa=110.02  arom=0
- m25  chain=10 psa=110.02  arom=0

Scaffold D — `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl-benzamide)
- m13  chain=2  psa=29.1  arom=2
- m11  chain=3  psa=29.1  arom=2
- m04  chain=4  psa=29.1  arom=2
- m30  chain=5  psa=29.1  arom=2
- m23  chain=6  psa=29.1  arom=2
- m20  chain=7  psa=29.1  arom=2
- m26  chain=8  psa=29.1  arom=2
- m16  chain=10 psa=29.1  arom=2
- m08  chain=9  psa=29.1  arom=2

(All scaffolds verified visually from SMILES; m05 series members chain lengths
confirmed with describe_structure.)

## Budget
- 17 total measurements, shared by Job 1 (predict m05) and Job 2 (find lead).
- 9 already spent paging the catalogue. 8 remain.

## Job 1 — predict m05 QED (chain=11, scaffold A)
Same series as m05 (scaffold A): m15(2), m24(3), m28(4), m17(5), m22(6).
Pool only holds short-chain members (max chain=6); m05 is chain=11 → extrapolation.
Plan: measure all 5 congeneric short members (m15,m24,m28,m17,m22), fit a trend,
extrapolate to chain=11.

## Job 2 — lead selection constraints
1. measured QED ≥ 0.6 (must measure)
2. psa ≥ 40  (free)
3. NOT scaffold A (free)

psa ≥ 40 filter on non-A molecules:
- Scaffold A all have psa=38.33 → excluded anyway (also same series as m05).
- Scaffold B (nitro): psa=72.24 ≥ 40 ✓ (9 molecules)
- Scaffold C (sugar): psa=110.02 ≥ 40 ✓ (9 molecules)
- Scaffold D (biphenyl): psa=29.1 < 40 ✗ excluded.

So lead candidates come from scaffold B or C only. Must measure to confirm QED≥0.6.

## Job 1 — measured m05 series (scaffold A)
| chain | mol | QED    |
|-------|-----|--------|
| 2     | m15 | 0.7621 |
| 3     | m24 | 0.7919 |  ← peak
| 4     | m28 | 0.7518 |
| 5     | m17 | 0.7498 |
| 6     | m22 | 0.7377 |

Trend: rises to chain 3, then declines. m05 (chain 11) is far beyond measured range.

Fit results at x_query=11:
- deg2, all 5 pts → 0.5416  (rmse 0.0119) — captures peak, decline accelerates
- deg1, all 5 pts → 0.695   (rmse 0.0131) — misses peak
- deg2, 4 declining pts (3-6) → 0.9378 (upward parabola, artifact, REJECTED)
- deg1, 4 declining pts → 0.6508

Chosen prediction: 0.54 (deg2 fit on all 5 points).
Rationale: task says don't assume a straight line → use the curve that captures
the observed peak. Physical support: m05 alogp=4.956 sits right at the QED
lipophilicity cliff (~5), and rotb=12 is past ideal — an accelerating decline
to ~0.54 is plausible. Linear fits (0.65-0.70) are too optimistic (no curvature);
4-pt quadratic (0.94) is an upward-parabola artifact.

## Job 2 — lead selection reasoning
psa≥40 AND not scaffold A:
- Scaffold A (methoxy, psa=38.33): excluded by psa<40 (and same-series rule).
- Scaffold D (biphenyl, psa=29.1): excluded by psa<40.
- Scaffold B (nitro, psa=72.24): PASSES psa. alogp 1.3-4.5 (ideal!), rotb 3-11.
- Scaffold C (sugar, psa=110.02): PASSES psa. But alogp -2.8..+0.3 (far below
  ideal 1-3) and arom=0 (penalized) → likely LOW QED. Skip.

Best drug-like leads (scaffold B, short chain):
- m09: MW194, alogp1.345, rotb3, arom1 — most drug-like
- m02: MW208, alogp1.735, rotb4, arom1 — excellent
- m12: MW222, alogp2.125, rotb5, arom1 — good (reserve)
Nitro group is NOT a QED/PAINS alert (QED is property-based); HBD/HBA in range.
Plan: measure m09 + m02 (parallel), keep m12 in reserve. Pick the higher that
clears 0.6.

## Job 2 — lead measurements
- m09 → QED 0.5826 (below 0.6, REJECTED)
- m02 → QED 0.6046 (≥ 0.6 ✓) psa 72.24 ✓ scaffold B ✓

✅ LEAD = m02, measured QED = 0.6046
Constraints satisfied:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. scaffold B (`O=[N+]([O-])c1ccc(cc1)C(=O)N`), not scaffold A (m05's) ✓

## Final answers
- target_qed (m05, chain 11) = 0.54 (deg2 fit on all 5 congener points)
- lead_mol_id = m02
- lead_qed_measured = 0.6046
- congener_ids (m05's series, scaffold A) = m15, m24, m28, m17, m22

Note: m05 itself belongs to its own series by definition but is the target
(never synthesized); congener_ids lists the pool's measured members only,
which is what the task asks ("every pool molecule in the SAME series as m05").
