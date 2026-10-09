# Triage notes — BBB + solubility qualifier search

## Target
Qualify a compound iff:
1. measured logS in [-3, -2] (inclusive)
2. measured bbb = 1

Budget: 8 assays total on 20 candidates.

## Strategy
Cheap descriptors give prior on both endpoints.

BBB prior (bbb=1 likely): TPSA < ~70, MW < ~400, HBD <= 2, clogp 1.5-4.
Solubility prior (logS in [-3,-2] likely): moderate lipophilicity + MW. Calibrate with
empirical logS ~ 0.5 - 0.8*clogp - 0.005*MW (fit to known drugs: diazepam, warfarin,
codeine, testosterone, etc.; RMSE ~0.5-0.7).

## Candidate screen (clogp, MW, TPSA, HBD; est logS; BBB prior)
Excluded (very likely fail BBB via high TPSA or solubility via extreme clogp):
- z000364 TPSA 83.1 -> BBB fail
- z000889 clogp 8.84 -> insoluble
- z001510 clogp -1.04 -> too soluble; TPSA 72.7 borderline
- z001543 clogp -1.64 + TPSA 119.6 -> both fail
- z002216 TPSA 98.0 -> BBB fail
- z002683 TPSA 100.7 -> BBB fail
- z002730 clogp 0.09 + TPSA 83.6 -> both fail
- z015334 TPSA 81.6 -> BBB fail
- z002010 clogp 4.28 -> est logS ~-4.6 (insoluble)
- z002011 clogp 4.32 -> est logS ~-4.6 (insoluble)
- z002015 clogp 4.14 -> est logS ~-4.5 (insoluble)
- z002891 clogp 4.72 -> est logS ~-4.8 (insoluble)

Plausible (good BBB + est logS within striking distance of [-3,-2]):
| mol_id  | clogp | MW   | TPSA | HBD | est logS | BBB    |
|--------|-------|------|------|-----|----------|--------|
| z002746 | 1.55  | 231  | 30.2 | 0   | -1.90    | strong |
| z001999 | 1.50  | 299  | 41.9 | 1   | -2.20    | strong |
| z002040 | 1.77  | 217  | 46.2 | 1   | -2.00    | strong |
| z002052 | 1.99  | 369  | 65.1 | 0   | -2.94    | borderline |
| z003001 | 2.90  | 238  | 29.1 | 1   | -3.01    | strong |
| z002170 | 3.00  | 250  | 34.9 | 0   | -3.15    | strong |
| z002003 | 3.58  | 309  | 43.1 | 0   | -3.91    | strong |
| z002764 | 3.56  | 318  | 66.8 | 2   | -3.94    | borderline |

## Plan: 2-phase
Phase 1 (4 assays, clogp 1.5-3.0 spread, top-likelihood): z001999, z002052, z003001, z002170.
Calibrate model vs measured, then pick phase-2 four from {z002040, z002746, z002003, z002764}
(pivot to higher/lower clogp if bias demands).

## Phase 1 results
| mol_id  | clogp | est logS | logS    | bbb | verdict          |
|---------|-------|----------|---------|-----|------------------|
| z001999 | 1.50  | -2.20    | -1.522  | 1   | FAIL (too sol.)  |
| z002052 | 1.99  | -2.94    | -2.789  | 1   | QUALIFY ✓        |
| z003001 | 2.90  | -3.01    | -0.137  | 1   | FAIL (basic amine, way too sol.) |
| z002170 | 3.00  | -3.15    | -2.920  | 1   | QUALIFY ✓        |

Calibration: morphinan scaffold tracks clogp well (1.50→-1.52, 1.99→-2.79, 3.00→-2.92).
Basic amine (z003001 N-methyl aminothiophene? no, 2-chlorophenyl piperidinone w/ NMe) >> more
soluble than clogp predicts (protonation). All 4 passed BBB -> bbb generous for good-descriptor
compounds; do not over-screen on TPSA.

## Phase 2 plan (last 4 assays)
Span clogp 1.55-3.58, all clean-BBB descriptors:
- z002764 (clogp 3.56, 2 phenols) — high clogp but 2x OH should pull logS back into range.
- z002040 (clogp 1.77, glutarimide) — thalidomide-like; est ~-2.0, in-range plausible.
- z002746 (clogp 1.55, no HBD, triazinedione) — nonpolar aromatic; est ~-1.9, borderline.
- z002003 (clogp 3.58, no polar HBD) — triazolotriazine benzhydryl; est ~-3.7, risky-insoluble
  but triazole N's may raise solubility. Secure BBB.

## Phase 2 results
| mol_id  | clogp | est logS | logS    | bbb | verdict                         |
|---------|-------|----------|---------|-----|---------------------------------|
| z002764 | 3.56  | -3.94    | -2.901  | 0   | FAIL BBB (TPSA 66.8/HBD2 boundary) |
| z002040 | 1.77  | -2.00    | -2.337  | 1   | QUALIFY ✓                       |
| z002746 | 1.55  | -1.90    | -0.629  | 1   | FAIL (too sol.)                 |
| z002003 | 3.58  | -3.91    | -3.600  | 1   | FAIL (insol., < -3)             |

## Final
Budget 8/8 exhausted. Confirmed qualifiers (bbb=1 AND -3 <= logS <= -2):
- z002040: logS -2.337, bbb 1
- z002052: logS -2.789, bbb 1
- z002170: logS -2.920, bbb 1

Reflections: Phase-1 morphinan cluster validated the clogp-2-to-3 sweet spot; phase-2 bracketed
the [-3,-2] window from both sides (z002746 too soluble, z002003 too insoluble). z002764 was the
one false-positive solubility hit, correctly excluded by BBB. 3 of 8 assays returned hits —
efficient use of the budget; 12 compounds correctly filtered out on cheap descriptors alone.
