# Notes

## Task
- Task 1: Predict QED of m05 (chain_len 11, never synthesized) from its congeneric series.
- Task 2: Pick a molecule with measured QED ≥ 0.6, psa ≥ 40, NOT in m05's series.
- Budget: 12 total, shared by list_pool + measure_qed. Each successful list_pool page = 1, each measure = 1.

## Scaffold grouping (free: group_by_scaffold)
4 scaffolds, 33 molecules:

1. **m05 series** `COc1ccc(cc1)C(=O)N` (4-methoxy-N-alkyl-benzamide):
   - m15(c2), m24(c3), m28(c4), m17(c5), m22(c6), **m05(c11, unsynth)**. psa≈38.3 (excluded from T2 anyway).

2. **nitrobenzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N`:
   - m09(c2), m02(c3), m12(c4), m32(c5), m33(c6), m29(c7), m31(c8), m06(c9), m10(c10). psa=72.24 ✓ (T2 candidate).

3. **sugar/polyol amide** `OCC(O)C(O)C(O)C(=O)N`: psa≈110, alogp very negative → QED-toxic. Avoid.

4. **biphenyl** `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: psa=29.1 < 40 → fails T2. Avoid.

## m05 actual descriptors (from list_pool p2)
mw=305.46, alogp=4.956, psa=38.33, rotb=12, arom=1. (psa<40, high logP/rotb.)

## Measurements (budget 11/12 used)
Task 1 (m05 series): m15=0.7621(c2), m24=0.7919(c3,peak), m28=0.7518(c4), m17=0.7498(c5), m22=0.7377(c6).
Nitro (Task 2 + Task-1 cross-ref): m09=0.5826(c2), m02=0.6046(c3), m12=0.4709(c4), m10=0.3724(c10).
list_pool p2, p3 = 2 budget.

## Task 1 answer: target_qed ≈ 0.54
- Quadratic fit through all 5 m05-series pts at c=11 → **0.5416** (RMSE 0.0119, best fit).
- Linear tail (c3–c6) → 0.6508 (ignores accelerating decline; rejected).
- Why trust the lower/quadratic: m05's real descriptors (mw=305, alogp=4.956, rotb=12) sit well beyond last measured pt m22 (mw≈277, alogp≈3.5, rotb=7). Between c6→c11, QED penalty thresholds are crossed (MW>300, logP→~5, rotb>10) → accelerating decline, not linear.
- Cross-check: m10 (nitro c10, mw=306/alogp=4.465/rotb=11 — same "extremeness" as m05) measured 0.3724. Methoxy scaffold ≈ +0.18 over nitro at same chain (0.7621 vs 0.5826 at c2), so m05≈0.37+0.18≈0.55. Corroborates quadratic 0.54.

## Task 2 answer: lead = m02, measured QED = 0.6046
- m02 = nitrobenzamide, chain c3. Measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓ (verified via list_pool p3)
- Not in m05's series: m02 scaffold = `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05 scaffold `COc1ccc(cc1)C(=O)N` ✓ (different scaffold entirely)
- Beats other nitro candidates: m09(c2)=0.5826 (just under), m12(c4)=0.4709, m10(c10)=0.3724.

## Deliverables
- answer.json: target_qed=0.54, lead_mol_id="m02", lead_qed_measured=0.6046.
