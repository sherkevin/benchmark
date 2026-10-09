# Medicinal Chemistry Task Notes

## Task Overview
1. **Task 1**: Predict QED for m05 (chain length 11 in scaffold "COc1ccc(cc1)C(=O)N")
2. **Task 2**: Find a molecule with:
   - QED ≥ 0.6
   - psa ≥ 40
   - Not in same congeneric series as m05
3. **Budget**: 12 measurements total for both tasks

## Molecular Structure Analysis
From group_by_scaffold():
- **Scaffold 1**: "COc1ccc(cc1)C(=O)N" - m05's series (m15, m24, m28, m17, m22, m05)
- **Scaffold 2**: "O=[N+]([O-])c1ccc(cc1)C(=O)N" - nitrobenzene series
- **Scaffold 3**: "OCC(O)C(O)C(O)C(=O)N" - polyol series  
- **Scaffold 4**: "c1ccc(cc1)-c1ccc(cc1)C(=O)N" - biphenyl series

## Strategy
1. For Task 1: Measure QED of several molecules from m05's series to establish trend
2. Use fit_trend to predict m05's QED
3. For Task 2: Screen molecules from other series with psa ≥ 40 (Scaffolds 2 and 3)
4. Measure promising candidates to find one with QED ≥ 0.6

## Measurements Plan
Budget allocation:
- Task 1: ~5-6 measurements (to establish good trend)
- Task 2: ~6-7 measurements (screening candidates)

## Task 1 Progress
**Measurements from m05's series (Scaffold 1):**
- m15 (chain_len: 2): QED = 0.7621
- m24 (chain_len: 3): QED = 0.7919  
- m28 (chain_len: 4): QED = 0.7518
- m17 (chain_len: 5): QED = 0.7498
- m22 (chain_len: 6): QED = 0.7377

**Observations:** QED seems to peak around chain length 3 (0.7919) and then decreases gradually.

**QED predictions for m05 (chain_len: 11):**
- Linear fit (degree=1): 0.6950
- Quadratic fit (degree=2): 0.5416 (RMSE: 0.01188)
- Cubic fit (degree=3): 2.1323 (unrealistic, >1.0)

**Decision:** Using quadratic fit prediction of 0.5416 as it seems most reasonable given the downward trend and has lowest RMSE.

**Budget used:** 5 measurements
**Remaining budget:** 7 measurements

## Task 2 Progress
**Screening molecules with psa ≥ 40 (Scaffolds 2 and 3):**

**Scaffold 2 measurements:**
- m09 (chain_len: 2): QED = 0.5826 (< 0.6)
- m02 (chain_len: 3): QED = 0.6046 (≥ 0.6) ✓ MEETS ALL CRITERIA
- m12 (chain_len: 4): QED = 0.4709 (< 0.6)
- m32 (chain_len: 5): QED = 0.4683 (< 0.6)

**Scaffold 3 measurements:**
- m14 (chain_len: 2): QED = 0.3237 (< 0.6)
- m27 (chain_len: 3): QED = 0.3330 (< 0.6)

**Best candidate found:** m02
- QED: 0.6046 (≥ 0.6)
- psa: 72.24 (≥ 40) 
- Scaffold: "O=[N+]([O-])c1ccc(cc1)C(=O)N" (different from m05's scaffold)

**Final budget:**
- Measurements used: 11
- Remaining: 1

## Final Results
**Task 1:** Predicted QED for m05 = 0.5416 (using quadratic fit on chain lengths 2-6)
**Task 2:** Selected m02 with measured QED = 0.6046

**Budget usage:**
- Total measurements: 11
- Remaining: 1
- Within 12-measurement limit ✓

**Answer.json updated with final values.**