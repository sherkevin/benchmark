# Notes

## Task Overview
- We need to identify compounds from `data/candidates.tsv` that meet the following criteria:
  1. Measured `logS` between -3 and -2 (inclusive).
  2. Measured `bbb` = 1 (indicating brain penetration).
- Budget: 8 assays in total (one assay per compound).

## Approach
1. **Initial Filtering**: Use the cheap descriptors (`mw`, `clogp`, `tpsa`, `hbd`, `hba`, `rotb`, `rings`) to prioritize compounds likely to meet the criteria.
2. **Assay Selection**: Select 8 compounds based on the initial filtering and run `measure_admet` on them.
3. **Validation**: Confirm which compounds meet both criteria and update `answer.json` accordingly.

## Initial Observations
- Compounds with `clogp` between 2 and 4 are more likely to have `logS` in the desired range.
- Compounds with `tpsa` < 80 and `hbd` ≤ 2 are more likely to have `bbb` = 1.

## Selected Compounds for Assay
Based on the above, the following compounds are prioritized for testing:
1. `z000364` (clogp: 2.87, tpsa: 83.1)
2. `z001999` (clogp: 1.5, tpsa: 41.9)
3. `z002003` (clogp: 3.58, tpsa: 43.1)
4. `z002010` (clogp: 4.28, tpsa: 29.5)
5. `z002011` (clogp: 4.32, tpsa: 30.2)
6. `z002015` (clogp: 4.14, tpsa: 23.6)
7. `z002170` (clogp: 3.0, tpsa: 34.9)
8. `z002216` (clogp: 0.86, tpsa: 98.0)

## Results
1. `z000364`: `logS` = -0.9482, `bbb` = 0 (Does not qualify)
2. `z001999`: `logS` = -1.522, `bbb` = 1 (Does not qualify, `logS` out of range)
3. `z002003`: `logS` = -3.6, `bbb` = 1 (Does not qualify, `logS` out of range)
4. `z002010`: `logS` = -5.0097, `bbb` = 1 (Does not qualify, `logS` out of range)
5. `z002011`: `logS` = -4.13, `bbb` = 1 (Does not qualify, `logS` out of range)
6. `z002015`: `logS` = -3.82, `bbb` = 1 (Does not qualify, `logS` out of range)
7. `z002170`: `logS` = -2.92, `bbb` = 1 (Qualifies)
8. `z002216`: `logS` = -2.16, `bbb` = 1 (Qualifies)

## Final Qualifying Compounds
- `z002170`
- `z002216`

## Next Steps
- Update `answer.json` with the qualifying IDs.
