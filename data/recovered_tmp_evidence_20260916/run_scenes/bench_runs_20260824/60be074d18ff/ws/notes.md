# Notes — predicting m05 QED and picking a backup lead

## Data: four congeneric series (same scaffold, N-alkyl chain length differs)

Scaffold identified from SMILES; chain length = #carbons after the amide N.

| series | scaffold (aryl of Ar-C(=O)N-) | members (chain → mol_id) | psa |
|---|---|---|---|
| **methoxy** (m05's series) | 4-methoxyphenyl | C2 m15, C3 m24, C4 m28, C5 m17, C6 m22, **C11 m05 (target, unsynth)** | 38.33 |
| nitro | 4-nitrophenyl | C2 m09, C3 m12, C4 m02, C5 m32, C6 m33, C7 m29, C8 m31, C9 m06, C10 m10 | 72.24 |
| sugar/pentaol | OCC(O)C(O)C(O)- | C2 m14, C3 m27, C4 m01, C5 m19, C6 m03, C7 m21, C8 m18, C9 m07, C10 m25 | 110.02 |
| biphenyl | biphenyl-4-yl | C2 m13, C3 m11, C4 m04, C5 m30, C6 m23, C7 m20, C8 m26, C9 m26…, C10 m16 | 29.1 |

`congener_ids` for m05 = the methoxy series pool members: m15, m24, m28, m17, m22.
(m05 itself is the target and is never measured; it is not listed as a pool congener.)

## Job 1 — predict m05 QED (methoxy series, extrapolate C6 → C11)

Measured (budget used 5 here):

| chain | mol_id | QED |
|---|---|---|
| C2 | m15 | 0.7621 |
| C3 | m24 | 0.7919  ← peak |
| C4 | m28 | 0.7518 |
| C5 | m17 | 0.7498 |
| C6 | m22 | 0.7377 |

Trend: rises C2→C3, then **declines with a decelerating tail** (C3→C6:
0.7919→0.7518→0.7498→0.7377; per-step drops 0.040, 0.002, 0.012 — clearly
noisy/stepwise, not linear). QED is bounded in [0,1] and plateaus as chains grow,
so a linear extrapolation is wrong. The tail is asymptoting near ~0.74–0.75.

m05 is C11, 5 carbons beyond the longest measured member (C6) — pure extrapolation,
cannot be measured (probe_unavailable).

**Model fitted:** exponential-decay-to-floor on the tail n≥3,
  Q(n) = L + A·e^{-k(n-3)}, anchored at Q(3)=0.7919 with the plateau L≈0.74.
The C3→C6 data is too noisy to over-fit (per-step drops are non-monotone), so I do not
trust a single r/k; I instead read the **plateau level (~0.74) and add a modest further
decline** for the 5 extra carbons plus the large logP growth (alogp 3.005 at C6 → 4.956
at C11; QED's logP weight penalises logP approaching 5).

Decelerating-tail arithmetic: average C3→C6 decline ≈0.018/step but decelerating;
5 more steps with continued deceleration ≈ 0.05–0.08 further decline below the C6
value of 0.7377, plus a little extra for the logP penalty crossing into the >3.5 zone.

**Predicted QED(m05, C11) ≈ 0.68.**

(Plausible band ~0.65–0.71; I report 0.68. It must stay below the C6 value because the
tail is still drifting down and m05 is markedly more lipophilic.)

## Job 2 — backup lead (QED ≥ 0.6 AND psa ≥ 40 AND not methoxy)

Free pre-filters first (constraints 2 & 3 cost nothing):
- psa ≥ 40 ⇒ exclude **biphenyl** (29.1) and **methoxy** (38.33, also m05's series —
  excluded by constraint 3 anyway). Only **nitro** (72.24) and **sugar** (110.02) qualify.
- not-in-m05-series ⇒ nitro and sugar only. Good.

Then measure to satisfy constraint 1 (QED ≥ 0.6). Budget after Job 1: 7 left.

Measured for the lead search:

| mol_id | series | chain | QED | ≥0.6? |
|---|---|---|---|---|
| m09 | nitro | C2 | 0.5826 | no |
| m12 | nitro | C3 | 0.4709 | no |
| m02 | nitro | C4 | **0.6046** | **yes** |
| m14 | sugar | C2 | 0.3237 | no |
| m27 | sugar | C3 | 0.3330 | no |

- Sugar series is far too low (psa 110 → QED floor ~0.32). Abandoned.
- Nitro series: C2 0.5826, C3 0.4709 (dip), C4 0.6046 (rebound). Non-monotone but
  **m02 = 0.6046 ≥ 0.6**.

**Chosen lead = m02.**
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- scaffold = 4-nitrophenyl, NOT the methoxy series of m05 ✓

All three constraints satisfied. (2 budget units left unused; no further measurement
needed — m02 already qualifies on all three.)

## Budget accounting
Total 12; used 10 (5 methoxy for Job 1 + 5 for the lead search: m09, m12, m14, m27, m02).
