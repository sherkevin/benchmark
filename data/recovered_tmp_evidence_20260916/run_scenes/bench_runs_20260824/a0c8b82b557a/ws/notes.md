# Task notes

## Setup
- Budget: 12 `measure_qed` calls total, shared by Task 1 & Task 2.
- m05 (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, 11-carbon N-alkyl chain) is unsynthesized → `measure_qed` returns `probe_unavailable`; must predict from its congeneric series trend.

## Congeneric series of m05 (methoxybenzamide scaffold `COc1ccc(cc1)C(=O)N(alkyl)`)
| mol_id | chain C | mw    | alogp  | psa   | rotb | arom |
|--------|---------|-------|--------|-------|------|------|
| m15    | 2       | 179.22| 1.445  | 38.33 | 3    | 1    |
| m24    | 3       | 193.25| 1.835  | 38.33 | 4    | 1    |
| m28    | 4       | 207.27| 2.225  | 38.33 | 5    | 1    |
| m17    | 5       | 221.30| 2.615  | 38.33 | 6    | 1    |
| m22    | 6       | 235.33| 3.005  | 38.33 | 7    | 1    |
| m05    | 11      | 305.46| 4.956  | 38.33 | 12   | 1    | (unsynth, predict)

As chain grows: mw↑, alogp↑ (4.96 at n=11 exceeds optimal ~2.5), rotb↑ (12 at n=11, well past optimal ≤5). Expect QED to peak ~n=4-5 then decline; m05 should be lower.

## Task 2 candidates (psa ≥ 40, NOT methoxy series)
- Methoxy series (m15/m24/m28/m17/m22): psa 38.33 < 40 → excluded.
- Biphenyl series (m04/m08/m11/m13/m16/m20/m23/m26/m30): psa 29.1 < 40 → excluded.
- Nitrobenzamide series `O=[N+]([O-])c1ccc(cc1)C(=O)N(alkyl)` (psa 72.24): m09(n2,rotb3), m02(n3,rotb4), m12(n4,rotb5), m32(n5,rotb6), m33(n6), m29(n7), m31(n8), m06(n9), m10(n10).
- Sugar-amide series `OCC(O)C(O)C(O)C(=O)N(alkyl)` (psa 110.02, alogp very negative ~-2.8..0.3): m14,m27,m01,m19,m03,m21,m18,m07,m25. Likely lower QED (alogp penalty + psa>90 penalty).
- Best QED≥0.6 bets: short-chain nitrobenzamides m09, m02, m12.

## Measurement plan
- Task 1 trend: measure m15, m24, m28, m17, m22 (5 calls).
- Task 2: measure nitrobenzamide short chains m09, m12 (2 calls). Pick one with QED ≥ 0.6.
- Total batch 1: 7 calls. Leaves 5 budget.

## Results — batch 1 (5 calls, Task 1 series)
Methoxy series (m05's congeneric series):
| mol_id | n | QED     |
|--------|---|---------|
| m15    | 2 | 0.7621  |
| m24    | 3 | 0.7919  | ← peak
| m28    | 4 | 0.7518  |
| m17    | 5 | 0.7498  |
| m22    | 6 | 0.7377  |
Shape: peak at n=3, gentle decline n=3→6 (-0.054 total). Drivers of decline: alogp 1.8→3.0, RotB 4→7, MW 193→235 (all still mild).
At n=11 (m05): alogp=4.956 (near the 5.0 cliff, dALOGP→~0.12), RotB=12 (dROTB low), MW=305. Expect a sharper drop than the gentle n=3-6 trend.

## Results — batch 2 (4 calls, Task 2 candidates)
Nitrobenzamide series (psa 72.24, eligible):
| mol_id | n | QED     |
|--------|---|---------|
| m09    | 2 | 0.5826  | ← best so far, but <0.6
| m12    | 4 | 0.4709  |
Steep decline n=2→n=4 (-0.11). Asymmetry (n=2 >> n=4) suggests peak at n≤2... BUT methoxy peaked at n=3, so nitro may also — m02 (n=3) is the key unmeasured candidate.

Sugar-amide series (psa 110.02, alogp very negative): m14(n2)=0.3237, m27(n3)=0.333. Too low (alogp penalty + psa>90 penalty). Not viable.

Budget used: 9 / 12. Remaining: 3.
Task 1 needs no more budget (m05 unsynthesized; no n=7..10 methoxy in pool) — pure extrapolation.
Task 2: measure m02 (nitro n=3) = only candidate that could be a peak ≥0.6.

## Results — batch 3 (1 call)
| mol_id | series | n | QED     |
|--------|--------|---|---------|
| m02    | nitro  | 3 | 0.6046  | ✓ ≥0.6, psa 72.24≥40, nitro≠methoxy scaffold
→ **Task 2 lead = m02, measured QED = 0.6046.**
Budget used: 10 / 12. Remaining: 2.

Nitro series shape now: m09(n2)=0.5826, m02(n3)=0.6046 (peak), m12(n4)=0.4709. Confirms peak-at-n=3, mirroring methoxy. m02 is the unique satisfying candidate.

## Task 1 — predicting m05 (n=11) QED
m05 unsynthesized; no n=7..10 methoxy in pool; trend extrapolation required (pure prediction, no extra budget).

Methoxy data: (n,QED) = (2,0.7621),(3,0.7919),(4,0.7518),(5,0.7498),(6,0.7377).

Need to extrapolate to n=11. Use piecewise linear fit through last two data points (m17 n=5 → m22 n=6), the "downslope" regime m05 lives in:
  slope ≈ (0.7377−0.7498)/(6−5) = −0.0121 per carbon.
  Linear extrapolation n=6→11: QED(11) ≈ 0.7377 + 5×(−0.0121) = 0.7377 − 0.0605 = 0.6772.

Sensitivity — other reasonable models:
  • Quads fit through last 3 pts (n=4,5,6): slope flattening. Linear-regression over last 3 pts gives slope ≈ −0.0098/C → QED(11) ≈ 0.7377 − 5×0.0098 ≈ 0.689.
  • Mirroring nitro (sharper late drop, since alogp/rotb penalties compound): could be ~0.62-0.66.
  • Driver reality check: alogp=4.956 is near the Lipinski/alert-5 cliff (QED ALOGP weight starts failing fast around alogp>5), and RotB=12 >> preferred (≤5). So genuine downside risk; not a flat extrapolation.

Best single-number estimate, weighting the mild downslope baseline with the driver-based downside:
  **target_qed (m05) ≈ 0.68** (range ~0.62–0.69; central estimate 0.68).

Report 0.68.
