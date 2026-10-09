# Notes for Task Completion

## Task 1: Predicting QED for `m05`
- The molecule `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so `measure_qed` will return `probe_unavailable`.
- To predict its QED, we need to analyze its congeneric series (molecules sharing the same scaffold but differing in the N-alkyl chain length).
- We will first list the pool to identify molecules in the same series as `m05`.
- Then, we will measure the QED of these molecules to establish a trend and predict `m05`'s QED.

## Task 2: Selecting a Lead Molecule
- We need to find a molecule that satisfies:
  1. Measured QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.
- We will iterate through the pool, measuring QED for candidates that meet the `psa` requirement and are not in `m05`'s series.
- The budget is limited to 17 units, so we must prioritize efficiently.
