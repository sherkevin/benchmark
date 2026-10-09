# Notes

## Task
Triaging 20 compounds to find those qualifying for:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 ADMET assays

## Strategy
Prioritized compounds with descriptors suggesting good BBB penetration (TPSA < 90, low HBD, clogP 2-5) and moderate solubility (clogP around 2-4).

## Assays Performed (8 total)

| mol_id | clogP | TPSA | HBD | logS | bbb | Qualifies? |
|--------|-------|------|-----|------|-----|------------|
| z001999 | 1.5 | 41.9 | 1 | -1.522 | 1 | No (logS too high) |
| z002003 | 3.58 | 43.1 | 0 | -3.6 | 1 | No (logS too low) |
| z002010 | 4.28 | 29.5 | 0 | -5.0097 | 1 | No (logS too low) |
| z002011 | 4.32 | 30.2 | 0 | -4.13 | 1 | No (logS too low) |
| z002015 | 4.14 | 23.6 | 0 | -3.82 | 1 | No (logS too low) |
| z002170 | 3.0 | 34.9 | 0 | -2.92 | 1 | **YES** |
| z002746 | 1.55 | 30.2 | 0 | -0.6286 | 1 | No (logS too high) |
| z003001 | 2.9 | 29.1 | 1 | -0.137 | 1 | No (logS too high) |

## Result
Only **z002170** qualifies: logS = -2.92 (within -3 to -2 range), bbb = 1.
