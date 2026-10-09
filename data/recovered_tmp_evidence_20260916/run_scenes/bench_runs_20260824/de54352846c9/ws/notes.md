# Notes: predict m05 QED & pick a backup lead

## Setup
- Budget: 12 measurements total, shared by both jobs.
- Pool: 33 molecules, 5 free descriptors (mw, alogp, psa, rotb, arom). QED weighs 8 properties (3 not in table: HBD, HBA, alerts).

## Series identification (by SMILES scaffold; differ only in N-alkyl chain length)

N-alkyl chain = number of CH2 after the amide N.

- **Series A — sugar head `OCC(O)C(O)C(O)C(=O)N`** (psa=110.02, HBD≈4 incl. 3×OH + amide NH): m14(C2), m27(C3), m01(C4), m19(C5), m03(C6), m21(C7), m18(C8), m07(C9), m25(C10). Very negative ALogP, high PSA, high HBD → expect LOW QED.
- **Series B — nitro `O=[N+]([O-])c1ccc(cc1)C(=O)N`** (psa=72.24, HBD=1 amide NH): m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10). Moderate PSA, moderate ALogP, optimal HBD → best QED prospects.
- **Series C — biphenyl `c1ccc(cc1)-c1ccc(cc1)C(=O)N`** (psa=29.1): m13(C2), m11(C3), m04(C4), m30(C5), m23(C6), m20(C7), m26(C8), m08(C9), m16(C10). psa=29.1 < 40 → FAILS psa gate, excluded from lead search.
- **Series D — methoxy `COc1ccc(cc1)C(=O)N` (TARGET m05's series)** (psa=38.33): m15(C2), m24(C3), m28(C4), m17(C5), m22(C6), **m05(C11, target — never synthesized)**.

## Job 1 — m05 series = Series D
Congeners (measurable short-chain members): m15(C2), m24(C3), m28(C4), m17(C5), m22(C6). Target m05 = C11, far beyond measured range → EXTRAPOLATION (not interpolation). Will measure all 5 and fit a degree-2 polynomial.

## Job 2 — lead constraints
1. measured QED ≥ 0.6
2. psa ≥ 40  (free)
3. not in Series D (m05's series)  (free)

### Free pre-filter (psa ≥ 40):
Series D all have psa=38.33 < 40 → automatically excluded. Series C all psa=29.1 → excluded. So lead must come from Series A (psa=110.02) or Series B (psa=72.24). All pass constraint 3 trivially.

### Descriptor reasoning to prioritize measurement (Series B, since Series A's high PSA/HBD/negative ALogP ⇒ low QED):
Best QED prospects by descriptor eyeballing (optimal ALogP ~1.5–3, ROTB ≤5–6, MW ~200–300):
- m12 (C4): mw 222, alogp 2.125, rotb 5, psa 72.24 — best balance (ROTB optimal, ALogP optimal)
- m02 (C3): mw 208, alogp 1.735, rotb 4 — also excellent
- m32 (C5): mw 236, alogp 2.515, rotb 6
- m09 (C2): mw 194, alogp 1.345, rotb 3 (low MW, lowish ALogP)
- m33 (C6): mw 250, alogp 2.905, rotb 7
- m29 (C7): mw 264, alogp 3.295, rotb 8

Measure in priority order until one ≥ 0.6.

## Budget plan
- Job 1: 5 measurements (all of Series D: m15, m24, m28, m17, m22).
- Job 2: up to 7 remaining; likely find a hit within 2–4.

## Batch 1 (7 measurements): m15, m24, m28, m17, m22 (Job 1) + m12, m02 (Job 2 top 2)

### Results
Series D (Job 1):
- m15 (C2): 0.7621
- m24 (C3): 0.7919  ← peak of the series
- m28 (C4): 0.7518
- m17 (C5): 0.7498
- m22 (C6): 0.7377
Trend: rises C2→C3, then declines. Clearly curved (peaked), not linear.

Job 2 prospects (nitro series B):
- m12 (C4): 0.4709 — FAIL <0.6
- m02 (C3): 0.6046 — PASS ≥0.6 ✓ (psa 72.24≥40 ✓, not Series D ✓)

## Batch 2 (free fit + 2 measurements)
- deg-2 fit on Series D: QED(x) = 0.74932 + 0.017024·x − 0.003264·x², RMSE=0.0119, peak at x≈2.6.
- deg-1 fit on Series D: slope −0.00909/CH2, RMSE=0.0131 (worse fit, cannot represent the peak).
- m09 (C2): 0.5826 — FAIL <0.6
- m32 (C5): 0.4683 — FAIL <0.6

deg-2 fit on nitro series (m09,m02,m12,m32) at x=3: predicts 0.5616 (vs measured m02=0.6046). Confirms C3 is the nitro-series peak region, so m02 is the best lead available; m09 just misses at 0.5826.

## Job 1 final prediction for m05 (C11)
Extrapolate the observed Series-D trend to x=11 (far beyond the C2–C6 measured range).
- deg-2 prediction = 0.5416. This is the honest fit: it reproduces the peak at C3 and the gentle decline (RMSE 0.0119, better than deg-1).
- deg-1 prediction = 0.695 (worse RMSE, and the task says don't assume a straight line).
- Report **target_qed = 0.542** (rounded to 3 dp; avoid over-precision given RMSE ~0.012 plus extrapolation uncertainty).

## Job 2 final lead
**m02** — O=[N+]([O-])c1ccc(cc1)C(=O)NCCC (nitro series, C3 chain).
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold = nitrobenzamide, NOT the methoxy scaffold of m05 → not in m05's series ✓
All three constraints satisfied. Other tested nitro members (m09 0.5826, m12 0.4709, m32 0.4683) fail the QED gate; sugar series A is descriptor-unfavorable (high PSA/HBD/neg ALogP) so not pursued given a valid lead already in hand.

## congener_ids (same series as m05, all measurable pool members)
["m15", "m24", "m28", "m17", "m22"]  (chain C2..C6; m05 itself = C11, not measurable)
