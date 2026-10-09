# Task Notes

## Task 1: Predict QED for m05
- Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so direct measurement returns probe_unavailable
- Identified that m05 belongs to the scaffold "COc1ccc(cc1)C(=O)N" with N-alkyl chain length 11
- Other molecules in this series: m15 (chain_len=2), m24 (chain_len=3), m28 (chain_len=4), m17 (chain_len=5), m22 (chain_len=6)
- Measured QED values for series members:
  - m15: 0.7621
  - m24: 0.7919
  - m28: 0.7518
  - m17: 0.7498
  - m22: 0.7377
- Used fit_trend with chain lengths [2, 3, 4, 5, 6] and corresponding QED values to predict QED for chain length 11
- Predicted QED for m05: 0.5416

## Task 2: Find molecule meeting all three criteria
Criteria:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same congeneric series as m05

Molecules with PSA ≥ 40:
- m01, m03, m07, m14, m18, m19, m21, m25, m27 (PSA = 110.02) - polyol scaffold
- m02, m06, m09, m10, m12, m29, m31, m32, m33 (PSA = 72.24) - nitrobenzene scaffold

I measured several polyol compounds but they all had low QED (~0.32), well below the 0.6 threshold.
I ran out of measurement budget trying to measure all polyol compounds, so I was unable to measure any nitrobenzene compounds which also met the criteria of PSA ≥ 40 and not being in the same series as m05.

Since I exhausted my measurement budget and did not find a suitable candidate, I cannot fulfill Task 2 completely. The nitrobenzene compounds (m02, m06, m09, m10, m12, m29, m31, m32, m33) would have been good candidates to measure next, as they satisfy both the PSA ≥ 40 and not-in-same-series-as-m05 criteria.