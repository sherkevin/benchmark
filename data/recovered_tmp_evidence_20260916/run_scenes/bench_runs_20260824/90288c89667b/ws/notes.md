# Notes — ADMET Triage

## Target profile
- Measured `logS` in [-3, -2] (inclusive)
- Measured `bbb` = 1
- Budget: 8 assays for 20 compounds → must prioritize.

## Heuristic reasoning
- **logS (aqueous solubility):** lower clogP → higher solubility. To hit logS ≈ -2.5 need moderate clogP (roughly 1–3) and moderate MW. Very high clogP (>4) → logS too negative (<-3). Very low clogP (<0) → logS likely > -2 (too soluble) OR still depends on MW/tpsa.
- **BBB:** requires low polarity: tpsa ideally <~70, hbd ≤ 2, moderate lipophilicity (clogP ~1–4). High tpsa (>90) or high hbd (>3) → poor BBB.

## Candidate scoring
| mol_id | mw | clogp | tpsa | hbd | hba | rings | notes |
|---|---|---|---|---|---|---|---|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | 3 | tpsa high → BBB marginal; MW high |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | 2 | clogP way too high → logS very negative |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | 2 | xanthine-derivative-ish; low clogP → logS maybe > -2? tpsa borderline |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | 2 | tpsa huge + 5 HBD → no BBB |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | 5 | GOOD: clogP ok, tpsa low, hbd 1, MW ok → top candidate |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | 4 | GOOD: tpsa low, hbd 0, clogP ok → top candidate |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | 2 | clogP high → logS likely too negative |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | 4 | clogP high → logS likely too negative |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | 3 | clogP high → logS likely too negative |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | 2 | GOOD: low MW, clogP ok, tpsa ok, hbd 1 → top candidate |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | 5 | tpsa borderline, MW moderate; possible |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | 3 | GOOD: low MW, clogP ok, tpsa low, hbd 0 → top candidate |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | 2 | tpsa high → BBB marginal |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | 2 | tpsa high → no BBB |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | 3 | tpsa borderline; clogP low → logS maybe > -2 |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | 2 | GOOD: low MW, clogP ok, tpsa low, hbd 0 → top candidate |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | 4 | tpsa borderline, hbd 2; possible |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | 4 | clogP high → logS likely too negative |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | 2 | GOOD: low MW, clogP ok, tpsa low, hbd 1 → top candidate |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | 3 | tpsa borderline; clogP low → logS maybe > -2 |

## Top picks (best BBB + plausible logS in range)
1. z001999 — clogP 1.5, tpsa 41.9, hbd 1, MW 299
2. z002003 — clogP 3.58, tpsa 43.1, hbd 0, MW 309
3. z002040 — clogP 1.77, tpsa 46.2, hbd 1, MW 217
4. z002170 — clogP 3.0, tpsa 34.9, hbd 0, MW 250
5. z002746 — clogP 1.55, tpsa 30.2, hbd 0, MW 231
6. z003001 — clogP 2.9, tpsa 29.1, hbd 1, MW 238
7. z002052 — clogP 1.99, tpsa 65.1, hbd 0, MW 369
8. z002764 — clogP 3.56, tpsa 66.8, hbd 2, MW 318

These 8 best fit dual criteria; spend budget on them.

## Assay results (8/8 budget used)
| mol_id | logS | bbb | logS in [-3,-2]? | bbb=1? | Qualifies? |
|---|---|---|---|---|---|
| z001999 | -1.522 | 1 | No (> -2) | Yes | No |
| z002003 | -3.6 | 1 | No (< -3) | Yes | No |
| z002040 | -2.3374 | 1 | Yes | Yes | **YES** |
| z002170 | -2.92 | 1 | Yes | Yes | **YES** |
| z002746 | -0.6286 | 1 | No (> -2) | Yes | No |
| z003001 | -0.137 | 1 | No (> -2) | Yes | No |
| z002052 | -2.7894 | 1 | Yes | Yes | **YES** |
| z002764 | -2.9008 | 0 | Yes | No | No |

## Qualifying compounds
- z002040 — logS -2.3374, bbb 1
- z002170 — logS -2.92, bbb 1
- z002052 — logS -2.7894, bbb 1

## Observations / lessons
- Several low-clogP compounds (z002746, z003001, z001999) had logS > -2 (too soluble) despite favorable BBB descriptors — clogP alone under-predicted solubility drop.
- z002003 (clogP 3.58) had logS -3.6 (too insoluble) — high clogP pushed solubility below range.
- The two diacetate/sugar-like compounds (z002052) and the small glutarimide (z002040) hit the window; the methylindolinone (z002170) too.
- Budget exhausted at 8; no assays left to probe remaining 12 candidates (none had stronger dual-criterion priors than the chosen 8).
