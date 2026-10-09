# Notes for Compound Triage

## Initial Analysis
- There are 20 compounds in `data/candidates.tsv`.
- The target profile requires:
  - `logS` between -3 and -2 (inclusive).
  - `bbb = 1` (blood-brain-barrier penetration).
- Budget: 8 assays.

## Strategy
1. **Filter based on descriptors**: Prioritize compounds with descriptors likely to correlate with the target profile:
   - `clogp` (partition coefficient): Lower values may indicate better solubility.
   - `tpsa` (topological polar surface area): Lower values may indicate better BBB penetration.
   - `hbd` (hydrogen bond donors): Fewer donors may improve BBB penetration.
2. **Select top candidates**: Focus on compounds with `clogp` between 1 and 4, `tpsa` < 60, and `hbd` ≤ 1.
3. **Run assays**: Use `measure_admet` on the selected compounds.

## Assay Plan
- Compounds to prioritize:
  - `z000364`: `clogp = 2.87`, `tpsa = 83.1`, `hbd = 1`.
  - `z001999`: `clogp = 1.5`, `tpsa = 41.9`, `hbd = 1`.
  - `z002010`: `clogp = 4.28`, `tpsa = 29.5`, `hbd = 0`.
  - `z002052`: `clogp = 1.99`, `tpsa = 65.1`, `hbd = 0`.
  - `z002170`: `clogp = 3.0`, `tpsa = 34.9`, `hbd = 0`.
  - `z002216`: `clogp = 0.86`, `tpsa = 98.0`, `hbd = 2`.
  - `z002730`: `clogp = 0.09`, `tpsa = 83.6`, `hbd = 1`.
  - `z015334`: `clogp = 0.74`, `tpsa = 81.6`, `hbd = 1`.

## Results
- **z000364**: `logS = -0.9482`, `bbb = 0` — Does not qualify.
- **z001999**: `logS = -1.522`, `bbb = 1` — Does not qualify (logS outside range).
- **z002010**: `logS = -5.0097`, `bbb = 1` — Does not qualify (logS outside range).
- **z002052**: `logS = -2.7894`, `bbb = 1` — Qualifies!
- **z002170**: `logS = -2.92`, `bbb = 1` — Qualifies!
- **z002216**: `logS = -2.16`, `bbb = 1` — Qualifies!
- **z002730**: `logS = -2.6758`, `bbb = 1` — Qualifies!
- **z015334**: `logS = -1.8`, `bbb = 1` — Does not qualify (logS outside range).
