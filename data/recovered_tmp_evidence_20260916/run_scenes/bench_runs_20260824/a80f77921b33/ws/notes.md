# Task Notes

## Target Molecule m05
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N`
- Chain length: 11 carbons

## Plan
1. Explore pool to find all molecules with scaffold `COc1ccc(cc1)C(=O)N`
2. Measure QED for these congeneric molecules (short-chain members)
3. Fit trend and extrapolate to chain length 11 for m05
4. Find lead compound meeting three constraints:
   - QED ≥ 0.6 (must measure)
   - psa ≥ 40 (free descriptor)
   - Not in m05's series (check scaffold)

Budget: 17 total measurements shared between list_pool and measure_qed.

## Progress Log

### Pool Exploration
- Used 9 budget units to explore all 9 pages of pool (33 molecules total)
- Found congeners of m05 (same scaffold `COc1ccc(cc1)C(=O)N`):
  1. m15: `COc1ccc(cc1)C(=O)NCC` (chain length 2)
  2. m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain length 5)
  3. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain length 6)
  4. m24: `COc1ccc(cc1)C(=O)NCCC` (chain length 3)
  5. m28: `COc1ccc(cc1)C(=O)NCCCC` (chain length 4)

### QED Measurements for m05 Series
Measured QED values for congeners:
- Chain length 2 (m15): 0.7621
- Chain length 3 (m24): 0.7919  
- Chain length 4 (m28): 0.7518
- Chain length 5 (m17): 0.7498
- Chain length 6 (m22): 0.7377

Budget used: 14/17 (3 measurements remaining)

### Trend Analysis for m05 Prediction
Data points: xs = [2, 3, 4, 5, 6], ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Fits:
1. Quadratic (degree 2): Prediction for chain length 11 = 0.5416
   Coefficients: 0.74932 + 0.017024x - 0.003264x²
   RMSE: 0.01188
   
2. Linear (degree 1): Prediction for chain length 11 = 0.695
   Coefficients: 0.79502 - 0.00909x
   RMSE: 0.01308

Quadratic fit has slightly lower RMSE and shows the peak at chain length ~3 followed by decline. Extrapolation to chain length 11 gives 0.5416.

### Lead Compound Search
Used screen_candidates() to find molecules with psa ≥ 40: 18 candidates.

Filtered out m05's series (all have psa=38.33 < 40, so automatically excluded).

Measured candidates:
1. m01 (polyol series): QED = 0.3187 < 0.6 ❌
2. m02 (nitrobenzene series): QED = 0.6046 ≥ 0.6 ✅

m02 satisfies all constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓  
- Not in m05's series (scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` vs `COc1ccc(cc1)C(=O)N`) ✓

Budget used: 16/17 (1 remaining)

## Final Answer
- target_qed: 0.5416 (quadratic extrapolation from 5 measured congeners)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m24", "m28", "m17", "m22"] (all pool molecules with scaffold `COc1ccc(cc1)C(=O)N`)