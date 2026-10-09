# Notes: Predicting QED for `m05`

## Steps Taken
1. **Identified the scaffold**: The scaffold of `m05` is `COc1ccc(cc1)C(=O)NC`, with the variable alkyl chain.
2. **Searched for congeneric molecules**: Used `search_pool` to find molecules sharing this scaffold. Found 6 molecules, including `m05`.
3. **Measured QED**: Measured QED for the following molecules:
   - `m15`: Chain length 2, QED = 0.7621
   - `m24`: Chain length 3, QED = 0.7919
   - `m28`: Chain length 4, QED = 0.7518
   - `m17`: Chain length 5, QED = 0.7498
   - `m22`: Chain length 6, QED = 0.7377
4. **Trend Analysis**: QED peaks at chain length 3 and decreases as chain length increases. Attempted a quadratic fit, but extrapolation to chain length 12 yielded an unrealistic QED (~0.0). Adjusted approach: assumed QED stabilizes around `0.6` for longer chains.
5. **Prediction**: Predicted QED for `m05` (chain length 12) is `0.60`.

## Congeneric Molecules
- `m15`
- `m24`
- `m28`
- `m17`
- `m22`