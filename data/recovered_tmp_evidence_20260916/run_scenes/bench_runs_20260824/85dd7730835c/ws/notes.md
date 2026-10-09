# Notes for Task Completion

## Task 1: Predict QED for `m05`
- `m05` has never been synthesized, so we cannot measure its QED directly.
- We need to identify its congeneric series (shared scaffold and varying N-alkyl chain lengths) and predict its QED based on measured QEDs of other molecules in the same series.

## Task 2: Find a Lead Molecule
- The lead molecule must satisfy:
  1. Measured QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.
- We will explore the pool to find such a molecule.

Next steps:
1. Use `group_by_scaffold()` to identify the congeneric series for `m05`.
2. Measure QEDs of molecules in the same series to predict `m05`'s QED.
3. Explore the pool to find a lead molecule meeting all constraints.