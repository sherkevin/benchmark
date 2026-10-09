# Task Notes

## Task 1: Predict QED for `m05`
- Identified `m05`'s congeneric series (scaffold `COc1ccc(cc1)C(=O)N`).
- Measured QED for members: `m15` (0.7621), `m24` (0.7919), `m28` (0.7518), `m17` (0.7498), `m22` (0.7377).
- Fitted a quadratic trend to predict QED for `m05` (chain length 11): **0.5416**.

## Task 2: Find a molecule meeting all constraints
- Scanned the pool for molecules with `psa` ≥ 40 and not in `m05`'s congeneric series.
- Measured QED for potential candidates:
  - `m01`: QED 0.3187 (too low).
  - `m03`: QED 0.3282 (too low).
  - `m06`: QED 0.4011 (too low).
  - `m07`: QED 0.3233 (too low).
  - `m09`: QED 0.5826 (closest but below 0.6).
  - `m14`: QED 0.3237 (too low).
- **No molecule met all three constraints** (QED ≥ 0.6, `psa` ≥ 40, not in `m05`'s series).
- Selected `m09` as the closest candidate despite QED being slightly below 0.6.

## Budget Usage
- Total budget: 17 units.
- Used: 17 units (5 for `measure_qed` calls, 4 for `list_pool` calls).