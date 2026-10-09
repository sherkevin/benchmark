# Compound Analysis Notes

## Task Overview
- Need to find compounds with logS between -3 and -2 (inclusive) AND bbb = 1
- Budget: 8 assays total
- Will prioritize compounds based on their properties in the TSV file

## Initial Analysis
The compounds in the dataset have various properties. Based on known relationships between molecular properties and solubility/brain penetration, I'll prioritize testing:

1. Compounds with moderate clogP values (not too hydrophobic) for better solubility potential
2. Compounds with reasonable polar surface area (TPSA) values
3. Avoiding compounds that are likely to be too lipophilic (high clogP)

## Testing Plan
I'll start testing compounds that seem most likely to meet our criteria based on their descriptors.

## Test Results

### Test 1: z001999
- Result: logS = -1.522, bbb = 1
- Analysis: logS is too high (> -2), doesn't meet criteria

### Test 2: z002040
- Result: logS = -2.3374, bbb = 1
- Analysis: Qualifies! logS is between -3 and -2, bbb = 1

### Test 3: z002746
- Result: logS = -0.6286, bbb = 1
- Analysis: logS is too high (> -2), doesn't meet criteria

### Test 4: z002003
- Result: logS = -3.6, bbb = 1
- Analysis: logS is too low (< -3), doesn't meet criteria

### Test 5: z002015
- Result: logS = -3.82, bbb = 1
- Analysis: logS is too low (< -3), doesn't meet criteria

### Test 6: z001510
- Result: logS = -2.7372, bbb = 1
- Analysis: Qualifies! logS is between -3 and -2, bbb = 1

### Test 7: z002216
- Result: logS = -2.16, bbb = 1
- Analysis: Qualifies! logS is between -3 and -2, bbb = 1

### Test 8: z001543
- Result: logS = -0.8547, bbb = 1
- Analysis: logS is too high (> -2), doesn't meet criteria

## Final Results
Three compounds met both criteria:
- z002040: logS = -2.3374, bbb = 1
- z001510: logS = -2.7372, bbb = 1
- z002216: logS = -2.16, bbb = 1

These compounds were added to answer.json as qualifying_ids.