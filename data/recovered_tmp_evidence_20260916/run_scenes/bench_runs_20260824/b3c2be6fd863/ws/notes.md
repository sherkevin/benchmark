# Notes on Task Execution

## Overview
Task: Predict QED for m05 (unsynthesized) and find a lead molecule from the pool.

## Budget Allocation
- Total budget: 12 units (shared between list_pool and measure_qed)
- list_pool: 9 units (9 pages × 1 unit each to view all 33 molecules)
- group_by_scaffold: 0 units (free)
- measure_qed: 3 units
- fit_trend: 0 units (free)

## Molecule Pool Summary
33 molecules across 4 scaffold groups:
1. Scaffold `COc1ccc(cc1)C(=O)N` (m05's series): m15, m24, m28, m17, m22, m05 - psa=38.33
2. Scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N`: m02, m06, m09, m10, m12, m29, m31, m32, m33 - psa=72.24
3. Scaffold `OCC(O)C(O)C(O)C(=O)N`: m01, m03, m07, m14, m18, m19, m21, m25, m27 - psa=110.02
4. Scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N`: m04, m08, m11, m13, m16, m20, m23, m26, m30 - psa=29.1

## Task 1: Predict QED for m05
m05 has chain_len=11 in its series. Measured three congeners:
- m15 (chain_len=2): QED = 0.7621
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Fit quadratic trend through these points and extrapolated to chain_len=11:
- Predicted QED for m05: 0.6172

## Task 2: Find Lead Molecule
Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in m05's series (scaffold `COc1ccc(cc1)C(=O)N`)

**Issue**: All three measurements (m15, m17, m22) were in m05's series, leaving no budget to measure eligible candidates from scaffolds 2 or 3 (which have psa ≥ 40).

Eligible molecules for Task 2 (psa ≥ 40, not m05 series):
- Scaffold 2 (nitro): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Scaffold 3 (sugar): m01, m03, m07, m14, m18, m19, m21, m25, m27

**Strategic error**: Should have allocated measurements differently:
- 1-2 measurements in m05's series for trend
- 1-2 measurements in scaffolds 2 or 3 for Task 2 candidate

With no remaining budget, cannot complete Task 2 as specified.

## Lessons Learned
For future runs with limited budget:
1. List pool efficiently (maybe not all pages needed)
2. Reserve budget for both tasks
3. Measure at least one eligible Task 2 candidate early

## Final Answer
- target_qed: 0.6172 (predicted from quadratic fit of m15, m17, m22)
- lead_mol_id: null (no valid measurement from eligible molecule)
- lead_qed_measured: null

**Note**: Strategic error in budget allocation prevented completion of Task 2. All 3 measurements were used on m05's series, leaving no budget to measure eligible candidates (scaffold 2 or 3 with psa ≥ 40).
