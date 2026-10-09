# Notes

1. **Task Overview**: We need to identify compounds from `data/candidates.tsv` that meet the following criteria:
   - Measured `logS` between -3 and -2 (inclusive).
   - Measured `bbb` = 1 (indicating brain penetration).

2. **Budget**: We have a budget of 8 assays (`measure_admet` calls) to test these compounds.

3. **Results**:
   - `z000364`: `logS` = -0.9482, `bbb` = 0 (does not qualify).
   - `z001510`: `logS` = -2.7372, `bbb` = 1 (qualifies).
   - `z001543`: `logS` = -0.8547, `bbb` = 1 (does not qualify).
   - `z002216`: `logS` = -2.16, `bbb` = 1 (qualifies).
   - `z002683`: `logS` = -4.17, `bbb` = 0 (does not qualify).
   - `z002730`: `logS` = -2.6758, `bbb` = 1 (qualifies).
   - `z015334`: `logS` = -1.8, `bbb` = 1 (does not qualify).
   - `z002003`: `logS` = -3.6, `bbb` = 1 (qualifies).

4. **Qualifying Compounds**:
   - `z001510`, `z002216`, `z002730`, `z002003`.

5. **Next Steps**:
   - Update `answer.json` with the qualifying IDs.
