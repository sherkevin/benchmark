# Notes

## Initial Observations
- The task involves identifying compounds from `data/candidates.tsv` that meet two criteria:
  1. Measured `logS` between -3 and -2 (inclusive).
  2. Measured `bbb` = 1 (indicating brain penetration).
- We have a budget of 8 assays in total (one assay per `measure_admet` call).

## Strategy
1. **Prioritize Compounds**: Given the limited budget, we need to prioritize compounds that are more likely to meet the criteria based on their descriptors.
   - **Solubility (`logS`)**: Compounds with lower `clogp` (more hydrophilic) and higher `tpsa` (polar surface area) are more likely to have better solubility.
   - **Brain Penetration (`bbb`)**: Compounds with moderate `clogp` (typically between 1 and 3) and lower `tpsa` (< 60) are more likely to penetrate the blood-brain barrier.

2. **Filter Candidates**: Select compounds that fit the above criteria based on their descriptors.
3. **Run Assays**: Use the `measure_admet` tool to assay the selected compounds and verify their `logS` and `bbb` values.
4. **Update `answer.json`**: Populate the `qualifying_ids` list with compounds that meet both criteria.

## Selected Candidates for Assay
Based on the descriptors, the following compounds were prioritized for assay:
1. `z001510` (low `clogp`, moderate `tpsa`)
2. `z001543` (low `clogp`, high `tpsa`)
3. `z002216` (low `clogp`, high `tpsa`)
4. `z002730` (low `clogp`, moderate `tpsa`)
5. `z015334` (low `clogp`, moderate `tpsa`)
6. `z000364` (moderate `clogp`, moderate `tpsa`)
7. `z002003` (moderate `clogp`, low `tpsa`)
8. `z002010` (moderate `clogp`, low `tpsa`)

## Assay Results
1. **z001510**:
   - `logS`: -2.7372 (within range)
   - `bbb`: 1 (qualifies)
2. **z001543**:
   - `logS`: -0.8547 (outside range)
   - `bbb`: 1 (does not qualify)
3. **z002216**:
   - `logS`: -2.16 (within range)
   - `bbb`: 1 (qualifies)
4. **z002730**:
   - `logS`: -2.6758 (within range)
   - `bbb`: 1 (qualifies)
5. **z015334**:
   - `logS`: -1.8 (outside range)
   - `bbb`: 1 (does not qualify)
6. **z000364**:
   - `logS`: -0.9482 (outside range)
   - `bbb`: 0 (does not qualify)
7. **z002003**:
   - `logS`: -3.6 (outside range)
   - `bbb`: 1 (does not qualify)
8. **z002010**:
   - `logS`: -5.0097 (outside range)
   - `bbb`: 1 (does not qualify)

## Final Qualifying Compounds
- `z001510`
- `z002216`
- `z002730`

## Updated `answer.json`
The file `answer.json` has been updated with the qualifying compounds:
```json
{
  "qualifying_ids": ["z001510", "z002216", "z002730"]
}
```