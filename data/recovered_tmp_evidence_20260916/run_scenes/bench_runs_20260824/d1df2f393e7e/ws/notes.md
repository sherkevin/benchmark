# Notes for Medicinal Chemistry Task

## Task Overview
1. Predict QED for m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
2. Find a backup lead compound meeting all three constraints:
   - Measured QED ≥ 0.6
   - psa ≥ 40
   - Not in the same series as m05

Budget: 17 total measurements shared between both tasks.

## Step 1: Understanding m05
Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxybenzamide (`COc1ccc(cc1)C(=O)N`)
- Alkyl chain: `CCCCCCCCCCCC` (12-carbon alkyl chain)
.### This is a long-chain congener

## Step 2: Pool Exploration Complete
- Budget used for listing: 9 units
- Remaining budget for measurements: 8 units
- Total molecules in pool: 33

## Step 3: Identifying Congeners of m05
Based on SMILES analysis, congeners (same scaffold, different alkyl chain length) identified:
- m15: `COc1ccc(cc1)C(=O)NCC` (2-carbon chain)
- m24: `COc1ccc(cc1)C(=O)NCCC` (3-carbon chain)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4-carbon chain)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5-carbon chain)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6-carbon chain)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12-carbon chain) - target

## Step 4: QED Measurements for Congeners
Measurements completed (5 measurements used, total budget used: 14/17):
### Chain Length vs QED Data:
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919 (peak)
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

## Step 5: Trend Analysis and Prediction for m05
Data: Chain length vs QED for methoxybenzamide series
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518
- 5C: 0.7498
- 6C: 0.7377

### Trend Analysis:
1. QED shows parabolic-like behavior with peak at 3-carbon chain
2. Beyond peak, QED decreases with increasing chain length
3. Rate of decrease: 
   - 3C→4C: -0.0401
   - 4C→5C: -0.0020  
   - 5C→6C: -0.0121
   - Average: -0.0181 per carbon

### Extrapolation to m05 (12-carbon chain):
Linear extrapolation from 6C:
- 6 more carbons beyond measured range
- Using average decrease: 0.7377 - (6 × 0.0181) = 0.6291
- Considering variability in decrease rate: estimated range 0.60-0.65

### Additional Factors:
1. m05 alogp = 4.956 (vs 3.005 for 6C) indicates high lipophilicity
2. High lipophilicity generally reduces QED (drug-likeness)
3. Long alkyl chains (12C) often reduce solubility and increase metabolic instability

### Final Prediction:
Conservative estimate: QED = 0.58
- Accounts for potential acceleration in QED decrease with very long chains
- Consistent with high alogp value
- Within reasonable extrapolation range

## Step 6: Lead Compound Search Results
Measurements for potential leads (non-congener, psa ≥ 40):
- m09 (nitrobenzamide, 2C): QED = 0.5826 (psa=72.24) - below 0.6 threshold
- m12 (nitrobenzamide, 4C): QED = 0.4709 (psa=72.24) - below 0.6
- m14 (polyol, 2C): QED = 0.3237 (psa=110.02) - below 0.6

No candidate meets all three constraints (QED ≥ 0.6, psa ≥ 40, not same series as m05).

Selected m09 as lead despite QED < 0.6 because:
1. It has the highest QED among measured non-congeners
2. It meets psa ≥ 40 constraint
3. It is not in the same series as m05
4. Alternative would be to leave lead_mol_id blank, but answer.json requires a value

Note: With better budget management, would have measured m02 (3C nitrobenzamide) which might have QED ≥ 0.6 based on trend patterns observed in other series.

## Summary
- Predicted m05 QED: 0.58 (based on declining trend with chain length)
- Selected lead: m09 (nitrobenzamide, 2C) with measured QED: 0.5826
- Congeners of m05: m15, m24, m28, m17, m22, m05
- Budget fully utilized: 17/17 units

## Lessons Learned
1. Should have first identified all potential lead candidates (psa ≥ 40, not same series)
2. Should have measured promising lead candidates before exhaustive congener measurements
3. Should have conserved list_pool budget by searching more strategically

## Step 7: Budget Status
Total budget: 17 units
Used: 9 (list_pool) + 8 (measurements) = 17 units
No budget remaining.

## Step 5: Lead Compound Search
Need to find molecule meeting:
1. Measured QED ≥ 0.6
2. psa ≥ 40 (from descriptors)
3. Not in same series as m05 (different scaffold)