# Notes

1. **Task Overview**: We need to identify compounds from `data/candidates.tsv` that meet the following criteria:
   - Measured `logS` between -3 and -2 (inclusive).
   - Measured `bbb` = 1 (indicating brain penetration).

2. **Budget**: We have a budget of 8 assays (`measure_admet` calls).

3. **Strategy**:
   - Prioritize compounds with descriptors that suggest potential solubility and brain penetration.
   - Use the budget efficiently to test compounds most likely to qualify.

4. **Initial Observations**:
   - Compounds with lower `clogp` (logP) values might have better solubility.
   - Compounds with moderate `tpsa` (Topological Polar Surface Area) and fewer `rotb` (rotatable bonds) might have better brain penetration.

5. **Assay Results**:
   - **z000364**: `logS` = -0.9482, `bbb` = 0 → Does not qualify.
   - **z001510**: `logS` = -2.7372, `bbb` = 1 → Qualifies.
   - **z001543**: `logS` = -0.8547, `bbb` = 1 → Does not qualify.
   - **z002003**: `logS` = -3.6, `bbb` = 1 → Does not qualify (logS too low).
   - **z002010**: `logS` = -5.0097, `bbb` = 1 → Does not qualify (logS too low).
   - **z002015**: `logS` = -3.82, `bbb` = 1 → Does not qualify (logS too low).
   - **z002040**: `logS` = -2.3374, `bbb` = 1 → Qualifies.
   - **z002216**: `logS` = -2.16, `bbb` = 1 → Qualifies.

6. **Qualifying Compounds**:
   - `z001510`
   - `z002040`
   - `z002216`