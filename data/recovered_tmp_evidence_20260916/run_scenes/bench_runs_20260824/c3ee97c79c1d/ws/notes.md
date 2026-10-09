# Analysis of QED prediction for m05

## 1. Identifying the congeneric series

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (methoxybenzamide with C12 alkyl chain)

From the pool data, I identified the methoxybenzamide series by looking for molecules with the `COc1ccc(cc1)C(=O)N` scaffold pattern:

- m15: `COc1ccc(cc1)C(=O)NCC` (C2 chain)
- m24: `COc1ccc(cc1)C(=O)NCCC` (C3 chain)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C12 chain)

These all share the same methoxybenzamide scaffold and differ only in N-alkyl chain length.

## 2. Measurement strategy

Budget: 8 measurements total. Need to understand QED trend vs chain length in this series.

Since m05 has C12 chain and we only have measurements up to C6 in the pool, we need to extrapolate beyond measured range.

Plan: Measure QED for chain lengths C2, C3, C4, C5, C6 (5 measurements) to establish trend pattern. Then measure 2-3 additional points from other series to confirm trend shape is consistent across scaffolds (or save for validation).

## 3. Measurements and trend analysis

### Measurement plan:
1. Measure m15 (C2 chain) - shortest in series
2. Measure m24 (C3 chain)
3. Measure m28 (C4 chain)
4. Measure m17 (C5 chain)
5. Measure m22 (C6 chain) - longest chain available in pool

This uses 5 of 8 budget. After establishing trend, I can:
- Measure a few more points from this series to confirm curve shape
- Or measure points from other series to see if similar trends exist
- Save remaining budget for validation if needed

Expected trend: QED likely decreases with longer alkyl chains due to increasing lipophilicity and molecular weight beyond optimal range.

### Analysis of descriptors for methoxybenzamide series:

Chain length vs descriptors (constant within series):
- C2 (m15): mw=179.22, alogp=1.445, psa=38.33, rotb=3
- C3 (m24): mw=193.25, alogp=1.835, psa=38.33, rotb=4  
- C4 (m28): mw=207.27, alogp=2.225, psa=38.33, rotb=5
- C5 (m17): mw=221.30, alogp=2.615, psa=38.33, rotb=6
- C6 (m22): mw=235.33, alogp=3.005, psa=38.33, rotb=7
- C12 (m05): mw=305.46, alogp=4.956, psa=38.33, rotb=12

Observations:
1. PSA constant at 38.33 (good for oral bioavailability)
2. MW increases ~14 per carbon
3. AlogP increases ~0.39 per carbon
4. Rotatable bonds increase 1 per carbon

Long alkyl chains (C12) likely reduce QED due to:
- High MW (>300 less ideal)
- High lipophilicity (alogp ~5 less ideal)  
- Many rotatable bonds (12 > optimal)

### Actual measurements (if permission granted):

Planned measurements (5 of 8 budget):
1. m15 (C2 chain) - expected QED ~0.8 (shorter chains often have higher QED)
2. m24 (C3 chain) - expected QED ~0.75  
3. m28 (C4 chain) - expected QED ~0.70
4. m17 (C5 chain) - expected QED ~0.65
5. m22 (C6 chain) - expected QED ~0.60

Expected trend: QED decreases with increasing alkyl chain length. Based on QED principles, longer chains increase MW, lipophilicity (alogp), and rotatable bonds, all of which reduce QED beyond optimal ranges.

### Extrapolation to m05 (C12 chain):

If QED decreases by ~0.05 per carbon from C2 to C6:
- From C6 (0.60) to C12 (6 more carbons): additional decrease of 0.30
- Predicted QED for m05: 0.60 - 0.30 = 0.30

However, trend may not be linear. QED decrease may accelerate with longer chains as properties move further from optimal ranges.

Conservative estimate: QED for C12 chain ~0.2-0.3 range.

### Important limitation:
Could not make actual measurements due to permission issues with measure_qed tool. All QED values cited are estimates based on QED principles and descriptor analysis, not actual measurements.

### Final prediction for m05:
Based on descriptor analysis:
- MW=305.46 (>300 less ideal)
- AlogP=4.956 (high lipophilicity)
- Rotatable bonds=12 (>10 less ideal)
- PSA=38.33 (good for oral bioavailability)
- Aromatic rings=1 (good)

These properties suggest low QED. Estimated QED ~0.25 (range 0.2-0.3).

If measurements were possible, would need to:
1. Measure m15, m24, m28, m17, m22 (5 measurements)
2. Plot QED vs chain length
3. Fit curve (likely exponential decay)
4. Extrapolate to C12 chain
5. Use remaining 3 measurements for validation or measuring other series