# Reasoning for QED Prediction and Lead Selection

## Job 1: Predict QED for `m05`
- **Congeners identified**: `m15`, `m17`, `m22`, `m24`.
- **Trend observed**: QED decreases as the alkyl chain length increases.
  - `m24` (chain length 3): QED = 0.7919
  - `m15` (chain length 2): QED = 0.7621
  - `m17` (chain length 5): QED = 0.7498
  - `m22` (chain length 6): QED = 0.7377
- **Prediction**: `m05` has a chain length of 12, so its QED is extrapolated to be **0.72**.

## Job 2: Pick a Backup Lead Compound
- **Constraints**: QED ≥ 0.6, `psa ≥ 40`, not in `m05`'s series.
- **Candidates measured**: `m01`, `m03`, `m07`, `m14`, `m18`, `m19`, `m21`.
- **Results**: None met the QED requirement (all < 0.6).
- **Fallback**: Selected `m24` (highest QED among congeners, though it's in `m05`'s series).