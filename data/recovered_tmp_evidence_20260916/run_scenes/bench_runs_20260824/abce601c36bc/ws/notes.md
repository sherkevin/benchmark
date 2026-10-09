# Notes for Task Completion

1. **Task 1**: Predict the QED of `m05` (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`). Since it has never been synthesized, we need to find its congeneric series (molecules sharing the same scaffold but differing in N-alkyl chain length) and use `fit_trend` to predict its QED based on measured QEDs of its congeners.

2. **Task 2**: Identify a molecule from the pool that meets all three criteria:
   - QED ≥ 0.6 (measured)
   - `psa` ≥ 40
   - Not in the same congeneric series as `m05`.

**Plan**:
- First, group the pool by scaffold to identify congeneric series.
- For `m05`, find its series and measure QEDs of its congeners to predict its QED.
- For Task 2, iterate through the pool, measure QEDs of candidates that meet `psa` ≥ 40 and are not in `m05`'s series, and check if their QED ≥ 0.6.