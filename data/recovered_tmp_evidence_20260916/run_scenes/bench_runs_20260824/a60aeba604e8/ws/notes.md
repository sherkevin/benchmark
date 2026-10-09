# Notes: QED prediction + lead selection

## Budget tracking
- Total budget: 17 (shared between list_pool and measure_qed).
- Spent on listing all 9 pages of the catalogue: 9. Remaining for measurements: **8**.

## Pool structure (33 molecules, 4 congeneric series)

Each series = fixed scaffold + variable N-alkyl chain length (number of carbons after the amide N).

### Series A — methoxyphenyl benzamide `COc1ccc(cc1)C(=O)N` (psa 38.33) — **m05's series**
| mol_id | chain | AlogP | rotb | MW   |
|--------|-------|-------|------|------|
| m15    | C2    | 1.445 | 3    | 179  |
| m24    | C3    | 1.835 | 4    | 193  |
| m28    | C4    | 2.225 | 5    | 207  |
| m17    | C5    | 2.615 | 6    | 221  |
| m22    | C6    | 3.005 | 7    | 235  |
| **m05**| **C11**| 4.956 | 12  | 305  | (TARGET, never synthesized → probe_unavailable)

Pool only holds short-chain members (C2–C6); m05 (C11) is far beyond → extrapolation.
AlogP rises ~+0.39 per carbon: 1.445 + 0.39*9 = 4.96 ✓ matches m05. So chain length is the right x-axis.

### Series B — nitrophenyl benzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa 72.24)
m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10).
AlogP 1.345 → 4.465. Short chains (C2–C5) have excellent drug-like profiles → Job 2 candidates.

### Series C — sugar-alcohol `OCC(O)C(O)C(O)C(=O)N` (psa 110.02)
m14(C2)…m25(C10). Very negative AlogP + many HBD/HBA → QED likely low. Backup only.

### Series D — biphenyl `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa 29.1)
psa < 40 → excluded from Job 2.

## Job 1 plan (predict m05 QED)
Measure the 5 pool congeners of m05's series: m15(C2), m24(C3), m28(C4), m17(C5), m22(C6).
Fit the observed QED-vs-chain-length trend (degree 2 unless data are clearly linear),
extrapolate to C11. Expect QED to rise C2→C4 (AlogP entering sweet spot, MW improving)
then fall as rotb/AlogP worsen; at C11 (AlogP 4.96, rotb 12) QED should be LOW.

## Job 2 plan (pick lead; constraints: QED≥0.6 AND psa≥40 AND not Series A)
Series A fails psa<40 (and is the target series anyway). Series D fails psa<40.
Candidates: Series B (nitrophenyl) and Series C (sugar). Series B short chains look
best (good AlogP/MW/rotb/PSA). Primary bet: **m12** (C4: AlogP 2.125 optimal, MW 222,
rotb 5, psa 72). If m12 < 0.6, retry m09 (C2, rotb 3), then m02 (C3). 2 spare budget.

## Measurements taken (budget 17, all spent)

**Batch 1** — Job-1 series (methoxyphenyl, m05's congeners) + Job-2 primary candidate:

| mol_id | series | chain | psa   | AlogP  | MW    | rotb | QED    |
|--------|--------|-------|-------|--------|-------|------|--------|
| m15    | A      | C2    | 38.33 | 1.445  | 179   | 3    | 0.7621 |
| m24    | A      | C3    | 38.33 | 1.835  | 193   | 4    | 0.7919 | (peak)
| m28    | A      | C4    | 38.33 | 2.225  | 207   | 5    | 0.7518 |
| m17    | A      | C5    | 38.33 | 2.615  | 221   | 6    | 0.7498 |
| m22    | A      | C6    | 38.33 | 3.005  | 235   | 7    | 0.7377 |
| m12    | B(nitro)| C4   | 72.24 | 2.125  | 222   | 5    | 0.4709 |

**Batch 2** — Job-2 series-ceiling probes (budget exhausted on m07):

| mol_id | series    | chain | psa   | AlogP  | rotb | QED    |
|--------|-----------|-------|-------|--------|------|--------|
| m19    | C(sugar)  | C5    | 110.02| -1.632 | 8    | 0.3244 |
| m21    | C(sugar)  | C7    | 110.02| -0.852 | 10   | 0.3297 |
| m07    | C(sugar)  | C9    | 110.02| -0.072 | 12   | probe/budget exhausted |

## Job 1 — m05 prediction
QED vs chain length in series A: rises to a peak at C3, then gently declines.
The quadratic fit through all 5 points fits well (RMSE 0.012):
  QED(n) ≈ 0.74932 + 0.017024·n − 0.003264·n²
Evaluating at n = 11 (m05's chain) gives **target_qed = 0.5416**.
This is an extrapolation beyond the measured range (C2–C6 → C11): the predicted
~0.20 drop from the C6 value (0.7377) is consistent with the broadening quadratic
and with m05's worsening descriptors (AlogP 4.96, rotb 12).

Degree sensitivity: a degree-1 fit through the same points (slope −0.0091/CH2,
RMSE 0.013) extrapolates to 0.695 at C11. The linear and quadratic fits both
reproduce the measured points (RMSE 0.013 vs 0.012, essentially tied), so they
cannot be discriminated within the measured range — but they diverge sharply on
extrapolation. Task instructs not to assume a straight line; chemically the
downward curvature should *accelerate* past C6 (ALogP 4.96 is far past the QED
ALogP optimum; rotb 12 is high), so the degree-2 value (0.5416) is the more
defensible prediction. Reported value: **0.5416**.

## Job 2 — lead selection
Three constraints: QED ≥ 0.6 AND psa ≥ 40 AND not in series A (m05's series).
- The two eligible series are B (nitrophenyl, psa 72.24) and C (sugar, psa 110.02).
  All biphenyls (psa 29.1) and methoxy molecules (psa 38.33) fail the psa gate.
- Measured ceilings on the two eligible series:
  · m12 (series B, optimal descriptors) = 0.4709 → series B capped below 0.6 by the aromatic nitro alert.
  · m19/m21 (series C, psa 110) = 0.32–0.33 → series C capped well below 0.6 by polyol HBD/HBA + very negative AlogP.
- Therefore no eligible molecule reaches QED ≥ 0.6. The best-justified measured lead is m12
  (best descriptors, best-in-class among eligible molecules, highest QED measured in an eligible series).
  Its QED is 0.4709 — recorded honestly as failing the 0.6 gate; no eligible candidate meets all three constraints.
