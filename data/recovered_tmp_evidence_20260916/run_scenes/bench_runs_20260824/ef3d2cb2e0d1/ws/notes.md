# Task notes

## Catalogue (33 molecules), paged through all 9 pages

Budget: 17 total, shared by list_pool + measure_qed. Paging 9 pages used **9 budget → 8 left for measurements**.

### Scaffolds (the part before `C(=O)N`)

Identified four congeneric series by reading SMILES. Chain length = number of carbons after `N`.

**A. Methoxybenzoyl** `COc1ccc(cc1)C(=O)N` (psa 38.33) — THIS IS m05's series:
- m15 `NCC` = chain 2
- m24 `NCCC` = chain 3
- m28 `NCCCC` = chain 4
- m17 `NCCCCC` = chain 5
- m22 `NCCCCCC` = chain 6
- m05 `NCCCCCCCCCCC` = chain 11 (TARGET, not synthesized → probe_unavailable)
Pool only has chains 2–6; target is chain 11 ⇒ EXTRAPOLATION beyond measured range.

**B. Nitrobenzoyl** `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa 72.24):
- m09 chain2, m12 chain4, m02 chain3, m32 chain5, m33 chain6, m29 chain7, m31 chain8, m06 chain9, m10 chain10

**C. Glyconoyl (sugar head)** `OCC(O)C(O)C(O)C(=O)N` (psa 110.02):
- m14 ch2, m27 ch3, m01 ch4, m19 ch5, m03 ch6, m21 ch7, m18 ch8, m25 ch10, m07 ch9
(need to recount: m14=NCC=2, m27=NCCC=3, m01=NCCCC=4, m19=NCCCCC=5, m03=NCCCCCC=6, m21=NCCCCCCC=7, m18=NCCCCCCCC=8, m07=NCCCCCCCCC=9, m25=NCCCCCCCCCC=10)

**D. Biphenylcarbonyl** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa 29.1):
- m13 ch2, m11 ch3, m04 ch4, m30 ch5, m23 ch6, m20 ch7, m26 ch8, m16 ch10, m08 ch9

### Job 2 constraints interplay
Lead needs: QED≥0.6 (measured) AND psa≥40 (free) AND not-in-m05-series.
- psa≥40 ⇒ only series B (nitro, 72.24) and C (glyconoyl, 110.02) qualify.
- series A (methoxy, 38.33) and D (biphenyl, 29.1) fail psa≥40 ⇒ automatically excluded, so "not in m05's series" is automatically satisfied for any psa≥40 candidate. Good.
- So lead must come from nitro or glyconoyl series.

### QED reasoning for lead (helps pick which to measure first)
- Nitro: nitro group is a structural/toxicity alert (lowers QED alert-desirability), but short-chain members have good MW, decent ALogP, HBD=1, low rotb. Longer chains push ALogP high (up to 4.5) and rotb high ⇒ lower. ⇒ prefer SHORT-chain nitro.
- Glyconoyl: HBD=5 (4 OH + amide), PSA 110, ALogP very negative ⇒ several desirabilities low; short chains help MW/rotb. Mixed; short-chain best bet.
⇒ Measure shortest-chain members of each series first.

## Budget plan (8 measurements left)
- Job 1: measure 4 methoxy points (chains 2,3,5,6 = m15, m24, m17, m22) → quadratic fit → extrapolate to chain 11. (Skips chain 4 to save a measurement; endpoints 2 & 6 + interior 3,5 anchor a smooth quadratic.)
- Job 2: 4 shots on promising short-chain nitro/glyconoyl candidates.

## Job 1 — methoxy series measurements (used budget 10–13)
| mol | chain n | QED |
|-----|--------|------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 (peak) |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

Trend: rises to peak near n=3 then declines (the decline is curving/accelerating, not linear).
Quadratic fit (deg=2): QED(n) = 0.70246 + 0.046777·n − 0.006983·n²
RMSE on the 4 points = 0.00946 (vs 0.01411 for linear) ⇒ quadratic captures curvature best.
Extrapolate to n=11 (target m05): **QED ≈ 0.372**.
Chose quadratic over linear: (a) lower RMSE; (b) the ALogP and rotb desirability penalties in QED accelerate past the optimum, so QED vs chain length is genuinely curved — a straight line would under-state the long-chain drop. The linear fit only declines to 0.697 (unrealistic — it barely drops despite 5 extra methylenes past the peak), whereas the quadratic's accelerating decline matches the chemistry.

## Job 2 — lead selection (budget 14–15)
Constraints reminder: QED≥0.6 (measure), psa≥40 (free), not-in-m05-series (free).
psa≥40 ⇒ only nitro (B, 72.24) and glyconoyl (C, 110.02) qualify. A (methoxy, 38.33) and D (biphenyl, 29.1) fail on psa alone ⇒ both excluded; "not in m05 series" auto-satisfied for any psa≥40 candidate.

Measured the two shortest, best-looking nitro members (low MW/ALogP/rotb, HBD=1):
| mol | series | chain | psa | QED |
|-----|--------|-------|-----|------|
| m09 | B nitro | 2 | 72.24 | 0.5826 ❌ <0.6 |
| m02 | B nitro | 3 | 72.24 | 0.6046 ✓ ≥0.6 |

m02 satisfies ALL THREE: QED 0.6046 ≥ 0.6 ✓, psa 72.24 ≥ 40 ✓, nitro series ≠ methoxy (m05) series ✓.
⇒ lead = **m02**, measured QED = **0.6046**.

Why not keep measuring? m09→m02 rose 0.583→0.605 (peak near chain 3); m12 (ch4) and m32 (ch5) have higher rotb/ALogP and will be lower — m02 is at/near the nitro optimum. Glyconoyl (series C, HBD=5, PSA 110, ALogP≤−1.6) is intrinsically less drug-like; its short chains also have many H-bond donors that depress QED, so it's unlikely to beat m02's 0.605. Two measurements left, but m02 already clears all constraints — locking it in rather than gamble on worse candidates.
