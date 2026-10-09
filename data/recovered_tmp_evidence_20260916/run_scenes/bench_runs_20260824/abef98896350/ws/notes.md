# Notes for Compound Selection Task

## Task Overview
We have 20 compounds to evaluate for:
1. Measured logS between -3 and -2 (inclusive)
2. Measured bbb = 1

We have a budget of 8 assay measurements total.

## Initial Analysis
The dataset contains 20 compounds with various descriptors:
- Molecular weight (mw)
- Calculated logP (clogp)
- Total polar surface area (tpsa)
- Hydrogen bond donors (hbd)
- Hydrogen bond acceptors (hba)
- Rotatable bonds (rotb)
- Rings

## Strategy
Since we only have 8 assays available (40% of compounds), we need to be selective. We should prioritize compounds that are most likely to meet both criteria based on their calculated properties.

## Key Considerations for Selection:

### For BBB penetration (bbb=1):
Compounds are more likely to cross BBB if:
- Lower molecular weight (<450)
- Moderate logP (typically 0-3)
- Lower polar surface area (<90 Å²)
- Limited hydrogen bond donors (<3)

### For Solubility (logS between -3 and -2):
Moderate solubility range suggests compounds should have some polarity but not too much. Compounds with:
- Moderate clogp (neither too lipophilic nor too hydrophilic)
- Some polar groups but not excessive
- Reasonable molecular weight

## Selection Priority Order
I'll need to select 8 compounds for testing based on these heuristics.

## Selected Compounds for Testing
Based on ADMET heuristics, I've selected these 8 compounds for assay testing:

1. **z001999**: MW=299.4, clogP=1.5, tpsa=41.9, HBD=1 - Good balance
2. **z002003**: MW=308.8, clogP=3.58, tpsa=43.1, HBD=0 - Moderate properties
3. **z002040**: MW=217.3, clogP=1.77, tpsa=46.2, HBD=1 - Low MW, good balance
4. **z002170**: MW=250.3, clogP=3.0, tpsa=34.9, HBD=0 - Good BBB potential
5. **z002746**: MW=231.3, clogP=1.55, tpsa=30.2, HBD=0 - Excellent BBB potential
6. **z003001**: MW=237.7, clogP=2.9, tpsa=29.1, HBD=1 - Good overall profile
7. **z001510**: MW=180.2, clogP=-1.04, tpsa=72.7, HBD=1 - Low MW but hydrophilic
8. **z002730**: MW=258.2, clogP=0.09, tpsa=83.6, HBD=1 - Very hydrophilic

## Testing Results

### Test 1: z001999
- Result: logS = -1.522, bbb = 1
- Analysis: BBB penetration good, but too soluble (outside -3 to -2 range)
- Status: Does NOT qualify

### Test 2: z002003
- Result: logS = -3.6, bbb = 1
- Analysis: BBB penetration good, but too insoluble (outside -3 to -2 range)
- Status: Does NOT qualify

### Test 3: z002040
- Result: logS = -2.3374, bbb = 1
- Analysis: Perfect! Within target solubility range (-3 to -2) and has BBB penetration
- Status: QUALIFIES

### Test 4: z002170
- Result: logS = -2.92, bbb = 1
- Analysis: Within target solubility range (-3 to -2) and has BBB penetration
- Status: QUALIFIES

### Test 5: z002746
- Result: logS = -0.6286, bbb = 1
- Analysis: BBB penetration good, but too soluble (outside -3 to -2 range)
- Status: Does NOT qualify

### Test 6: z003001
- Result: logS = -0.137, bbb = 1
- Analysis: BBB penetration good, but too soluble (outside -3 to -2 range)
- Status: Does NOT qualify

### Test 7: z001510
- Result: logS = -2.7372, bbb = 1
- Analysis: Within target solubility range (-3 to -2) and has BBB penetration
- Status: QUALIFIES

### Test 8: z002730
- Result: logS = -2.6758, bbb = 1
- Analysis: Within target solubility range (-3 to -2) and has BBB penetration
- Status: QUALIFIES

## Summary
Used all 8 assay measurements. Found 4 qualifying compounds:
1. z002040 - logS: -2.3374, bbb: 1
2. z002170 - logS: -2.92, bbb: 1
3. z001510 - logS: -2.7372, bbb: 1
4. z002730 - logS: -2.6758, bbb: 1

All 4 compounds meet both criteria:
- Measured logS between -3 and -2 (inclusive)
- Measured bbb = 1

Budget exhausted (8/8 assays used).