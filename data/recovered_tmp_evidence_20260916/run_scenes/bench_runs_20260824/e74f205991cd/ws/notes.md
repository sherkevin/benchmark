# Medicinal Chemistry Task Notes

## Task Overview
1. Predict QED for m05 (target molecule) which has never been synthesized
2. Pick a backup lead compound meeting three constraints:
   - Measured QED ≥ 0.6
   - PSA ≥ 40 (free descriptor)
   - Not in same series as m05

Total measurement budget: 17 measurements shared between both jobs.

## Initial Strategy
1. First identify the series for m05 using describe_structure
2. Find all molecules in pool with same scaffold
3. Measure QED for members of m05's series to establish trend
4. Extrapolate to predict m05's QED
5. Screen candidates for PSA ≥ 40 and not in m05's series
6. Measure QED of promising candidates to find one with QED ≥ 0.6

## Step 1: Analyze m05 structure
m05 has scaffold "COc1ccc(cc1)C(=O)N" with chain_len 11 (11-carbon alkyl chain).

## Step 2: Identify congeneric series members
Found these molecules with same scaffold:
-## m15 - chain_len 2 (PSA: 38.33)
-## m24 - chain_len 3 (PSA: 38.33)
-## m28 - chain_len 4 (PSA: 38.33)
-## m17 - chain_len 5 (PSA: 38.33)
-## m22 - chain_len 6 (PSA: 38.33)

All have PSA = 38.33 < 40, so they wouldn't meet constraint 2 for the backup lead.

## Step 3: Measure QED for series members to establish trend
Need to measure these 5 molecules to establish QED trend vs chain length.

Remaining budget: 17 - 9 (used for list_pool) = 8 measurements left.

## Step 4: QED measurements for m05's series
Measured QED values:
-## m15 (chain_len 2): 0.7621
-## m24 (chain_len 3): 0.7919
-## m28 (chain_len 4): 0.7518
-## m17 (chain_len 5): 0.7498
-## m22 (chain_len 6): 0.7377

Observations: QED seems to peak around chain_len 3 then gradually decreases.

Remaining budget: 17 - 14 = 3 measurements left.

## Step 5: Screen for backup lead candidates
Used screen_candidates() to find 18 molecules with PSA ≥ 40. All are not in m05's series.

## Step 6: Measure candidate QEDs
Measured m01 (polyhydroxy): QED = 0.3187 < 0.6 (too low)
Measured m02 (nitrobenzamide): QED = 0.6046 ≥ 0.6 ✓
m02 meets all 3 constraints:
1. QED = 0.6046 ≥ 0.6
2. PSA = 72.24 ≥ 40
3. Not in m05's series (different scaffold)

## Step 7: Predict m05 QED
Using fit_trend with all 5 series members (chain_len 2-6):
- Degree 2 (quadratic): predicts QED = 0.5416 for chain_len 11
- Degree 1 (linear): predicts QED = 0.6950
- Degree 3: gives impossible value >1

Quadratic fit is chosen as it captures the peak at chain_len 3 and decreasing trend, and longer alkyl chains typically reduce drug-likeness.

Final prediction: 0.5416

## Summary
- **Target m05 QED prediction**: 0.5416 (from quadratic fit of series members chain_len 2-6 extrapolated to chain_len 11)
- **Backup lead**: m02 with QED 0.6046
- **Congener series members**: m15, m24, m28, m17, m22
- **Measurements used**: 16/17 (1 remaining unused)
- **Rationale**: m02 meets all 3 constraints (QED ≥ 0.6, PSA ≥ 40, not in m05's series). Quadratic fit chosen for m05 prediction as it captures the peak at chain_len 3 and decreasing trend, which is biologically plausible for increasing alkyl chain length.