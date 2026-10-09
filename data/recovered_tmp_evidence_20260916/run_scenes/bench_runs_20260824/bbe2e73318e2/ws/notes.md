# Notes

## Task 1: Predict QED for `m05`

- `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so `measure_qed` will return `probe_unavailable`.
- To predict its QED, we need to analyze its congeneric series (molecules with the same scaffold but varying N-alkyl chain lengths).
- From `data/pool.tsv`, the congeneric series for `m05` includes:
  - `m15`: `COc1ccc(cc1)C(=O)NCC` (chain length: 2)
  - `m24`: `COc1ccc(cc1)C(=O)NCCC` (chain length: 3)
  - `m28`: `COc1ccc(cc1)C(=O)NCCCC` (chain length: 4)
  - `m17`: `COc1ccc(cc1)C(=O)NCCCCC` (chain length: 5)
  - `m22`: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length: 6)
  - `m05`: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain length: 12)

- We will measure QED for some of these to establish a trend and predict `m05`'s QED.

## Task 2: Select a Lead Molecule

- Constraints:
  1. QED ≥ 0.6
  2. `psa` ≥ 40
  3. Not in the same congeneric series as `m05`.

- We will identify candidates and measure their QED within the budget of 12 measurements.