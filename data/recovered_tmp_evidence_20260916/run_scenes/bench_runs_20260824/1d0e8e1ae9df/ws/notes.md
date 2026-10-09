# Notes - QED Prediction Task

## Step 1: Explored the pool (33 molecules)

Identified 4 congeneric series by scaffold:
1. **m05 series** (scaffold `COc1ccc(cc1)C(=O)N`): m05, m15, m17, m22, m24, m28
2. **Nitro series** (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`): m02, m06, m09, m10, m12, m29, m31, m32, m33
3. **Biphenyl series**: m04, m08, m11, m13, m16, m20, m23, m26, m30
4. **Sugar series**: m01, m03, m07, m14, m18, m19, m21, m25, m27

## Step 2: m05 series congeners (chain lengths)
- m15: 2, m24: 3, m28: 4, m17: 5, m22: 6, m05: 11 (target)

## Step 3: Measured QED for m05 series
- m15: 0.7621, m24: 0.7919, m28: 0.7518, m17: 0.7498, m22: 0.7377

## Step 4: Fit trend (degree 2) and extrapolate to chain_len=11
Prediction: 0.5416 (quadratic fit, RMSE=0.01188)

## Step 5: Found backup lead
- m02: QED=0.6046, psa=72.24, not in m05 series - VALID!

## Final Answer
- target_qed: 0.5416
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: [m15, m24, m28, m17, m22]
