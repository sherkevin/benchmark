# Notes for Task Completion

## Job 1: Predict the QED of `m05`
1. **Identified the congeneric series for `m05`**: Used `describe_structure` to find molecules with the scaffold `COc1ccc(cc1)C(=O)N`.
2. **Measured QED for the series**: Measured QED for `m15`, `m17`, `m22`, `m24`, and `m28`.
3. **Fitted the trend**: Used `fit_trend` to predict the QED for `m05` (chain_len = 11) based on the measured QED values. The predicted QED is **0.5416**.

## Job 2: Pick a backup lead compound
1. **Screened candidates**: Used `screen_candidates` to find molecules with `psa` ≥ 40.
2. **Filtered by series**: Excluded molecules in the same series as `m05`.
3. **Measured QED**: Measured QED for `m01`, `m03`, `m07`, `m14`, `m18`, `m19`, and `m21`. None met the QED ≥ 0.6 constraint within the budget.

## Conclusion
- Predicted QED for `m05`: **0.5416**.
- No lead compound met all three constraints within the measurement budget.