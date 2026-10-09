# Notes

## Task 1: Predicting QED for `m05`

- The molecule `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so `measure_qed` will return `probe_unavailable`.
- To predict its QED, we need to analyze its congeneric series (molecules with the same scaffold but differing in the N-alkyl chain length).
- We will first list the pool to identify molecules in the same series as `m05` and measure their QEDs to establish a trend.

## Task 2: Selecting a Lead Molecule

- We need to find a molecule that satisfies:
  1. QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.
- We will list the pool and filter candidates based on the descriptors, then measure their QEDs to confirm eligibility.

## Budget Management

- Total budget: 17 units (shared between `list_pool` and `measure_qed`).
- Each page of `list_pool` costs 1 unit.
- Each `measure_qed` call costs 1 unit (unless it returns `probe_unavailable`).

Next steps:
1. List the pool to identify molecules in the same series as `m05`.
2. Measure QEDs of these molecules to predict `m05`'s QED.
3. List the pool again to find candidates for Task 2.
4. Measure QEDs of eligible candidates.