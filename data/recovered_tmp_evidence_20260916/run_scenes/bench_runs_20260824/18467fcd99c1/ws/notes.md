# Notes

## Initial Observations
- The task is to identify compounds from `data/candidates.tsv` that meet the following criteria:
  1. Measured `logS` between -3 and -2 inclusive.
  2. Measured `bbb` = 1 (indicating blood-brain-barrier penetration).
- We have a budget of 8 assays (calls to `measure_admet`).

## Strategy
1. **Prioritize Compounds**: Based on the descriptors in `data/candidates.tsv`, prioritize compounds likely to meet the target profile.
   - `logS` is related to solubility; compounds with moderate `clogp` (e.g., between 1 and 4) and reasonable `tpsa` (e.g., < 100) might be more likely to fall in the desired `logS` range.
   - `bbb` is related to brain penetration; compounds with lower `tpsa` (< 70) and moderate `clogp` (2-5) might be more likely to penetrate the BBB.

2. **Select Candidates**: Select 8 compounds that fit the above criteria for testing.

## Selected Candidates for Assay
Based on the descriptors, the following compounds are prioritized for testing:
- `z000364`: Moderate `clogp` (2.87), reasonable `tpsa` (83.1).
- `z001543`: Low `clogp` (-1.64), high `tpsa` (119.6) — less likely but worth testing.
- `z001999`: Moderate `clogp` (1.5), low `tpsa` (41.9).
- `z002010`: High `clogp` (4.28), low `tpsa` (29.5).
- `z002040`: Moderate `clogp` (1.77), moderate `tpsa` (46.2).
- `z002052`: Moderate `clogp` (1.99), moderate `tpsa` (65.1).
- `z002216`: Low `clogp` (0.86), high `tpsa` (98.0) — less likely but worth testing.
- `z002764`: High `clogp` (3.56), moderate `tpsa` (66.8).

## Testing Plan
Run `measure_admet` on each of these compounds and record the results.

## Results
- `z000364`: `logS` = -0.9482, `bbb` = 0 — **Does not qualify**.
- `z001999`: `logS` = -1.522, `bbb` = 1 — **Qualifies**.
- `z002040`: `logS` = -2.3374, `bbb` = 1 — **Qualifies**.
- `z002052`: `logS` = -2.7894, `bbb` = 1 — **Qualifies**.
- `z002764`: `logS` = -2.9008, `bbb` = 0 — **Does not qualify**.
- `z001543`: `logS` = -0.8547, `bbb` = 1 — **Does not qualify**.
- `z002010`: `logS` = -5.0097, `bbb` = 1 — **Does not qualify**.
- `z002216`: `logS` = -2.16, `bbb` = 1 — **Qualifies**.

## Summary of Qualifying Compounds
- `z001999`
- `z002040`
- `z002052`
- `z002216`