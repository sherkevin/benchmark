# Notes

## Task Overview
- We have 20 compounds listed in `data/candidates.tsv`.
- We need to identify compounds that meet the following criteria:
  1. Measured `logS` between -3 and -2 inclusive.
  2. Measured `bbb` = 1 (indicating brain penetration).
- Budget: 8 assays in total.

## Strategy
1. **Initial Filtering**: Prioritize compounds based on descriptors that might correlate with solubility and brain penetration (e.g., `clogp`, `tpsa`, `hbd`, `hba`).
2. **Assay Selection**: Use the `measure_admet` tool to test the most promising candidates first.
3. **Record Results**: Document the results in `notes.md` and update `answer.json` with qualifying compounds.

## Initial Observations
- Compounds with lower `clogp` and higher `tpsa` might have better solubility.
- Compounds with fewer `hbd` and moderate `hba` might have better brain penetration.

## Assay Results
1. **z000364**:
   - `logS`: -0.9482 (outside range)
   - `bbb`: 0 (does not qualify)
2. **z002003**:
   - `logS`: -3.6 (outside range)
   - `bbb`: 1 (does not qualify)
3. **z002052**:
   - `logS`: -2.7894 (within range)
   - `bbb`: 1 (qualifies)
4. **z002010**:
   - `logS`: -5.0097 (outside range)
   - `bbb`: 1 (does not qualify)
5. **z002216**:
   - `logS`: -2.16 (within range)
   - `bbb`: 1 (qualifies)
6. **z002730**:
   - `logS`: -2.6758 (within range)
   - `bbb`: 1 (qualifies)
7. **z015334**:
   - `logS`: -1.8 (outside range)
   - `bbb`: 1 (does not qualify)
8. **z001543**:
   - `logS`: -0.8547 (outside range)
   - `bbb`: 1 (does not qualify)

## Budget Used: 8/8

## Final Answer
- **Qualifying Compounds**: `z002052`, `z002216`, `z002730`