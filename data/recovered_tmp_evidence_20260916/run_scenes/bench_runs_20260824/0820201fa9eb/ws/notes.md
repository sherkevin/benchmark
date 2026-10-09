# Triage notes — compound collection

## Goal
Find compounds satisfying BOTH:
- measured logS in [-3, -2] (moderate aqueous solubility)
- measured bbb = 1 (brain penetration)

Budget: 8 assays total for 20 compounds. Must triage by cheap descriptors; cannot assay all.

## Heuristic model
- logS ≈ -clogp (crystal-packing/MP unknown, so treat clogp as main driver).
  → logS window [-3,-2] roughly corresponds to clogp ~1.5–3.0 (with MP uncertainty either way).
- BBB: favor low TPSA (<70, ideally <50), low HBD (≤1), MW < ~400, moderate clogp.
  TPSA > 80 → very likely bbb=0.

## Full-screen triage (clogp, tpsa, hbd, mw → verdict)
| mol_id | clogp | tpsa | hbd | mw | verdict |
|---|---|---|---|---|---|
| z000364 | 2.87 | 83.1 | 1 | 399 | BBB borderline (TPSA high) — skip initially |
| z000889 | 8.84 | 29.5 | 1 | 431 | far too lipophilic, logS<<-3 — skip |
| z001510 | -1.04 | 72.7 | 1 | 180 | too soluble (logS>-2) + BBB borderline — skip |
| z001543 | -1.64 | 119.6 | 5 | 286 | too soluble, no BBB — skip |
| z001999 | 1.5 | 41.9 | 1 | 299 | **test** (good BBB, clogp ok) |
| z002003 | 3.58 | 43.1 | 0 | 309 | test? (clogp a bit high → logS risk) |
| z002010 | 4.28 | 29.5 | 0 | 340 | logS too low — skip |
| z002011 | 4.32 | 30.2 | 0 | 326 | logS too low — skip |
| z002015 | 4.14 | 23.6 | 0 | 336 | logS too low — skip |
| z002040 | 1.77 | 46.2 | 1 | 217 | **test** (best dual profile) |
| z002052 | 1.99 | 65.1 | 0 | 369 | test (BBB borderline but hbd0) |
| z002170 | 3.0 | 34.9 | 0 | 250 | **test** (good BBB, clogp boundary) |
| z002216 | 0.86 | 98.0 | 2 | 250 | no BBB — skip |
| z002683 | 2.55 | 100.7 | 2 | 273 | no BBB — skip |
| z002730 | 0.09 | 83.6 | 1 | 258 | too soluble + BBB borderline — skip |
| z002746 | 1.55 | 30.2 | 0 | 231 | **test** (best dual profile) |
| z002764 | 3.56 | 66.8 | 2 | 318 | logS+BBB both borderline — skip |
| z002891 | 4.72 | 34.1 | 0 | 314 | logS too low — skip |
| z003001 | 2.9 | 29.1 | 1 | 238 | **test** (good BBB, clogp boundary) |
| z015334 | 0.74 | 81.6 | 1 | 306 | BBB borderline + too soluble — skip |

## Plan
Two waves (4 + 4), adaptive: wave 1 spans clogp 1.5–3.0 to calibrate the logS model.
- Wave 1: z002040, z002746, z001999, z002170
- Wave 2 (provisional): z003001, z002052, z002003, +1 adaptive

## Results
| mol_id | clogp | logS | bbb | verdict |
|---|---|---|---|---|
| z002040 | 1.77 | -2.337 | 1 | ✓ QUALIFIES |
| z002170 | 3.0 | -2.92 | 1 | ✓ QUALIFIES |
| z002746 | 1.55 | -0.629 | 1 | too soluble |
| z001999 | 1.5 | -1.522 | 1 | too soluble |
| z002052 | 1.99 | -2.789 | 1 | ✓ QUALIFIES |
| z003001 | 2.9 | -0.137 | 1 | too soluble (amide outlier) |
| z002003 | 3.58 | -3.6 | 1 | too insoluble |
| z000364 | 2.87 | -0.948 | 0 | too soluble + no BBB |

## Calibration
- BBB is the EASY constraint: every low-TPSA compound assayed passed bbb=1 (incl. z000364's TPSA 83 still got bbb... no wait z000364 got bbb=0; but z002052 TPSA 65 passed). So TPSA<70 + hbd≤1 ≈ passes.
- logS is the BINDING constraint. Monotonic-ish in clogp for simple cores: 1.5→-1.5, 1.77→-2.34, 1.99→-2.79, 3.0→-2.92, 3.58→-3.6. Sweet spot clogp ≈ 1.77–3.0.
- Outliers (more soluble than clogp predicts): z003001 (N-methyl lactam), z000364 (trimethoxy). H-bond acceptor-rich cores dissolve better than clogp suggests — untestable now (budget spent).

## Final answer
Qualifying (both constraints confirmed by real assays): z002040, z002052, z002170.
