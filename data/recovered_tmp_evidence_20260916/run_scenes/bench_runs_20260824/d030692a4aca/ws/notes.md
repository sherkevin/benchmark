# Notes - Compound Triage

## Task
Find compounds with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays

## Strategy
Looking for compounds that might have moderate lipophilicity (clogp ~2-4) and reasonable TPSA for BBB penetration (typically TPSA < 90 for good BBB).

## Compounds Assayed

| mol_id | clogp | TPSA | logS | bbb | Qualifies? |
|--------|-------|------|------|-----|------------|

## Assay Results

1. z002003: logS=-3.6, bbb=1 - NO (logS < -3)
2. z002170: logS=-2.92, bbb=1 - **QUALIFIES**
3. z002764: logS=-2.9008, bbb=0 - NO (bbb=0)
4. z003001: logS=-0.137, bbb=1 - NO (logS > -2)
5. z001999: logS=-1.522, bbb=1 - NO (logS > -2)
6. z002010: logS=-5.0097, bbb=1 - NO (logS < -3)
7. z002011: logS=-4.13, bbb=1 - NO (logS < -3)
8. z002015: logS=-3.82, bbb=1 - NO (logS < -3)

## Summary
Budget: 8/8 assays used
Qualifying compounds: z002170
