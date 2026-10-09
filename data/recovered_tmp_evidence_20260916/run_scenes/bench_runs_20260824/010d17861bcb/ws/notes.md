# Notes for QED Prediction and Lead Compound Selection

## Job 1: Predicting QED for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has a chain length of 11 and belongs to the methoxyphenyl series with scaffold COc1ccc(cc1)C(=O)N.

The congeners in the same series (same scaffold, different chain lengths) are:
- m15: chain length 2, QED = 0.7621
- m24: chain length 3, QED = 0.7919  
- m28: chain length 4, QED = 0.7518
- m17: chain length 5, QED = 0.7498
- m22: chain length 6, QED = 0.7377

Using fit_trend with these 5 points and a quadratic fit (degree=2), I extrapolated to predict the QED for m05 with chain length 11:
- xs = [2, 3, 4, 5, 6]
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query = 11
- Prediction = 0.5416

## Job 2: Selecting a Lead Compound

I needed a compound that meets all three criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in the same series as m05

From screen_candidates(), I identified 18 compounds with PSA ≥ 40:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

Of these, compounds in the same series as m05 (COc1ccc(cc1)C(=O)N) were excluded:
- m15, m24, m28, m17, m22, m05

So available candidates with PSA ≥ 40 were:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

I tested m01 first and found QED = 0.3187 (< 0.6), so it failed criterion 1.
I tested m02 next and found QED = 0.6046 (≥ 0.6), and confirmed:
- PSA = 72.24 ≥ 40 ✓
- Scaffold is O=[N+]([O-])c1ccc(cc1)C(=O)N, different from m05's COc1ccc(cc1)C(=O)N ✓

Therefore, m02 meets all three criteria and is selected as the lead compound.