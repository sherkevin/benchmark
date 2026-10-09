# Notes

## Task 1: Predict QED for m05

m05 is in scaffold group "COc1ccc(cc1)C(=O)N" with chain_len=11. Other members:
- m15: chain_len=2, QED=0.7621
- m24: chain_len=3, QED=0.7919
- m28: chain_len=4, QED=0.7518
- m17: chain_len=5, QED=0.7498
- m22: chain_len=6, QED=0.7377

Used fit_trend with degree=2 polynomial to extrapolate to chain_len=11:
- Prediction: QED = 0.5416

## Task 2: Find lead molecule

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in m05's scaffold series ("COc1ccc(cc1)C(=O)N")

Scaffold groups with psa ≥ 40:
- "O=[N+]([O-])c1ccc(cc1)C(=O)N": psa=72.24 (m09, m02, m12, m32, m33, m29, m31, m06, m10)
- "OCC(O)C(O)C(O)C(=O)N": psa=110.02 (m14, m27, m01, m19, m03, m21, m18, m07, m25)

Measurements:
- m02: QED=0.6046, psa=72.24, scaffold="O=[N+]([O-])c1ccc(cc1)C(=O)N" → QUALIFIES
- m12: QED=0.4709 → fails QED constraint
- m01: QED=0.3187 → fails QED constraint

Selected lead: m02 (QED=0.6046, psa=72.24, not in m05's series)

## Budget
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17 units (full budget used)
