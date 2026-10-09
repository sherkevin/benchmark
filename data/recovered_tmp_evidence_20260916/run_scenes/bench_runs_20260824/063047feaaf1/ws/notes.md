# Task Notes

## Job 1: Predict QED for `m05`

1. **Identified the series for `m05`**: Scaffold `COc1ccc(cc1)C(=O)N`, chain length = 11.
2. **Found molecules in the same series**:
   - `m15`: Chain length = 2, QED = 0.7621
   - `m24`: Chain length = 3, QED = 0.7919
   - `m17`: Chain length = 5, QED = 0.7498
   - `m22`: Chain length = 6, QED = 0.7377
3. **Fitted a trend**: Used `fit_trend` with degree=2 to predict `m05`'s QED.
   - Predicted QED for `m05` (chain length = 11): **0.372**.

## Job 2: Pick a Backup Lead Compound

1. **Filtered candidates with `psa` ≥ 40**: Used `screen_candidates`.
2. **Measured QED for promising candidates**:
   - `m01`: QED = 0.3187 (too low)
   - `m02`: QED = 0.6046 (meets QED ≥ 0.6)
3. **Verified `m02` is not in `m05`'s series**: Scaffold is different.

## Final Answer

- **Target QED for `m05`**: 0.372
- **Lead Compound**: `m02` (QED = 0.6046)
- **Congener IDs**: `["m15", "m24", "m17", "m22"]`