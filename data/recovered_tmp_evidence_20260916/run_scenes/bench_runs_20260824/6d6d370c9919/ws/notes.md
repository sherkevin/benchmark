# Notes

## Task Overview
- Task 1: Predict QED for m05 (not synthesized, so measure_qed returns probe_unavailable)
- Task 2: Find a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's congeneric series
- Budget: 12 units total (shared by list_pool and measure_qed)

## Data Collection
Read all 33 molecules from the pool (9 pages × 1 unit = 9 budget units).

## Scaffold Analysis (free)
Used group_by_scaffold to identify 4 scaffold groups:
1. `COc1ccc(cc1)C(=O)N` - m05's series (m15, m24, m28, m17, m22, m05) - psa=38.33
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` - nitro series (9 molecules) - psa=72.24
3. `OCC(O)C(O)C(O)C(=O)N` - sugar series (9 molecules) - psa=110.02
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` - biphenyl series (9 molecules) - psa=29.1

For Task 2, candidates must be from Group 2 or 3 (psa ≥ 40) and NOT Group 1.

## Task 1: Predicting m05's QED
Measured 3 molecules from m05's series (3 budget units):
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518

Used fit_trend with linear fit (degree=1) to predict QED at chain_len=11:
- Prediction: 0.7274

Quadratic fit gave unrealistic negative value (-1.4861), so linear fit was used.

## Task 2: Finding a Lead Molecule
**Problem**: Budget exhausted (9 + 3 = 12 units). Cannot measure any molecules from Groups 2 or 3.

Without measurements, I cannot verify which molecules have QED ≥ 0.6.

### Potential Candidates (based on descriptors)
From Group 2 (nitro, psa=72.24), molecules with moderate alogp might have good QED:
- m02: alogp=1.735, mw=208.22
- m12: alogp=2.125, mw=222.24
- m32: alogp=2.515, mw=236.27

From Group 3 (sugar, psa=110.02), molecules tend to have very low alogp (negative values), which might lower QED.

## Budget Management Issue
I spent:
- 9 units on list_pool (reading all 33 molecules)
- 3 units on measure_qed (m15, m24, m28 for m05's series)
- Total: 12 units (exhausted)

A better strategy would have been:
- Use group_by_scaffold first (free)
- Read fewer pages from list_pool (only needed for descriptors)
- Reserve budget for measuring candidates from Groups 2/3 for Task 2

## Conclusion
- Task 1: Completed with prediction of 0.7274 for m05 (using linear fit on m15, m24, m28)
- Task 2: Cannot complete as specified due to budget exhaustion. No measured QED values available for non-m05-series molecules with psa ≥ 40.

## Final Answer
- target_qed: 0.7274
- lead_mol_id: null (cannot identify without measurements)
- lead_qed_measured: null (no budget for measurements)

## Lessons Learned
- Use free tools (group_by_scaffold, fit_trend) before spending budget
- Read only necessary pages from list_pool (not all 9 pages)
- Reserve budget for both tasks: need measurements for m05 series AND for candidate molecules from other series
- Optimal budget allocation might be: 2-3 units for list_pool, 3-4 units for m05 series measurements, 4-5 units for Task 2 candidate measurements
