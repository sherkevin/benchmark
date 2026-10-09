# Task Notes

## Understanding the Problem

We have 33 molecules in `data/pool.tsv` with:
- `mol_id`: molecule identifier
- `smiles`: chemical structure
- `mw`: molecular weight
- `alogp`: octanol-water partition coefficient
- `psa`: polar surface area
- `rotb`: number of rotatable bonds
- `arom`: number of aromatic rings

## Task 1
We need to predict the QED of `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`). This molecule has never been synthesized, so we cannot measure it directly. We need to use the `fit_trend` function to predict its QED based on measurements of similar molecules.

## Task 2
We need to find one molecule that satisfies all three constraints:
1. measured QED ≥ 0.6
2. `psa` ≥ 40
3. not in the same congeneric series as `m05` (different scaffold or not just N-alkyl chain length variation)

## Budget
We have 12 measurement budget total for both tasks.

## Initial Observations
Looking at the SMILES patterns:
-## m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` has methoxy group and long alkyl chain
- Different scaffold types appear in the data:
  1. `OCC(O)C(O)C(O)C(=O)NCC...` - sugar-like scaffold with multiple hydroxyls
  2. `O=[N+]([O-])c1ccc(cc1)C(=O)N...` - nitrobenzene scaffold
  3. `c1ccc(cc1)-c1ccc(cc1)C(=O)N...` - biphenyl scaffold
  4. `COc1ccc(cc1)C(=O)N...` - methoxybenzene scaffold (same as m05)

Let me start by grouping the molecules by scaffold to understand the series.

## Scaffold Groups Analysis

From `group_by_scaffold()`:
1. **Group 1**: Scaffold `COc1ccc(cc1)C(=O)N` (methoxybenzene series, same as m05)
   - m15 (chain_len: 2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)

2. **Group 2**: Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzene series)
   - m09 (2), m02 (3), m12 (4), m32 (5), m33 (6), m29 (7), m31 (8), m06 (9), m10 (10)

3. **Group 3**: Scaffold `OCC(O)C(O)C(O)C(=O)N` (sugar-like series)
   - m14 (2), m27 (3), m01 (4), m19 (5), m03 (6), m21 (7), m18 (8), m07 (9), m25 (10)

4. **Group 4**: Scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl series)
   - m13 (2), m11 (3), m04 (4), m30 (5), m23 (6), m20 (7), m26 (8), m08 (9), m16 (10)

## Strategy for Task 1
I need to measure QED for some molecules in Group 1 (same scaffold as m05) to establish a trend between chain length and QED, then predict m05's QED using `fit_trend`.

## Strategy for Task 2
I need to find a molecule from Groups 2, 3, or 4 (not same scaffold as m05) with psa ≥ 40 and measured QED ≥ 0.6.

## Measurements So Far (Budget Used: 4/12)

**Group 1 measurements (for Task 1 trend):**
1. m15 (chain_len: 2): QED = 0.7621
2. m24 (chain_len: 3): QED = 0.7919  
3. m17 (chain_len: 5): QED = 0.7498
4. m22 (chain_len: 6): QED = 0.7377

Chain lengths vs QED:
- 2 → 0.7621
- 3 → 0.7919
- 5 → 0.7498
- 6 → 0.7377

m05 has chain_len: 11, so I need to extrapolate beyond chain_len 6.

## Final Measurements and Results (Budget Used: 12/12)

**Additional Group 1 measurement:**
- m28 (chain_len: 4): QED = 0.7518

**Group 2 measurements (for Task 2):**
- m09 (chain_len: 2): QED = 0.5826 (below 0.6)
- m02 (chain_len: 3): QED = 0.6046 ✓ (satisfies all constraints)
- m12 (chain_len: 4): QED = 0.4709
- m32 (chain_len: 5): QED = xx0.4683

**Group 3 measurements:**
- m14 (chain_len: 2): QED = 0.3237
- m27 (chain_len: 3): QED = 0.333
- m01 (chain_len: 4): QED = 0.3187

## Task 1 Prediction for m05
Using `fit_trend` with all 5 Group 1 points (chain_len: [2,3,4,5,6], QED: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]):
- Degree 2 polynomial: predicted QED = 0.5416
- Degree 1 polynomial: predicted QED = 0.695

Given m05's long chain (11 vs max measured 6), high MW (305.46 vs 235.33), and many rotatable bonds (12 vs 7), the degree 2 prediction (0.5416) accounts for accelerating decrease in drug-likeness and is selected.

## Task 2 Selection
Best candidate: **m02**
- Measured QED: 0.6046 ≥ 0.6 ✓
- psa: 72.24 ≥ 40 ✓  
- Scaffold: Different from m05 (Group 2 vs Group 1) ✓
- All constraints satisfied.

## Final Answer
- Task 1: Predicted QED for m05 = 0.5416 (using degree 2 polynomial fit on 5 measurements from same scaffold series)
- Task 2: Selected molecule m02 with measured QED = 0.6046

All budget (12 measurements) used:
- 5 measurements for Task 1 trend (m15, m24, m28, m17, m22)
- 7 measurements for Task 2 candidate search (m09, m02, m12, m32, m14, m27, m01)