# ADMET Triage Notes

## Task
- 20 compounds in `data/candidates.tsv`. Budget: 8 `measure_admet` assays.
- Qualifier = measured `logS` in [-3, -2] inclusive AND measured `bbb` = 1.
- Must confirm every reported qualifier with a real assay call.

## Strategy
Catalogue has no solubility/BBB data, only cheap descriptors. Use descriptors to triage:
- **BBB**: favor low TPSA (<~70), low HBD (≤1), MW < ~450, moderate clogp.
- **logS in [-3,-2]**: rough rule logS ≈ -1 - clogp (GSE-ish), so clogp ~1-2.5 is the sweet spot.

## Descriptor triage of all 20
| mol_id | clogp | tpsa | hbd | mw | verdict |
|--------|-------|------|-----|----|---------|
| z000364 | 2.87 | 83.1 | 1 | 399 | TPSA too high → BBB fail |
| z000889 | 8.84 | 29.5 | 1 | 431 | clogp huge → logS << -3 |
| z001510 | -1.04 | 72.7 | 1 | 180 | clogp<0 → logS > -2; TPSA high |
| z001543 | -1.64 | 119.6 | 5 | 286 | too soluble + TPSA huge |
| z001999 | 1.5 | 41.9 | 1 | 299 | **promising** |
| z002003 | 3.58 | 43.1 | 0 | 309 | logS borderline low |
| z002010 | 4.28 | 29.5 | 0 | 340 | logS too low |
| z002011 | 4.32 | 30.2 | 0 | 326 | logS too low |
| z002015 | 4.14 | 23.6 | 0 | 336 | logS too low |
| z002040 | 1.77 | 46.2 | 1 | 217 | **promising** |
| z002052 | 1.99 | 65.1 | 0 | 369 | BBB borderline (TPSA) |
| z002170 | 3.0 | 34.9 | 0 | 250 | logS borderline low |
| z002216 | 0.86 | 98.0 | 2 | 250 | TPSA huge → BBB fail |
| z002683 | 2.55 | 100.7 | 2 | 273 | TPSA huge → BBB fail |
| z002730 | 0.09 | 83.6 | 1 | 258 | TPSA high → BBB fail |
| z002746 | 1.55 | 30.2 | 0 | 231 | **promising** |
| z002764 | 3.56 | 66.8 | 2 | 318 | both borderline |
| z002891 | 4.72 | 34.1 | 0 | 314 | logS too low |
| z003001 | 2.9 | 29.1 | 1 | 238 | logS borderline low |
| z015334 | 0.74 | 81.6 | 1 | 306 | TPSA high → BBB fail |

## 8 to assay (most likely to satisfy BOTH)
1. z001999 (clogp 1.5, tpsa 41.9, hbd 1)
2. z002746 (clogp 1.55, tpsa 30.2, hbd 0)
3. z002040 (clogp 1.77, tpsa 46.2, hbd 1)
4. z002052 (clogp 1.99, tpsa 65.1, hbd 0) — BBB borderline
5. z002170 (clogp 3.0, tpsa 34.9, hbd 0) — logS borderline
6. z003001 (clogp 2.9, tpsa 29.1, hbd 1) — logS borderline
7. z002003 (clogp 3.58, tpsa 43.1, hbd 0) — logS likely low
8. z002764 (clogp 3.56, tpsa 66.8, hbd 2) — both borderline

## Assay results (budget 8/8 used)

| mol_id | logS | bbb | logS∈[-3,-2]? | bbb=1? | qualifies? |
|--------|-------|-----|---------------|--------|------------|
| z001999 | -1.522 | 1 | no (>-2) | yes | NO |
| z002746 | -0.6286 | 1 | no (>-2) | yes | NO |
| z002040 | -2.3374 | 1 | **yes** | **yes** | **YES** |
| z002052 | -2.7894 | 1 | **yes** | **yes** | **YES** |
| z002170 | -2.92 | 1 | **yes** | **yes** | **YES** |
| z003001 | -0.137 | 1 | no (>-2) | yes | NO |
| z002003 | -3.6 | 1 | no (<-3) | yes | NO |
| z002764 | -2.9008 | 0 | yes | no | NO |

## Conclusion
Qualifying compounds (both logS∈[-3,-2] AND bbb=1): **z002040, z002052, z002170**

Budget exhausted; remaining 12 compounds not assayed, so not reported per the rule
that every reported qualifier must be confirmed by a real assay call.
