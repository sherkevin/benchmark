# Predicting QED of m05 (unmeasurable target)

## Target
`m05` = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
= 4-methoxybenzamide with an N-alkyl chain. Scaffold (constant) = `COc1ccc(cc1)C(=O)N`;
variable = the N-alkyl chain length. Target has an **undecyl (C11)** chain.
Descriptors (free): mw 305.46, alogp 4.956, psa 38.33, rotb 12, arom 1.
`measure_qed(m05)` = probe_unavailable (never synthesized) → must extrapolate.

## Step 1 — find the series (scaffold-substring search)
Searched the pool for `COc1ccc(cc1)C(=O)N` (the constant benzamide head of m05).
2 pages, total 6 matches (the 5 measurable congeners + the target m05 itself).

Sorted by N-alkyl chain length n (# carbons after N; rotb = n+1; each CH2 adds
~14 mw and ~0.39 alogp):

| mol_id | n  | SMILES tail      | mw    | alogp | psa   | rotb | QED (measured) |
|--------|----|------------------|-------|-------|-------|------|-----------------|
| m15    | 2  | NCC              |179.22 | 1.445 | 38.33 | 3    | 0.7621          |
| m24    | 3  | NCCC             |193.25 | 1.835 | 38.33 | 4    | 0.7919          |
| m28    | 4  | NCCCC            |207.27 | 2.225 | 38.33 | 5    | 0.7518          |
| m17    | 5  | NCCCCC           |221.30 | 2.615 | 38.33 | 6    | 0.7498          |
| m22    | 6  | NCCCCCC          |235.33 | 3.005 | 38.33 | 7    | 0.7377          |
| m05    | 11 | NCCCCCCCCCCC     |305.46 | 4.956 | 38.33 | 12   | ??? (target)    |

Budget: 2 search pages + 5 measurements = 7/12 used. No longer-chain congeners
exist in the pool, so m05 (n=11) is far outside the measured range (n=2–6) →
this is **extrapolation**, not interpolation.

## Step 2 — the observed trend
QED rises from n=2 (0.7621) to a **peak at n=3 (0.7919)**, then declines
monotonically: 0.7518 (n=4) → 0.7498 (n=5) → 0.7377 (n=6).

The rise to n=3 is the ALogP term climbing toward its ~1–3 optimum; the decline
from n=3 onward is MW/ALogP/RotB moving past their optima (chain lengthening
raises MW, ALogP and RotB, all penalised once large).

PSA (38.33), arom (1) are constant across the series, and the 3 hidden QED
inputs (HBD, HBA, structural alerts) are also constant for this scaffold (same
amide/methoxy head, no new donors/acceptors/alerts per CH2). So only the MW,
ALogP and RotB desirability terms vary with n; the rest act as a constant
multiplier. This bounds QED below a positive floor — it cannot fall without limit
— so a naive **quadratic extrapolation is unphysical** (it bends toward 0).

## Step 3 — fitting the decline and extrapolating to n=11
Least-squares linear fit to the decline phase (n=3..6, exact):
  n̄=4.5, Q̄=0.7578; Σ(n−n̄)(Q−Q̄)=−0.08230, Σ(n−n̄)²=5.0
  slope b=−0.01646, intercept a=0.83187  →  Q(n) = 0.83187 − 0.01646·n
  → Q(11) = 0.83187 − 0.18106 = **0.6508**

Cross-check with a component (mechanistic) model: QED is a weighted geometric
mean of 8 desirabilities; with PSA/Arom/HBD/HBA/alerts constant, only the MW,
ALogP, RotB terms change from n=6→11. Using standard QED weights
(MW .66, ALogP .46, PSA .95, RotB .30, Arom .59, HBD .33, HBA .45, alert 1.0;
total 4.74) and rough desirability ratios n=11/n=6:
  MW 235→305 (≈near peak, ~0.6% drag), ALogP 3.0→4.96 (d≈0.95→0.30–0.60,
   ~4–11% drag), RotB 7→12 (~4% drag) ⇒ combined factor ≈0.85–0.91
  ⇒ QED(11) ≈ 0.7377 × 0.85–0.91 ≈ 0.63–0.67, central ≈ 0.65.

The empirical linear extrapolation (≈0.65) and the mechanistic component model
(≈0.65, range 0.63–0.67) converge. A quadratic fit to all 5 points gives
≈0.54, but this is rejected as unphysical for far extrapolation (predicts
unbounded fall, violating the positive floor from the constant terms).

## Conclusion
Predicted QED(m05, n=11) ≈ **0.651** (≈0.65, honest uncertainty ~0.63–0.67).
Every QED value cited above as measured came from a real measure_qed call.
