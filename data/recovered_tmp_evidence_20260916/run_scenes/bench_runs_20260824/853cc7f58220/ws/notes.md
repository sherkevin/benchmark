# Notes — Compound Triaging Task

## Strategy
With 20 candidates and only 8 assays, I prioritized compounds most likely to satisfy BOTH:
- logS between -3 and -2 (moderate aqueous solubility)
- bbb = 1 (brain penetration)

I used descriptors to estimate: moderate clogp (~2-4) suggests moderate logS, while low TPSA (<70) and low HBD suggest BBB penetration.

## Assay Results (8/8 used)

| # | mol_id | clogp | TPSA | logS | bbb | Qualifies? |
|---|--------|-------|------|------|-----|------------|
| 1 | z003001 | 2.9 | 29.1 | -0.137 | 1 | No — logS too high (> -2) |
| 2 | z002170 | 3.0 | 34.9 | -2.92 | 1 | **YES** |
| 3 | z002746 | 1.55 | 30.2 | -0.629 | 1 | No — logS too high |
| 4 | z001999 | 1.5 | 41.9 | -1.522 | 1 | No — logS too high |
| 5 | z002003 | 3.58 | 43.1 | -3.60 | 1 | No — logS too low (< -3) |
| 6 | z002015 | 4.14 | 23.6 | -3.82 | 1 | No — logS too low |
| 7 | z002010 | 4.28 | 29.5 | -5.01 | 1 | No — logS too low |
| 8 | z002011 | 4.32 | 30.2 | -4.13 | 1 | No — logS too low |

## Observations
- All 8 tested compounds had bbb=1, confirming that the moderate-MW, low-TPSA selection criteria worked well for BBB penetration.
- The logS sweet spot (-3 to -2) was narrow: compounds with clogp ~1.5-2.9 had logS > -2 (too soluble), while compounds with clogp > 3.5 had logS < -3 (too insoluble).
- z002170 (clogp=3.0) was the only compound landing in the target logS window.

## Untested Compounds (12 remaining)
The remaining 12 compounds were not assayed. Based on descriptors:
- Very low clogp compounds (z001510, z001543, z015334, z002216, z002730): likely too soluble and unlikely to cross BBB.
- Very high clogp compounds (z000889, z002891): likely too insoluble.
- Moderate clogp but high TPSA compounds (z000364, z002683): BBB penetration uncertain.
- z002040 (clogp=1.77), z002052 (clogp=1.99): likely too soluble based on trend from tested compounds.
- z002764 (clogp=3.56, TPSA=66.8, HBD=2): might have acceptable logS but BBB uncertain due to HBD.

## Conclusion
Only **z002170** was confirmed to meet both criteria.
