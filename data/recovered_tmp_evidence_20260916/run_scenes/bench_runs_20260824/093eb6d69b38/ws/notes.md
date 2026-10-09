# Notes

1. **Task Overview**: We need to identify compounds from `data/candidates.tsv` that meet the target profile:
   - `logS` between -3 and -2 (inclusive)
   - `bbb` = 1 (brain penetration)

2. **Budget**: We have 8 assays (`measure_admet`) to spend. Each assay provides `logS` and `bbb` for one compound.

3. **Strategy**:
   - Prioritize compounds likely to meet the criteria based on descriptors (e.g., `clogp`, `tpsa`, `hbd`, `hba`).
   - Use assays on compounds with moderate `clogp` (2-5) and reasonable `tpsa` (30-100) as they are more likely to have balanced solubility and brain penetration.

4. **Initial Observations**:
   - Compounds with high `clogp` (e.g., `z000889`) are less likely to have good solubility.
   - Compounds with low `clogp` (e.g., `z001510`) might lack brain penetration.
   - Compounds with moderate `clogp` and reasonable `tpsa` (e.g., `z000364`, `z002003`) are good candidates for testing.

5. **Testing Plan**:
   - First, test compounds with `clogp` between 2 and 5 and `tpsa` between 30 and 100.
   - If budget allows, test outliers with potential.

6. **Assay Results**:
   - `z000364`: `logS` = -0.9482, `bbb` = 0 (Does not qualify)
   - `z002003`: `logS` = -3.6, `bbb` = 1 (Does not qualify, `logS` too low)
   - `z002010`: `logS` = -5.0097, `bbb` = 1 (Does not qualify, `logS` too low)
   - `z002011`: `logS` = -4.13, `bbb` = 1 (Does not qualify, `logS` too low)
   - `z002015`: `logS` = -3.82, `bbb` = 1 (Does not qualify, `logS` too low)
   - `z002052`: `logS` = -2.7894, `bbb` = 1 (Qualifies)
   - `z002170`: `logS` = -2.92, `bbb` = 1 (Qualifies)
   - `z002216`: `logS` = -2.16, `bbb` = 1 (Does not qualify, `logS` too high)

7. **Qualifying Compounds**:
   - `z002052`: `logS` = -2.7894, `bbb` = 1
   - `z002170`: `logS` = -2.92, `bbb` = 1