# Task notes — predict QED of m05 (never synthesized)

## Target
`m05`, SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — 4-methoxybenzamide with an
N-undecyl chain (11 carbons after the N). `measure_qed` returns probe_unavailable.

## Strategy
Search the pool with a scaffold substring of the target (`COc1ccc(cc1)C(=O)N`,
the para-methoxybenzamide head) to pull the whole congeneric series in one go,
then measure each short-chain member and extrapolate the QED-vs-chain-length
trend out to n=11 (beyond the measured range n=2..6).

## Searches (cost 1/page, shared with measurements; budget 12)
- Query `COc1ccc(cc1)C(=O)N`, page 1 → 4 matches (m05, m15, m17, m22). cost 1.
- Same query, page 2 → 2 matches (m24, m28). cost 1.
- total_matches = 6. So the full series in the pool is exactly:
  m05 (target, n=11), m15 (n=2), m24 (n=3), m28 (n=4), m17 (n=5), m22 (n=6).

## Measurements (cost 1 each)
| mol_id | n (C after N) | MW     | ALOGP  | PSA    | rotb | arom | QED    |
|--------|---------------|--------|--------|--------|------|------|--------|
| m15    | 2             | 179.22 | 1.445  | 38.33  | 3    | 1    | 0.7621 |
| m24    | 3             | 193.25 | 1.835  | 38.33  | 4    | 1    | 0.7919 |
| m28    | 4             | 207.27 | 2.225  | 38.33  | 5    | 1    | 0.7518 |
| m17    | 5             | 221.30 | 2.615  | 38.33  | 6    | 1    | 0.7498 |
| m22    | 6             | 235.33 | 3.005  | 38.33  | 7    | 1    | 0.7377 |
| m05*   | 11            | 305.46 | 4.956  | 38.33  | 12   | 1    | ? (target) |

Budget used: 2 + 5 = 7 / 12.

## Observed trend
QED rises n=2→3 (0.7621→0.7919, peak), then declines monotonically
n=3→6 (0.7919→0.7377). NOT linear: it has a peak at n≈3 and the decline
accelerates slightly as logP/rotB climb.

## How the descriptors move with n (all linear in n, confirmed against m05)
- MW(n)   = 151.18 + 14.02·n      → n=11: 305.4 ✓
- ALOGP(n)= 0.665 + 0.39·n        → n=11: 4.955 ✓ (severely lipophilic)
- rotb(n) = n + 1                 → n=11: 12 ✓
- PSA, arom, HBA, HBD constant across the series.

## Why the decline accelerates beyond the measured range
QED = (product of 8 property weights)^(1/8). Across this series 5 of the 8
weights are constant (PSA, HBA, HBD, arom, alerts); only MW, ALOGP, rotB vary.
- ALOGP weight: near its plateau for logP 1.5–3 (gentle decline seen n=3→6),
  then steepens; at logP≈5 it is strongly penalised.
- rotB weight: monotonically declines; rotB=12 is well into the penalty zone.
- MW weight: rises toward its peak (~250–300) through n=2..6 (this *softens*
  the decline in the measured range) but turns DOWN past the peak, so beyond
  n≈7 it *adds* to the decline rather than offsetting it.
⇒ a straight-line extrapolation (constant per-carbon drop) UNDER-estimates the
  drop; the true decline is faster than linear for n>6.

## Fits and extrapolation to n=11
(1) Quadratic in QED on all 5 points (u=n−4):
    QED = 0.7652 − 0.00909·u − 0.003264·u²  →  QED(11, u=7) ≈ 0.542
    (in-sample fits well but a quadratic over-extrapolates far → likely too low)
(2) Linear in QED on the declining arm n=3..6 (slope −0.0165/C): QED(11) ≈ 0.651
    Linear in ln(QED) on n=3..6: ≈ 0.658
    (these assume constant rate — the trend is NOT linear, so over-estimate)
(3) PHYSICALLY-MOTIVATED fit: since QED is a geometric mean of weights whose
    logs are ~quadratic-in-n (Gaussian MW & logP weights) or linear-in-n
    (exponential rotB), ln(QED) is quadratic in n. Fit lnQ = a+b·u+c·u²:
        lnQ = −0.2681 − 0.0120·u − 0.00431·u²
        lnQ(11) = −0.5635  →  QED(11) = exp(−0.5635) ≈ 0.569
    (in-sample: predicts 0.770/0.771/0.765/0.752/0.734 vs actual
     0.7621/0.7919/0.7518/0.7498/0.7377 — good; peak at n≈2.6 vs observed n=3)
(4) Weight-ratio sanity check (independent of the polynomial fit): extrapolate
    each property weight from n=6→11 using standard QED desirability shapes
    (logP weight steeply penalised at 5, rotB weight declining, MW ~flat near
    peak): QED(11)/QED(6) = [0.98·(0.30–0.40)/0.82·(0.45–0.55)/0.88]^(1/8)
                          ≈ 0.80–0.86  →  QED(11) ≈ 0.59–0.63.

## Chosen prediction
The non-linear fits all land below the linear extrapolation (as the task
warns), in the band 0.54–0.63. The physically-motivated ln(QED)-quadratic
(0.569) is the most defensible single "fit the trend" model; the weight-ratio
check (≈0.60) agrees and is more trustworthy far from data than a raw
polynomial (which over-shoots low). I report the central value:

**target_qed ≈ 0.58**

(sits below the linear 0.65, reflecting the accelerating decline as ALOGP→~5
and rotB→12; above the raw-quadratic 0.54, avoiding polynomial over-extrapolation;
consistent with the weight-function physics.)

## Congeners (same series as m05, found in the pool)
m15, m17, m22, m24, m28  (m05 itself is the target, not a congener).
