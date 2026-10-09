# Medicinal Chemistry Analysis Notes

## Initial Observations

From `data/pool.tsv`, we have 33 molecules with:
- `mol_id`: molecule identifier
- `smiles`: molecular structure
- `mw`: molecular weight
- `alogp`: calculated logP
- `psa`: polar surface area
- `rotb`: number of rotatable bonds
- `arom`: number of aromatic rings

Target molecule `m05` has SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

## Scaffold Analysis Results

From `group_by_scaffold()` tool, we have 4 scaffold groups:

1. **Group 1**: `COc1ccc(cc1)C(=O)N` (m05's series)
   - m15 (chain_len: 2)
   - m24 (chain_len: 3)
   - m28 (chain_len: 4)
   - m17 (chain_len: 5)
   - m22 (chain_len: 6)
   - m05 (chain_len: 11) - target

2. **Group 2**: `O=[N+]([O-])c1ccc(cc1)C(=O)N`
   - m09, m02, m12, m32, m33, m29, m31, m06, m10

3. **Group 3**: `OCC(O)C(O)C(O)C(=O)N`
   - m14, m27, m01, m19, m03, m21, m18, m07, m25

4. **Group 4**: `c1ccc(cc1)-c1ccc(cc1)C(=O)N`
   - m13, m11, m04, m30, m23, m20, m26, m08, m16

## Task Breakdown

### Task 1: Predict QED for m05
- m05 has never been synthesized, so cannot be measured directly
- Must predict QED from its congeneric series (same scaffold, varying N-alkyl chain length)
- m05 is in Group 1 with scaffold `COc1ccc(cc1)C(=O)N`
- Available series members: m15, m24, m28, m17, m22
- Need to measure QED for some of these to establish trend
- Use `fit_trend` to predict m05's QED at chain_len=11

### Task 2: Find molecule satisfying all constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40  
3. not in same congeneric series as m05 (not in Group 1)

**PSA Analysis:**
- Group 1 (m05's series): psa = 38.33 (all < 40)
- Group 2: psa = 72.24 (all ≥ 40)
- Group 3: psa = 110.02 (all ≥ 40)
- Group 4: psa = 29.1 (all < 40)

**Therefore for Task 2, only Groups 2 and 3 qualify based on psa ≥ 40.**

Budget: 12 total measurements shared between both tasks.

## Measurements Taken (11 used, 1 remaining)

### Group 1 (m05's series) measurements:
1. m15 (chain_len=2): QED = 0.7621
2. m24 (chain_len=3): QED = 0.7919  
3. m28 (chain_len=4): QED = 0.7518
4. m17 (chain_len=5): QED = 0.7498
5. m22 (chain_len=6): QED = 0.7377

### Group 2 measurements:
1. m09 (chain_len=2): QED = 0.5826
2. m02 (chain_len=3): QED = 0.6046 ✓ (satisfies QED ≥ 0.6)
3. m12 (chain_len=4): QED = 0.4709
4. m32 (chain_len=5): QED = 0.4683

### Group 3 measurements:
1. m14 (chain_len=2): QED = 0.3237
2. m27 (chain_len=3): QED = 0.333

## Task 1 Prediction for m05
Using linear fit on Group 1 data (chain lengths 2-6):
- xs: [2, 3, 4, 5, 6]
- ys: [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]
- x_query: 11 (m05 chain_len)
- Prediction: 0.695

## Task 2 Candidate Selection
Best candidate found: **m02**
- QED: 0.6046 (≥ 0.6)
- psa: 72.24 (≥ 40)
- Not in same series as m05 (Group 2 vs Group 1)

## Final Answer
- Task 1: Predicted QED for m05 = 0.695 (linear fit based on congeneric series)
- Task 2: Selected m02 with measured QED = 0.6046

All requirements satisfied within 11 measurements (1 remaining unused).