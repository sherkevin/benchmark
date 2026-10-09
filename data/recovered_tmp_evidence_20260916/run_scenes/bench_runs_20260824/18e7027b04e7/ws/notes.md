# Notes

1. **Objective**: Identify compounds from `data/candidates.tsv` that meet both criteria:
   - `logS` between -3 and -2 (inclusive).
   - `bbb` = 1 (brain penetration).

2. **Budget**: 8 assays (`measure_admet` calls).

3. **Strategy**:
   - Prioritize compounds with descriptors suggesting favorable solubility (`clogp` between 1 and 3, `tpsa` > 50) and brain penetration (`mw` < 450, `rotb` < 5).
   - Filter out compounds with extreme `clogp` or high `rotb`.

4. **Initial Filtering**:
   - Exclude compounds with `clogp` < 0 or > 5.
   - Exclude compounds with `mw` > 450.
   - Exclude compounds with `rotb` > 5.

5. **Selected Candidates for Assay**:
   - `z000364`, `z001999`, `z002003`, `z002010`, `z002015`, `z002040`, `z002052`, `z002216`.

6. **Assay Results**:
   - `z000364`: `logS` = -0.9482, `bbb` = 0 → **Does not qualify**.
   - `z001999`: `logS` = -1.522, `bbb` = 1 → **Qualifies**.
   - `z002003`: `logS` = -3.6, `bbb` = 1 → **Does not qualify** (logS too low).
   - `z002010`: `logS` = -5.0097, `bbb` = 1 → **Does not qualify** (logS too low).
   - `z002015`: `logS` = -3.82, `bbb` = 1 → **Does not qualify** (logS too low).
   - `z002040`: `logS` = -2.3374, `bbb` = 1 → **Qualifies**.
   - `z002052`: `logS` = -2.7894, `bbb` = 1 → **Qualifies**.
   - `z002216`: `logS` = -2.16, `bbb` = 1 → **Qualifies**.

7. **Qualifying Compounds**:
   - `z001999`, `z002040`, `z002052`, `z002216`.
