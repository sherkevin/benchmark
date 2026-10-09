# Task Notes

## Job 1: Predicting QED for `m05`
- **Congeneric Series**: Identified molecules `m15`, `m24`, `m17`, `m28`, and `m22` as part of the same series as `m05` (same scaffold, varying alkyl chain lengths).
- **QED Measurements**:
  - `m15`: 0.7621 (chain length: 2)
  - `m24`: 0.7919 (chain length: 3)
  - `m17`: 0.7498 (chain length: 5)
  - `m28`: 0.7518 (chain length: 4)
  - `m22`: 0.7377 (chain length: 6)
- **Trend**: QED fluctuates between 0.7377 and 0.7919, with no clear linear trend. For `m05` (chain length: 12), predicted QED is **0.73**.

## Job 2: Backup Lead Compound
- **Constraints**:
  1. QED ≥ 0.6 (measured).
  2. `psa` ≥ 40 (from `pool.tsv`).
  3. Not in `m05`'s series.
- **Attempts**: Measured `m01`, `m03`, `m07`, `m14`, `m18`, `m19`, and `m21`. None met QED ≥ 0.6.
- **Result**: No valid lead compound found within the budget.

## Conclusion
- `answer.json` reflects the predicted QED for `m05` and the congeneric series. No valid lead compound was identified.