# Notes

## Task 1: Predict QED for m05

m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so I needed to predict its QED from its congeneric series.

### Scaffold grouping
m05 belongs to scaffold `COc1ccc(cc1)C(=O)N` with chain length 11. Other members:
- m15 (chain_len=2)
- m24 (chain_len=3)
- m28 (chain_len=4)
- m17 (chain_len=5)
- m22 (chain_len=6)

### Measurements for trend fitting
Measured QED for all available members of the same scaffold:
- m15: QED = 0.7621
- m24: QED = 0.7919
- m28: QED = 0.7518
- m17: QED = 0.7498
- m22: QED = 0.7377

### Polynomial fit
Used fit_trend with degree=2 polynomial on chain lengths [2,3,4,5,6] and QED values above.
Predicted QED for chain_len=11 (m05): **0.5416**

## Task 2: Find a lead molecule

Constraints:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in same congeneric series as m05 (scaffold A)

### Analysis
- Scaffold A (m05's series): psa=38.33 - excluded anyway
- Scaffold B (nitro series): psa=72.24 ≥ 40 ✓
- Scaffold C (sugar-like series): psa=110.02 ≥ 40 ✓
- Scaffold D (biphenyl series): psa=29.1 < 40 - excluded

### Measurements
Measured candidates from valid scaffolds:
- m02 (Scaffold B): QED = 0.6046 ✓ (meets all criteria)
- m12 (Scaffold B): QED = 0.4709 ✗
- m01 (Scaffold C): QED = 0.3187 ✗

### Selected lead
**m02** satisfies all constraints:
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Different scaffold than m05 ✓

## Budget usage
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 (budget exhausted)
