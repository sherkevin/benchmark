# Task Notes

## Overview
I need to complete two jobs:
1. Predict the QED of molecule `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) which has never been synthesized
2. Pick a backup lead compound that meets three constraints:
   - measured QED ≥ 0.6
   - psa ≥ 40
   - not in the same series as `m05`

I have 17 total measurements available (shared between both jobs).

## Plan
1. First, explore the compound pool to identify molecules in the same series as `m05`
2. Measure QED values for the series members to establish a trend
3. Extrapolate to predict QED for `m05` (which has a longer chain)
4. Identify candidates for the lead compound by:
   - Using `screen_candidates()` to filter by psa ≥ 40
   - Checking which candidates are NOT in `m05`'s series
   - Measuring their QED to find one ≥ 0.6

## Step 1: Explore the compound pool
I've explored all 9 pages of the pool (33 molecules total, 4 per page). I've identified the following series:

1. **Polyhydroxy series**: `OCC(O)C(O)C(O)C(=O)N-` (m01, m03, m07, m14, m18, m19, m21, m25, m27)
2. **Nitro phenyl series**: `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (m02, m06, m09, m10, m12, m29, m31, m32, m33)
3. **Biphenyl series**: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` (m04, m08, m11, m13, m16, m20, m23, m26, m30)
4. **Methoxy phenyl series (m05's series)**: `COc1ccc(cc1)C(=O)N-` (m05, m15, m17, m22, m24, m28)

## Step 2: Identify m05 series members
Molecules in the same series as `m05` (same scaffold `COc1ccc(cc1)C(=O)N-`, only alkyl chain length differs):
## Step 2: Identify m05 series members
Molecules in the same series as `m05` (same scaffold `COc1ccc(cc1)C(=O)N-`, only alkyl chain length differs):
- `m15`: `COc1ccc(cc1)C(=O)NCC` (C2 chain)
- `m24`: `COc1ccc(cc1)C(=O)NCCC` (C3 chain)
- `m28`: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain)
- `m17`: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain)
- `m22`: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain)
- `m05`: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C12 chain) - target, unsynthesized

Chain lengths: C2, C3, C4, C5, C6 (measured pool members) → C12 (target, needs extrapolation)

## Step 3: Measure QED values for m05 series members
Measured QED values:
- `m15` (C2): QED = 0.7621
- `m24` (C3): QED = 0.7919  
- `m28` (C4): QED = 0.7518
- `m17` (C5): QED = 0.7498
- `m22` (C6): QED = 0.7377

Pattern: QED peaks at C3 (0.7919) then decreases as chain length increases.

Budget used: 14/17 (9 for list_pool pages + 5 for measurements)

## Step 4: Fit trend and extrapolate to m05 (C12)
I need to use `fit_trend` to extrapolate to chain length 12. I'll use all 5 data points with degree=2 (quadratic fit) since the relationship appears curved.

Fit results:
- Quadratic fit (degree=2): predicted QED = 0.4836, RMSE = 0.01188
- Linear fit (degree=1): predicted QED = 0.6859, RMSE = 0.01308

The quadratic fit has slightly better RMSE and shows the downward curvature observed in the data (peak at C3, then decrease). Since we're extrapolating far beyond the measured range (C6 → C12), both predictions are uncertain, but quadratic seems more reasonable given the clear peak-decline pattern.

I'll use the quadratic prediction: **target_qed = 0.4836**

Now I need to work on Job 2: finding a backup lead compound.

## Step 5: Find lead compound meeting all constraints
Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in the same series as `m05`

### Step 5.1: Filter by psa ≥ 40
Used `screen_candidates()`: 18 candidates pass psa ≥ 40 filter.

### Step 5.2: Exclude m05 series members
m05 series: m15, m24, m28, m17, m22, m05
None of these are in the filtered list, so all 18 candidates satisfy constraint 3.

### Step 5.3: Measure QED to find ≥ 0.6
Budget remaining: 3 measurements (14 used: 9 list_pool + 5 m05 series measurements)

Measured candidates:
1. `m01` (polyhydroxy series): QED = 0.3187 < 0.6 ❌
2. `m02` (nitro phenyl series): QED = 0.6046 ≥ 0.6 ✓

`m02` meets all three constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓ (from screen_candidates)
- Not in m05 series (nitro phenyl vs methoxy phenyl) ✓

Budget used: 16/17 (1 remaining)

I could use the last measurement to try to find a compound with higher QED, but `m02` already satisfies all requirements. Since the task only requires picking **one** molecule that satisfies all constraints, `m02` is a valid choice.

## Summary
1. **Job 1 - Predict QED of m05**: 
   - Identified 5 congeners in same series (m15, m24, m28, m17, m22)
   - Measured their QED values
   - Fitted quadratic trend (degree=2) to chain lengths vs QED
   - Extrapolated to chain length 12: predicted QED = 0.4836

2. **Job 2 - Select backup lead**:
   - Used `screen_candidates()` to filter by psa ≥ 40 (18 candidates)
   - Excluded m05 series members (none in filtered list)
   - Measured `m02`: QED = 0.6046 ≥ 0.6
   - `m02` satisfies all constraints

**Final answer**:
- `target_qed`: 0.4836 (predicted for m05)
- `lead_mol_id`: "m02"
- `lead_qed_measured`: 0.6046
- `congener_ids`: ["m15", "m24", "m28", "m17", "m22"]