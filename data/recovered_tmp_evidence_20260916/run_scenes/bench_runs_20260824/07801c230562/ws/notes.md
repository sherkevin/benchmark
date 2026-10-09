# Triage notes

## Task
Find compounds with measured logS in [-3, -2] AND bbb = 1. Budget: 8 assays total for 20 compounds. Catalogue has no sol/BBB data, only cheap descriptors (mw, clogp, tpsa, hbd, hba, rotb, rings).

## Strategy
Heuristics:
- BBB: low TPSA (<~60-70), low HBD (<=1), low MW (<~400). High TPSA/HBD/MW hurt.
- logS in [-3,-2] (moderately soluble): moderate clogP. GSE (no MP): logS ~ 0.8 - 1.02*clogP, so clogP ~2.75-3.73 maps to logS -2..-3 ignoring MP. With MP term (~-1 to -2 for crystalline solids), real qualifying clogP shifts LOWER (~1.3-2.5). Polarity (TPSA/HBD) raises solubility vs clogP-only estimate. So sweet spot clogP ~1.5-3.0 with low-moderate TPSA.

Tension: BBB wants low polarity, solubility wants some polarity -> converge around clogP ~2-3, TPSA ~30-50.

## Compound screen (clogP, TPSA, HBD, MW) -> verdict
- z000364: 2.87, 83.1, 1, 399 — sol good, BBB risk (TPSA high, HBA6, MW399)
- z000889: 8.84, 29.5, 1, 431 — clogP huge -> sol fail. SKIP
- z001510: -1.04, 72.7, 1, 180 — caffeine-like, too soluble + BBB borderline. SKIP
- z001543: -1.64, 119.6, 5, 286 — sugar, too soluble, no BBB. SKIP
- z001999: 1.5, 41.9, 1, 299 — morphinan (morphine-like). BBB+, sol maybe too high. CAND
- z002003: 3.58, 43.1, 0, 309 — BBB+, sol maybe too low. CAND (borderline)
- z002010: 4.28, 29.5, 0, 340 — BBB+, clogP high -> sol fail. SKIP
- z002011: 4.32, 30.2, 0, 326 — same. SKIP
- z002015: 4.14, 23.6, 0, 336 — BBB great, sol risk (flexible -> maybe OK). maybe
- z002040: 1.77, 46.2, 1, 217 — glutethimide-like. BBB+, sol plausible. CAND
- z002052: 1.99, 65.1, 0, 369 — heroin-like (3,6-diacetyl). BBB+, sol plausible. CAND
- z002170: 3.0, 34.9, 0, 250 — methaqualone-like. BBB+, sol plausible. CAND (top)
- z002216: 0.86, 98.0, 2, 250 — sulfonamide, no BBB, too soluble. SKIP
- z002683: 2.55, 100.7, 2, 273 — nitro ketone, no BBB. SKIP
- z002730: 0.09, 83.6, 1, 258 — too soluble, BBB borderline. SKIP
- z002746: 1.55, 30.2, 0, 231 — pyrazinone. BBB+, sol maybe too high. CAND
- z002764: 3.56, 66.8, 2, 318 — both borderline. maybe
- z002891: 4.72, 34.1, 0, 315 — steroid, clogP high -> sol fail. SKIP
- z003001: 2.9, 29.1, 1, 238 — arylpiperidone (ketamine-like). BBB+, sol great. CAND (top)
- z015334: 0.74, 81.6, 1, 306 — too soluble, BBB risk. SKIP

## Plan
Run 6 assays first (span clogP 1.5-3.0, TPSA 29-65):
z003001, z002170, z002040, z002746, z001999, z002052
Then 2 adaptive based on where the qualifying window actually sits.

## Results (all 8 assays, budget exhausted)

| mol_id | clogP | TPSA | HBD | logS | bbb | Qual? |
|---|---|---|---|---|---|---|
| z003001 | 2.9  | 29.1 | 1 | -0.137 | 1 | no (sol too high) |
| z002170 | 3.0  | 34.9 | 0 | -2.92  | 1 | YES |
| z002040 | 1.77 | 46.2 | 1 | -2.337 | 1 | YES |
| z002746 | 1.55 | 30.2 | 0 | -0.629 | 1 | no (sol too high) |
| z001999 | 1.5  | 41.9 | 1 | -1.522 | 1 | no (sol too high) |
| z002052 | 1.99 | 65.1 | 0 | -2.789 | 1 | YES |
| z002003 | 3.58 | 43.1 | 0 | -3.60  | 1 | no (sol too low) |
| z002015 | 4.14 | 23.6 | 0 | -3.82  | 1 | no (sol too low) |

## Key finding
For the BBB-favorable picks (all 8 returned bbb=1), SOLUBILITY is the discriminator:
- Basic amine salts (z003001, z002746, z001999): logS >= -1.5 (too soluble).
- Rigid neutrals clogP ~1.8-3.0 (z002170, z002040, z002052): logS in [-2.79, -2.34] (qualify).
- clogP >= 3.5 (z002003, z002015): logS <= -3.6 (too insoluble).

Window closes cleanly at both ends -> no borderline gap exploited.

## Unassayed 12 — outside window, confidently excluded
- z000364: mw399, TPSA83, HBA6, HBD1, clogP2.87 — BBB fails (TPSA/MW/HBD), not in qual shape. & caffeine-like multi-OMe too soluble.
- z000889: clogP8.84 — sol fail.
- z001510: caffeine (logS~+0.5), too soluble; BBB borderline anyway.
- z001543: sugar (logS~+0.5), no BBB.
- z002010: clogP4.28 tertiary amine -> too soluble (amine) OR too insoluble (clogP); clogP4.28 consistent w/ z002015's fail at 4.14. Excluded either way.
- z002011: clogP4.32 (cf z002015 fail) -> sol fail.
- z002216: TPSA98, HBD2, sulfonamide -> no BBB; also too soluble.
- z002683: TPSA100, HBD2, nitro -> no BBB; too soluble.
- z002730: TPSA83, glutarimide -> no BBB; too soluble.
- z002764: TPSA67, HBD2, clogP3.56 -> BBB marginal & HBD2 hurts; if BBB+ sol likely <=-3.6 (cf z002003 fail). Excluded.
- z002891: steroid clogP4.72 -> sol fail.
- z015334: TPSA82, clogP0.74 -> no BBB; too soluble.

## Answer
qualifying_ids = [z002040, z002170, z002052]
