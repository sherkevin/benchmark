# Notes

## Task Overview
- We need to identify compounds from `data/candidates.tsv` that meet the following criteria:
  1. Measured `logS` between -3 and -2 inclusive.
  2. Measured `bbb` = 1 (indicating brain penetration).
- We have a budget of 8 assays (`measure_admet` calls) to test these compounds.

## Strategy
1. **Initial Filtering**: Use cheap descriptors (`clogp`, `tpsa`, `hbd`, `hba`, `rotb`, `rings`) to prioritize compounds likely to meet the target profile.
   - Compounds with `clogp` between 1 and 4 are more likely to have `logS` in the desired range.
   - Compounds with `tpsa` < 80 Å² are more likely to have `bbb` = 1.
   - Compounds with `hbd` ≤ 2 and `hba` ≤ 5 are more likely to have good solubility and brain penetration.

2. **Prioritization**: Rank compounds based on these descriptors and select the top candidates for testing.

3. **Testing**: Use `measure_admet` to assay the prioritized compounds and record results.

## Initial Observations
- The dataset contains 20 compounds.
- We will prioritize compounds with `clogp` between 1 and 4 and `tpsa` < 80 Å².

## Next Steps
1. Identify the top 8 candidates based on the initial filtering criteria.
2. Run `measure_admet` on these candidates and record results.
3. Update `answer.json` with qualifying IDs.
