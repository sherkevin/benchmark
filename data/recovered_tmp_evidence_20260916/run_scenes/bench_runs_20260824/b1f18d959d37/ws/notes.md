# Notes for Compound Triage Task

## Approach
1. **Initial Analysis**: Review the compounds in `data/candidates.tsv` and identify potential candidates based on descriptors that correlate with solubility and brain penetration.
2. **Prioritization**: Prioritize compounds with descriptors that suggest they might fall within the target solubility range (-3 to -2 logS) and have potential for brain penetration (bbb = 1).
3. **Assay Execution**: Use the `measure_admet` tool to assay the prioritized compounds, staying within the budget of 8 assays.
4. **Validation**: Confirm which compounds meet the target profile and update `answer.json` accordingly.

## Observations
- The task requires both measured `logS` between -3 and -2 and `bbb = 1`.
- The budget is limited to 8 assays, so prioritization is critical.

## Planned Assays
- Compounds with `clogp` between 1 and 4 (moderate lipophilicity) and `tpsa` (topological polar surface area) below 80 Å² (better brain penetration potential) will be prioritized.

## Results
1. `z000364`: logS = -0.9482, bbb = 0 (Does not qualify)
2. `z001999`: logS = -1.522, bbb = 1 (Does not qualify; logS too high)
3. `z002010`: logS = -5.0097, bbb = 1 (Does not qualify; logS too low)
4. `z002040`: logS = -2.3374, bbb = 1 (Qualifies)
5. `z002052`: logS = -2.7894, bbb = 1 (Qualifies)
6. `z002170`: logS = -2.92, bbb = 1 (Qualifies)
7. `z002015`: logS = -3.82, bbb = 1 (Does not qualify; logS too low)
8. `z003001`: logS = -0.137, bbb = 1 (Does not qualify; logS too high)

## Final Answer
- Qualifying compounds: `z002040`, `z002052`, `z002170`
