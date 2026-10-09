# Notes for QED Prediction of m05

## Strategy
1. **Identify the scaffold**: The target molecule `m05` has the SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`. The scaffold is `COc1ccc(cc1)C(=O)NC`, which is the core structure shared by all congeners in the series.
2. **Search for congeners**: Used `search_pool` with the scaffold substring `COc1ccc(cc1)C(=O)NC` to find molecules in the same series.
3. **Measure QED**: Measured the QED of the found congeners to observe the trend.
4. **Extrapolate**: Fit the observed trend to predict the QED of `m05`.

## Steps
1. **Search for congeners**:
   - Query: `COc1ccc(cc1)C(=O)NC`
   - Found congeners: `m15`, `m17`, `m22`, `m24`, `m28`.
2. **Measure QED**:
   - `m15`: QED = 0.7621 (chain length: 2 carbons)
   - `m17`: QED = 0.7498 (chain length: 5 carbons)
   - `m22`: QED = 0.7377 (chain length: 6 carbons)
   - `m24`: QED = 0.7919 (chain length: 3 carbons)
   - `m28`: QED = 0.7518 (chain length: 4 carbons)
3. **Analyze trend**:
   - The QED values do not show a simple linear trend with chain length. Instead, they fluctuate around 0.75.
   - The average QED of the measured congeners is approximately 0.7587.
4. **Predict QED for m05**:
   - Given the lack of a clear trend, the average QED of the congeners (0.7587) is used as the predicted QED for `m05`.

## Expected Output
- `congener_ids`: ["m15", "m17", "m22", "m24", "m28"]
- `target_qed`: 0.7587