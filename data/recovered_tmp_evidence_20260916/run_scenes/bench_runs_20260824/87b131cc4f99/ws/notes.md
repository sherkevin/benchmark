# Notes

1. **Objective**: Identify compounds from `data/candidates.tsv` that meet both criteria:
   - Measured `logS` between -3 and -2 (inclusive).
   - Measured `bbb` = 1 (indicating brain penetration).

2. **Constraints**:
   - Only 8 assays (`measure_admet` calls) can be performed due to budget constraints.
   - Compounds must be confirmed via `measure_admet` before inclusion in `answer.json`.

3. **Strategy**:
   - Prioritize compounds with descriptors (`clogp`, `tpsa`, `hbd`, `hba`) that are more likely to meet the target profile.
   - Focus on compounds with `clogp` between 1 and 4 (moderate lipophilicity) and `tpsa` < 80 (lower polar surface area is favorable for brain penetration).

4. **Initial Candidates**:
   - `z000364`: `clogp` = 2.87, `tpsa` = 83.1
   - `z001999`: `clogp` = 1.5, `tpsa` = 41.9
   - `z002003`: `clogp` = 3.58, `tpsa` = 43.1
   - `z002010`: `clogp` = 4.28, `tpsa` = 29.5
   - `z002040`: `clogp` = 1.77, `tpsa` = 46.2
   - `z002052`: `clogp` = 1.99, `tpsa` = 65.1
   - `z002170`: `clogp` = 3.0, `tpsa` = 34.9
   - `z002216`: `clogp` = 0.86, `tpsa` = 98.0

5. **Assay Results**:
   - `z001999`: `logS` = -1.522, `bbb` = 1 (Does not meet `logS` criteria)
   - `z002003`: `logS` = -3.6, `bbb` = 1 (Does not meet `logS` criteria)
   - `z002010`: `logS` = -5.0097, `bbb` = 1 (Does not meet `logS` criteria)
   - `z002040`: `logS` = -2.3374, `bbb` = 1 (Qualifies)
   - `z002052`: `logS` = -2.7894, `bbb` = 1 (Qualifies)
   - `z002170`: `logS` = -2.92, `bbb` = 1 (Qualifies)
   - `z002216`: `logS` = -2.16, `bbb` = 1 (Qualifies)
   - `z000364`: `logS` = -0.9482, `bbb` = 0 (Does not meet criteria)

6. **Qualifying Compounds**:
   - `z002040`
   - `z002052`
   - `z002170`
   - `z002216`