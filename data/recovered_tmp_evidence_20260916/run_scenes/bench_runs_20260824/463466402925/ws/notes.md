# Notes

## Task
Find compounds with logS between -3 and -2 (inclusive) AND bbb=1 from 20 candidates.
Budget: 8 assays.

## Analysis of all 20 compounds

| mol_id | MW | clogp | TPSA | HBD | Assessment |
|--------|------|-------|-------|-----|------------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | TPSA borderline for BBB |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | Too lipophilic → logS too low |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | Too hydrophilic → logS too high |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | Too hydrophilic, high TPSA → no BBB |
| z001999 | 299.4 | 1.50 | 41.9 | 1 | Good BBB profile, solubility might be high |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | Good BBB, solubility borderline |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | High clogp, might be too insoluble |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | High clogp, might be too insoluble |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | High clogp, might be too insoluble |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | Good balance |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | Decent for both |
| z002170 | 250.3 | 3.00 | 34.9 | 0 | Strong candidate |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | High TPSA → no BBB |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | High TPSA → no BBB |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | Low clogp → too soluble |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | Good BBB, solubility might be high |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | Decent for both |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | High clogp, might be too insoluble |
| z003001 | 237.7 | 2.90 | 29.1 | 1 | Strong candidate |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | Low clogp, borderline TPSA |

## Selection strategy
Prioritize: clogp 1.5-3.5, TPSA < 70, low HBD → best chance for BOTH criteria.

## 8 compounds to assay:
1. z003001 (clogp=2.9, TPSA=29.1) - Strong candidate
2. z002170 (clogp=3.0, TPSA=34.9) - Strong candidate  
3. z002746 (clogp=1.55, TPSA=30.2) - Good BBB
4. z002040 (clogp=1.77, TPSA=46.2) - Good balance
5. z001999 (clogp=1.5, TPSA=41.9) - Good BBB
6. z002052 (clogp=1.99, TPSA=65.1) - Decent for both
7. z002003 (clogp=3.58, TPSA=43.1) - Good BBB, borderline solubility
8. z002764 (clogp=3.56, TPSA=66.8) - Decent for both

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z003001 | -0.137 | 1 | NO - logS too high |
| z002170 | -2.92 | 1 | YES ✓ |
| z002746 | -0.629 | 1 | NO - logS too high |
| z002040 | -2.337 | 1 | YES ✓ |
| z001999 | -1.522 | 1 | NO - logS too high |
| z002052 | -2.789 | 1 | YES ✓ |
| z002003 | -3.6 | 1 | NO - logS too low |
| z002764 | -2.901 | 0 | NO - bbb=0 |

## Qualifying compounds (confirmed by assay):
- z002170 (logS=-2.92, bbb=1)
- z002040 (logS=-2.3374, bbb=1)
- z002052 (logS=-2.7894, bbb=1)

## Untested compounds analysis:
- z000889 (clogp=8.84): certainly too insoluble
- z001510 (clogp=-1.04): certainly too soluble
- z001543 (clogp=-1.64, TPSA=119.6): too soluble + no BBB
- z002010 (clogp=4.28): likely too insoluble (z002003 at clogp=3.58 already had logS=-3.6)
- z002011 (clogp=4.32): likely too insoluble
- z002015 (clogp=4.14): likely too insoluble
- z002216 (clogp=0.86, TPSA=98): too soluble + no BBB
- z002683 (clogp=2.55, TPSA=100.7): high TPSA → no BBB
- z002730 (clogp=0.09): too soluble
- z002891 (clogp=4.72): likely too insoluble
- z015334 (clogp=0.74, TPSA=81.6): likely too soluble
- z000364 (clogp=2.87, TPSA=83.1): uncertain but budget exhausted

Budget used: 8/8
