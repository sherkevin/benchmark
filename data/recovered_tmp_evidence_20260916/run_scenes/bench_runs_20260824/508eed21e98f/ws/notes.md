# Notes for Task

## Task 1: Predicting QED for `m05`

- The molecule `m05` has the SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`.
- Since it has never been synthesized, `measure_qed` will return `probe_unavailable`.
- Need to predict its QED based on the trend of its congeneric series (molecules with the same scaffold but varying N-alkyl chain lengths).

## Task 2: Selecting a Lead Molecule

- Must find a molecule from the pool that meets all three criteria:
  1. Measured QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.

### Plan
1. Use `list_pool` to explore the molecule pool, 4 entries per page.
2. Identify molecules not in the same series as `m05`.
3. Measure QED for promising candidates that meet `psa` ≥ 40.
4. Select the first molecule that satisfies all criteria.

## Budget Considerations
- Total budget: 12 units (shared between `list_pool` and `measure_qed`).
- Each page of `list_pool` costs 1 unit.
- Each `measure_qed` call costs 1 unit.
- Need to optimize budget usage to ensure we can complete both tasks.