# Notes

## Task 1: Predict QED of m05

m05 is in scaffold `COc1ccc(cc1)C(=O)N` with chain_len=11.
Members of this series: m15 (2), m24 (3), m28 (4), m17 (5), m22 (6), m05 (11)

Measured molecules from m05's series:
- m15: chain_len=2, QED=0.7621
- m24: chain_len=3, QED=0.7919
- m28: chain_len=4, QED=0.7518
- m17: chain_len=5, QED=0.7498
- m22: chain_len=6, QED=0.7377

Fit polynomial trend (degree 2) and predict at chain_len=11:
- **Predicted QED for m05: 0.5416**

## Task 2: Find a lead molecule

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in m05's scaffold series (COc1ccc(cc1)C(=O)N)

Measurements made:
- m14: QED=0.3237, psa=110.02 (below 0.6)
- m27: QED=0.333, psa=110.02 (below 0.6)
- m01: QED=0.3187, psa=110.02 (below 0.6)
- m19: QED=0.3244, psa=110.02 (below 0.6)
- m03: QED=0.3282, psa=110.02 (below 0.6)
- m09: QED=0.5826, psa=72.24 (below 0.6)
- m02: QED=0.6046, psa=72.24 ✓

**Selected molecule: m02**
- QED = 0.6046 (≥ 0.6) ✓
- psa = 72.24 (≥ 40) ✓
- Scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N (different from m05's COc1ccc(cc1)C(=O)N) ✓

## Budget usage
- 5 measurements for m05 series trend
- 7 measurements for Task 2 candidates
- Total: 12 (budget exhausted)
