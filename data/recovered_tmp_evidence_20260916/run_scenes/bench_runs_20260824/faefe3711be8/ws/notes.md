# Task notes

## Goal
- Job 1: Predict QED of `m05` (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) — never synthesized, extrapolate from its congeneric series.
- Job 2: Pick a lead with measured QED ≥ 0.6, psa ≥ 40, NOT in m05's series.

## Budget
- 17 total measurements shared across both jobs.

## m05 SMILES
`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: p-methoxybenzamide (COc1ccc(cc1)C(=O)N-), N-alkyl chain = 11 carbons.

## Full pool catalogue (33 mols, all benzamide ?c1ccc(cc1)C(=O)N-alkyl)
Four congeneric series by para-substituent:

### Series A — p-methoxy (COc1...) — m05's series (psa=38.33, all FAIL psa≥40)
| mol | chain C | rotb |
|-----|---------|------|
| m15 | 2 (NCC)        | 3 |
| m24 | 3 (NCCC)       | 4 |
| m28 | 4 (NCCCC)      | 5 |
| m17 | 5 (NCCCCC)     | 6 |
| m22 | 6 (NCCCCCC)    | 7 |
| m05 | 11 (target)    | 12 — unmeasurable |

Congeners of m05 in pool = {m15, m24, m28, m17, m22}. Need to extrapolate from chain 2..6 out to 11.

### Series B — p-nitro (O=[N+]([O-])c1...) — psa=72.24 PASS
m09(C2), m12(C4), m02(C3), m33(C6), m32(C5), m29(C7), m31(C8), m10(C10)

### Series C — polyhydroxyalkyl (OCC(O)C(O)C(O)C(=O)N...) — psa=110.02 PASS
m14(C2), m27(C3), m01(C4), m03(C6), m19(C5), m21(C7), m18(C8), m07(C9), m25(C10)

### Series D — biphenyl (c1ccc(cc1)-c1ccc(cc1)C(=O)N...) — psa=29.1 FAIL
m13,m11,m04,m30,m23,m20,m26,m16 — excluded from lead search (psa<40).

## Budget tracking
- list_pool pages 1-9 = 9 units. measure_qed(m05)=probe_unavailable = free (0).
- Job 1: measured m15,m24,m28,m17,m22 = 5 → total 14.
- Job 2: measured m09,m14,m02 = 3 → total 17 (budget exhausted).

## Job 1 — m05 QED prediction

Measured series A (p-methoxybenzamide) QED vs N-alkyl chain length C:
| C | mol | QED |
|---|-----|------|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Trend: rises from C2→C3 (peak 0.7919) then declines monotonically through C6. Clearly curved (not linear). m05 is at C11 — extrapolating far beyond measured range (C2–6).

Fits (x=chain length C, y=QED):
- deg-2, all 5 pts (C2–6): coeffs [0.74932, 0.017024, -0.003264] → at C11 = **0.5416** (rmse 0.01188). CHOSEN MODEL.
- deg-1, all 5 pts: 0.695 (rmse 0.01308) — ignores curvature; task says don't assume linear.
- deg-2, C3–6 only: 0.9378 (rmse 0.00539) — predicts an upturn at long chain, physically implausible; overfits local wiggle. Discarded.

Choosing deg-2 on all 5 points: it captures the observed peak-then-decline curvature and extrapolates the decline accelerating. Physically sensible: alogp climbs 3.0→5.0 and rotb 7→12 between C6 and C11, both increasingly penalized by QED (Lipinski/alert weights). The accelerating decline is what QED should do for a greasy, floppy long chain.

**target_qed (m05, C11) = 0.5416 ≈ 0.54**

## Job 2 — lead search

Constraints: QED≥0.6 (measured), psa≥40 (free), not series A (free).
- Series D fails psa (29.1). Series A fails psa (38.33) anyway and is m05's series.
- Eligible families: Series B (nitro, psa 72.24) and Series C (polyhydroxy, psa 110.02).

Measured (budget = 3 left after Job 1):
- m09 (nitro C2, rotb3, mw194): QED 0.5826 — below 0.6.
- m14 (polyhydroxy C2, rotb5, mw193): QED 0.3237 — far below; polyhydroxy series too polar/negative-logP, poor lead family.
- m02 (nitro C3, rotb4, mw208, alogp1.735): QED **0.6046** — exceeds 0.6.

Lead = **m02**. Checks:
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. series B (O=[N+]([O-])c1...) ≠ m05's series A (COc1...) ✓
