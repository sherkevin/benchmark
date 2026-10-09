# Notes

## Task 1: Predict QED for `m05`

- `m05` has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` and is part of a congeneric series where the N-alkyl chain length varies.
- Since `m05` has never been synthesized, we cannot measure its QED directly. Instead, we will predict its QED based on the trend of QED values for other molecules in the same series.

## Task 2: Find a Lead Molecule

- We need to find a molecule that satisfies:
  1. QED ≥ 0.6 (measured)
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05` (i.e., does not share the scaffold `COc1ccc(cc1)C(=O)N` with varying N-alkyl chain lengths).

### Plan:
1. Identify molecules not in the same series as `m05`.
2. Filter these molecules for `psa` ≥ 40.
3. Measure QED for the remaining candidates (within the budget of 12 measurements).
4. Select the first molecule that meets all criteria.
