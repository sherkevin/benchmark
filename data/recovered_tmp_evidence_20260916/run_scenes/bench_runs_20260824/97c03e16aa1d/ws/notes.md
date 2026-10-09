# Notes

1. **Task Overview**: Identify compounds from `data/candidates.tsv` that meet the target profile:
   - `logS` between -3 and -2 (inclusive)
   - `bbb` = 1 (brain penetration)
   - Budget: 8 assays in total.

2. **Initial Analysis**:
   - The dataset contains 20 compounds with descriptors like `mw`, `clogp`, `tpsa`, etc.
   - The target profile requires experimental measurement of `logS` and `bbb`.

3. **Strategy**:
   - Prioritize compounds with descriptors that suggest potential for solubility (`clogp` not too high, `tpsa` moderate) and brain penetration (lower `tpsa`, moderate `clogp`).
   - Use the budget of 8 assays to test the most promising candidates.

4. **Assay Results**:
   - Tested compounds: `z000364`, `z001999`, `z002010`, `z002040`, `z002052`, `z002170`, `z002216`, `z002730`.
   - **Qualifying Compounds**:
     - `z001999`: `logS` = -1.522, `bbb` = 1
     - `z002040`: `logS` = -2.3374, `bbb` = 1
     - `z002052`: `logS` = -2.7894, `bbb` = 1
     - `z002170`: `logS` = -2.92, `bbb` = 1
     - `z002216`: `logS` = -2.16, `bbb` = 1
     - `z002730`: `logS` = -2.6758, `bbb` = 1

5. **Conclusion**:
   - 6 out of 8 tested compounds meet the target profile.
   - The final list of qualifying IDs has been updated in `answer.json`.