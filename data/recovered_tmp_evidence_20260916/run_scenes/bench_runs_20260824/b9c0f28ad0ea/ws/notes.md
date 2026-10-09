# Notes

## Task Overview
- **Objective**: Identify compounds from `data/candidates.tsv` that meet both criteria:
  1. Measured `logS` between -3 and -2 (inclusive).
  2. Measured `bbb` = 1 (brain penetration).
- **Budget**: 8 assays (`measure_admet` calls) in total.

## Strategy
1. **Initial Filtering**: Use cheap descriptors (`clogp`, `tpsa`, `hbd`, `hba`) to prioritize compounds likely to meet the target profile.
   - `logS` range (-3 to -2) suggests moderate solubility, often associated with moderate `clogp` (e.g., 1-3).
   - `bbb` = 1 suggests compounds with lower `tpsa` (e.g., < 80) and moderate `clogp`.
2. **Prioritization**: Rank compounds based on descriptor values and select top candidates for assay.
3. **Assay Execution**: Use `measure_admet` to test prioritized compounds until the budget is exhausted.
4. **Validation**: Ensure each reported compound in `answer.json` has been confirmed via `measure_admet`.

## Initial Observations
- Compounds with `clogp` between 1 and 3 and `tpsa` < 80 are strong candidates.
- Examples: `z000364`, `z001999`, `z002052`, `z002170`.

## Next Steps
1. Select the top 8 candidates based on descriptors.
2. Run `measure_admet` on each and record results.
3. Update `answer.json` with qualifying IDs.
