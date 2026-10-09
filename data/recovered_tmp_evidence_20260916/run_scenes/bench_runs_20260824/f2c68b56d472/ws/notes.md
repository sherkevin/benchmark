# Notes - Compound Screening Task

## Task Summary
- 20 compounds to evaluate
- Budget: 8 assays (measure_admet calls)
- Qualifying criteria: logS between -3 and -2 (inclusive) AND bbb = 1

## Strategy
Based on medicinal chemistry principles:
- BBB penetration favored by: TPSA < 90, moderate clogP (2-4), low HBD
- Solubility logS -3 to -2: moderate lipophilicity, not too high/low clogP

## Compounds Assayed

| mol_id | clogP | TPSA | HBD | logS | bbb | Qualifies? |
|--------|-------|------|-----|------|-----|------------|
| z001999 | 1.5 | 41.9 | 1 | -1.522 | 1 | No (logS > -2) |
| z002003 | 3.58 | 43.1 | 0 | -3.6 | 1 | No (logS < -3) |
| z002010 | 4.28 | 29.5 | 0 | -5.01 | 1 | No (logS < -3) |
| z002011 | 4.32 | 30.2 | 0 | -4.13 | 1 | No (logS < -3) |
| z002015 | 4.14 | 23.6 | 0 | -3.82 | 1 | No (logS < -3) |
| z002170 | 3.0 | 34.9 | 0 | -2.92 | 1 | **YES** |
| z002746 | 1.55 | 30.2 | 0 | -0.63 | 1 | No (logS > -2) |
| z003001 | 2.9 | 29.1 | 1 | -0.14 | 1 | No (logS > -2) |

## Assay Log (in order)

1. z001999: logS=-1.522, bbb=1 → No (logS too high)
2. z002003: logS=-3.6, bbb=1 → No (logS too low)
3. z002010: logS=-5.01, bbb=1 → No (logS too low)
4. z002011: logS=-4.13, bbb=1 → No (logS too low)
5. z002015: logS=-3.82, bbb=1 → No (logS too low)
6. z002170: logS=-2.92, bbb=1 → **QUALIFIES**
7. z002746: logS=-0.63, bbb=1 → No (logS too high)
8. z003001: logS=-0.14, bbb=1 → No (logS too high)

## Summary
- Total assays used: 8/8
- Qualifying compounds: 1 (z002170) 
