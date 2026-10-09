# Project Notes: Medicinal Chemistry Task

## Task Overview
We have a compound collection of 33 molecules with:
- `mol_id`, `smiles`, and five descriptors (`mw`, `alogp`, `psa`, `rotb`, `arom`)
- No QED values listed in the catalogue
- Tools available:
  - `list_pool(page)` - 4 entries per page, costs 1 budget unit per successful page
  - `measure_qed(mol_id)` - measure QED (0-1), costs 1 budget unit per successful measurement
  - Shared budget of 17 units total for both tasks

## Task Requirements
1. **Task 1**: Report QED of `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
   - `m05` has never been synthesized, so `measure_qed` returns `probe_unavailable`
   - Need to predict QED using trend from its congeneric series

2. **Task 2**: Pick one molecule that satisfies all three:
   a. measured QED ≥ 0.6
   b. `psa` ≥ 40
   c. not in same congeneric series as `m05`

## Initial Findings from group_by_scaffold()
- Total 4 scaffold groups
- `m05` is in group 1 with scaffold `COc1ccc(cc1)C(=O)N`
- Other members in same series: 
  - `m15` (chain_len 2)
  - `m24` (chain_len 3) 
  - `m28` (chain_len 4)
  - `m17` (chain_len 5)
  - `m22` (chain_len 6)
- `m05` has chain_len 11 (longest in this series)

## All Pages Listed (1-9)
Budget used: 9, Budget remaining: 118

Complete list of molecules with psa ≥ 40 (potential Task 2 candidates):

**Scaffold 2 (O=[N+]([O-])c1ccc(cc1)C(=O)N):**
- `m02`: psa = 72.24
- `m06`: psa = 72.24  
- `m09`: psa = 72.24
- `m10`: psa = 72.24
- `m12`: psa = 72.24
- `m29`: psa = 72.24
- `m31`: psa = 72.24
- `m32`: psa = 72.24
- `m33`: psa = 72.24

**Scaffold 3 (OCC(O)C(O)C(O)C(=O)N):**
- `m01`: psa = 110.02
- `m03`: psa = 110.02
- `m07`: psa = 110.02
- `m14`: psa = 110.02
- `m18`: psa = 110.02
- `m19`: psa = 110.02
- `m21`: psa = 110.02
- `m25`: psa = 110.02
- `m27`: psa = 110.02

**Scaffold 1 (COc1ccc(cc1)C(=O)N) - m05's series:**
All have psa = 38.33 (< 40): m15, m24, m28, m17, m22, m05

**Scaffold 4 (c1ccc(cc1)-c1ccc(cc1)C(=O)N):**
All have psa = 29.1 (< 40): m04, m11, m13, m30, m23, m20, m26, m08, m16

## Task 1 Progress
Measured QED for m05 series members:
1. `m15` (chain_len 2): QED = 0.7621
2. `m24` (chain_len 3): QED = 0.7919
3. `m28` (chain_len 4): QED = 0.7518
4. `m17` (chain_len 5): QED = 0.7498
5. `m22` (chain_len 6): QED = 0.7377

`m05` chain_len = 11

Fit results for m05 QED prediction:
- Linear (degree=1): 0.6950 (RMSE 0.01308)
- Quadratic (degree=2): 0.5416 (RMSE 0.01188) ✓ Best fit
- Cubic (degree=3): 2.1323 (RMSE 0.00835) Overfitted, invalid (>1)

Selected prediction: 0.5416 (quadratic fit)

Budget used: 14, Budget remaining: 113

## Task 2 Progress
Candidate search:
1. `m02`: QED = 0.6046 ✓, psa = 72.24 ✓, different scaffold than m05 ✓
2. `m06`: QED = 0.4011 ✗ (< 0.6)
3. `m09`: QED = 0.5826 ✗ (< 0.6)

`m02` meets all Task 2 criteria!

Budget used: 17/17 (all budget exhausted)

## Final Results
**Task 1**: m05 predicted QED = 0.5416 (quadratic fit)
**Task 2**: Selected molecule = m02 with QED = 0.6046

## Budget Usage Summary
- List pages: 9 units (pages 1-9, all 33 molecules)
- QED measurements: 8 units
  - m05 series: m15, m24, m28, m17, m22 (5 units)
  - Task 2 candidates: m02, m06, m09 (3 units)
- Total: 17/17 budget units used

## Rationale
1. **Task 1 prediction**: Used quadratic fit (degree=2) as it had lowest RMSE (0.01188) and gave reasonable prediction (0.5416) within 0-1 range. Linear fit gave 0.6950 but had higher RMSE. Cubic overfitted giving impossible value >1.
2. **Task 2 selection**: `m02` meets all criteria:
   - Measured QED = 0.6046 ≥ 0.6
   - psa = 72.24 ≥ 40
   - Different scaffold than m05 (scaffold 2 vs scaffold 1)
   Checked other candidates: m06 (QED=0.4011) and m09 (QED=0.5826) failed QED requirement.

## Budget Management
Total budget: 17 units
Need to allocate wisely between listing pages and QED measurements.