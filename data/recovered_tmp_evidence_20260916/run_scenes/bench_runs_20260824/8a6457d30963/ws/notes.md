# Notes for Compound Analysis

## Task 1: Predicting QED for m05

### Step 1: Identify the congeneric series of m05
- Use `group_by_scaffold` to find molecules sharing the same scaffold as `m05`.
- Extract the N-alkyl chain lengths and mol_ids.

### Step 2: Measure QEDs of related molecules
- Measure QEDs for molecules in the same series to gather data for trend fitting.

### Step 3: Predict QED for m05
- Use `fit_trend` to predict `m05`'s QED based on the measured data.

## Task 2: Finding a Lead Molecule

### Step 1: List the pool
- Use `list_pool` to explore available molecules.

### Step 2: Filter for psa ≥ 40
- Identify molecules with `psa` ≥ 40.

### Step 3: Measure QEDs and verify constraints
- Measure QEDs of promising candidates.
- Ensure the molecule is not in the same series as `m05`.

## Budget Tracking
- Total budget: 12 units.
- Each `list_pool` page: 1 unit.
- Each `measure_qed`: 1 unit.
