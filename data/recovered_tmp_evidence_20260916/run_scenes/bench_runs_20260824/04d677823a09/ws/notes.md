# Notes for Compound Triage Task

## Overview
- **Task**: Identify compounds from `data/candidates.tsv` that meet the target profile:
  - Measured `logS` between -3 and -2 (inclusive).
  - Measured `bbb` = 1 (brain penetration).
- **Budget**: 8 assays in total.

## Strategy
1. **Initial Filtering**: Use cheap descriptors (`clogp`, `tpsa`, `hbd`, `hba`) to prioritize compounds likely to meet the solubility and brain penetration criteria.
2. **Assay Selection**: Allocate the budget to assay the most promising candidates.
3. **Validation**: Confirm each candidate's `logS` and `bbb` via `measure_admet`.

## Observations
- Compounds with lower `clogp` (e.g., < 3) and moderate `tpsa` (e.g., 30-80) are more likely to have favorable solubility and brain penetration.
- Compounds with high `hbd` (> 2) may have poor brain penetration.

## Results
- **z000364**: `logS` = -0.9482, `bbb` = 0 → Does not qualify.
- **z001543**: `logS` = -0.8547, `bbb` = 1 → Does not qualify (solubility out of range).
- **z002216**: `logS` = -2.16, `bbb` = 1 → **Qualifies**.
- **z002683**: `logS` = -4.17, `bbb` = 0 → Does not qualify.
- **z002730**: `logS` = -2.6758, `bbb` = 1 → **Qualifies**.
- **z002764**: `logS` = -2.9008, `bbb` = 0 → Does not qualify.
- **z015334**: `logS` = -1.8, `bbb` = 1 → Does not qualify (solubility out of range).
- **z001510**: `logS` = -2.7372, `bbb` = 1 → **Qualifies**.

## Final Qualifying Compounds
- `z002216`
- `z002730`
- `z001510`
