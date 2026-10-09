# Notes

## Task 1

- The QED for `m05` (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) cannot be measured directly because it has never been synthesized (`probe_unavailable`).
- Need to predict its QED based on the trend of its congeneric series (molecules with the same scaffold but varying N-alkyl chain lengths).

## Task 2

- Need to find a molecule that satisfies:
  1. Measured QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05` (different scaffold or not just varying N-alkyl chain length).
- Will use `list_pool` to explore the compound collection and `measure_qed` to measure QED for candidates.

### Findings

- **m02**: QED = 0.6046, `psa` = 72.24, not in the same series as `m05` (different scaffold).
- **m02** satisfies all three constraints:
  1. QED = 0.6046 ≥ 0.6
  2. `psa` = 72.24 ≥ 40
  3. Not in the same congeneric series as `m05`.