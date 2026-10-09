# Notes

## Initial Observations
1. The task is to identify compounds from `data/candidates.tsv` that meet the following criteria:
   - Measured `logS` between -3 and -2 inclusive.
   - Measured `bbb` = 1 (indicating brain penetration).
2. There are 20 compounds listed in `data/candidates.tsv`.
3. The budget allows for only 8 assays (`measure_admet` calls).

## Strategy
1. **Prioritize compounds** based on cheap descriptors (`clogp`, `tpsa`, `hbd`, `hba`) to maximize the likelihood of meeting the target profile.
   - Compounds with `clogp` between 2 and 4 are more likely to have `logS` in the desired range.
   - Compounds with `tpsa` < 80 Å² are more likely to have `bbb` = 1.
2. **Filter candidates** based on the above criteria before spending the assay budget.
3. **Run assays** on the prioritized compounds and record results.
4. **Update `answer.json`** with qualifying `mol_id`s.

## Prioritized Compounds
Based on initial filtering, the following compounds are prioritized for assays:
- `z000364`: `clogp` = 2.87, `tpsa` = 83.1
- `z001999`: `clogp` = 1.5, `tpsa` = 41.9
- `z002010`: `clogp` = 4.28, `tpsa` = 29.5
- `z002015`: `clogp` = 4.14, `tpsa` = 23.6
- `z002040`: `clogp` = 1.77, `tpsa` = 46.2
- `z002052`: `clogp` = 1.99, `tpsa` = 65.1
- `z002216`: `clogp` = 0.86, `tpsa` = 98.0
- `z002764`: `clogp` = 3.56, `tpsa` = 66.8

## Assay Results
1. `z000364`: `logS` = -0.9482, `bbb` = 0 → **Does not qualify**.
2. `z001999`: `logS` = -1.522, `bbb` = 1 → **Does not qualify** (`logS` outside range).
3. `z002010`: `logS` = -5.0097, `bbb` = 1 → **Does not qualify** (`logS` outside range).
4. `z002015`: `logS` = -3.82, `bbb` = 1 → **Does not qualify** (`logS` outside range).
5. `z002040`: `logS` = -2.3374, `bbb` = 1 → **Qualifies**.
6. `z002052`: `logS` = -2.7894, `bbb` = 1 → **Qualifies**.
7. `z002216`: `logS` = -2.16, `bbb` = 1 → **Qualifies**.
8. `z002764`: `logS` = -2.9008, `bbb` = 0 → **Does not qualify**.

## Final Qualifying Compounds
- `z002040`
- `z002052`
- `z002216`

## Next Steps
1. Update `answer.json` with the qualifying `mol_id`s.
