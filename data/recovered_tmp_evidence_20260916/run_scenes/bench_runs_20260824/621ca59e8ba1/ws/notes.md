# Notes

## Approach

### Task 1: Predict QED for m05
- m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so measure_qed returns probe_unavailable
- m05 belongs to scaffold group `COc1ccc(cc1)C(=O)N` with chain_len=11
- Other members of this congeneric series: m15 (len=2), m24 (len=3), m28 (len=4), m17 (len=5), m22 (len=6)

**Measurements taken:**
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518

**Trend fitting:**
- Linear fit (degree=1) through points (2, 0.7621), (3, 0.7919), (4, 0.7518)
- Prediction for chain_len=11: QED = 0.7274

### Task 2: Find lead molecule
Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. Not in m05's congeneric series (scaffold `COc1ccc(cc1)C(=O)N`)

**Problem:** Budget exhausted before measuring any non-m05-series molecules.

Budget usage:
- list_pool: 9 pages × 1 = 9 units
- measure_qed: 3 successful (m15, m24, m28) × 1 = 3 units
- Total: 12/12 units

All measured molecules (m15, m24, m28) are in m05's congeneric series, so they don't satisfy constraint 3.

**Potential candidates (psa ≥ 40, not in m05 series):**
- Scaffold 2 (nitro): m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
- Scaffold 3 (sugar-like): m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)

Would need to measure one of these to verify QED ≥ 0.6.

## Lessons Learned
- Should have been more conservative with list_pool (could have stopped after ~5 pages)
- Should have prioritized measuring Task 2 candidates before m05 series
- Optimal strategy: measure 1-2 non-m05 molecules first, then 2-3 m05 series molecules

## QED Component Analysis
Looking at measured molecules:
- m15 (QED=0.7621): MW=0.33 (low), other components good
- m24 (QED=0.7919): MW=0.41, balanced
- m28 (QED=0.7518): MW=0.49, but ALERTS=0.64 (lower)

Key insight: MW and ALERTS are major drivers. Very low or high MW hurts QED.

## Task 2 Candidate Analysis
Without measurements, analyzed descriptors:
- m02: mw=208.22 (good), alogp=1.735 (good), psa=72.24, rotb=4, arom=1
  - Nitro group might be structural alert
  - Overall properties suggest QED could be ≥ 0.6
- m09: mw=194.19, alogp=1.345, psa=72.24, rotb=3, arom=1
  - Lower MW but balanced properties
  - Could also have QED ≥ 0.6

## Final Answer (with caveat)
- target_qed: 0.7274 (predicted for m05 via linear trend from m15, m24, m28)
- lead_mol_id: m02 (estimated QED ≥ 0.6 based on descriptor analysis, but NOT measured)
- lead_qed_measured: Unknown (budget exhausted before measurement)

Note: Strategic error in budget allocation prevented proper Task 2 completion.
Should have measured non-m05 series molecules first.
