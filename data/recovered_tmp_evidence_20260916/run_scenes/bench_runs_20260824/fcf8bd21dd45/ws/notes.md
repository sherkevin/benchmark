# Notes

## Task
Find compounds with measured logS between -3 and -2 (inclusive) AND bbb = 1.
Budget: 8 assays out of 20 compounds.

## Strategy
Using descriptors to prioritize:
- **BBB penetration**: needs low TPSA (<70, ideally <60), moderate clogP (1-5), MW < 400, low HBD
- **logS -3 to -2**: needs moderate lipophilicity — not too high clogP (would be too insoluble) and not too low (too soluble). Sweet spot: clogP ~2-4 with MW ~230-350.

## Eliminated (unlikely to qualify):
- z000889: clogP=8.84 → far too insoluble (logS << -3)
- z001510: clogP=-1.04, TPSA=72.7 → too soluble, poor BBB
- z001543: clogP=-1.64, TPSA=119.6 → too soluble, poor BBB
- z002216: TPSA=98.0 → poor BBB
- z002683: TPSA=100.7 → poor BBB
- z002730: clogP=0.09, TPSA=83.6 → too soluble and poor BBB
- z015334: clogP=0.74, TPSA=81.6 → likely too soluble, borderline BBB

## Selected 8 for assay (best combined profile):
1. z002170: clogP=3.0, TPSA=34.9, MW=250.3 — TOP PICK
2. z003001: clogP=2.9, TPSA=29.1, MW=237.7 — TOP PICK
3. z002003: clogP=3.58, TPSA=43.1, MW=308.8 — good
4. z002764: clogP=3.56, TPSA=66.8, MW=318.3 — good solubility profile
5. z002746: clogP=1.55, TPSA=30.2, MW=231.3 — great BBB, might be too soluble
6. z002040: clogP=1.77, TPSA=46.2, MW=217.3 — decent
7. z001999: clogP=1.5, TPSA=41.9, MW=299.4 — decent
8. z002011: clogP=4.32, TPSA=30.2, MW=325.8 — great BBB, borderline solubility

## Results
| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|-----------|
| z002170 | -2.92 | 1 | ✅ YES |
| z003001 | -0.137 | 1 | ❌ (too soluble) |
| z002003 | -3.6 | 1 | ❌ (too insoluble) |
| z002764 | -2.9008 | 0 | ❌ (no BBB) |
| z002746 | -0.6286 | 1 | ❌ (too soluble) |
| z002040 | -2.3374 | 1 | ✅ YES |
| z001999 | -1.522 | 1 | ❌ (too soluble) |
| z002011 | -4.13 | 1 | ❌ (too insoluble) |

## Qualifying compounds: z002170, z002040

## Observations
- Sweet spot for logS in [-3,-2] was clogP ~1.8-3.0 with moderate MW (~220-250)
- High clogP (4+) → too insoluble (z002011 at 4.32 → logS=-4.13)
- Low clogP (1.5-2.9) → often too soluble (z003001 at 2.9 → logS=-0.14)
- z002764 had good logS but failed BBB (TPSA=66.8 was borderline)
- Budget exhausted; remaining untested compounds mostly had profiles suggesting they wouldn't qualify (too high/low clogP or too high TPSA for BBB)
