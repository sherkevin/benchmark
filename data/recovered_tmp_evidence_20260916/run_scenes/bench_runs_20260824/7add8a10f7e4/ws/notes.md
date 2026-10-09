# Medicinal Chemistry Compound Screening Notes

## Task Overview
- Screen 20 compounds from `data/candidates.tsv`
- Identify compounds that meet both criteria:
  1. Measured `logS` between -3 and -2 inclusive
  2. Measured `bbb` = 1 (brain penetration)
- Budget: 8 ADMET assays total (one per compound tested)

## Strategy
1. First analyze the chemical descriptors to prioritize compounds most likely to meet criteria
2. Use limited assays (8 total) to test compounds with highest probability of success
3. Track assay results and update answer.json with confirmed qualifying compounds

## Initial Analysis of Compounds
The dataset contains 20 compounds with molecular properties:
- Molecular weight (mw): 180.2 to 430.7 Da
- Calculated logP (clogp): -1.64 to 8.84
- Topological polar surface area (tpsa): 23.6 to 119.6
- Hydrogen bond donors (hbd): 0 to 5
- Hydrogen bond acceptors (hba): for solubility (-3 to -2 logS) and bbb=1
- Rotatable bonds (rotb): for brain penetration
- Rings: 2 to 5

## Assay Results
Budget used: 8/8 assays

### Tested Compounds:
1. **z002040**: PASS ✓
   - logS: -2.3374 (within -3 to -2 range)
   - bbb: 1 (brain penetration)
   - Status: QUALIFIES

2. **z003001**: FAIL ✗
   - logS: -0.137 (outside -3 to -2 range, too soluble)
   - bbb: 1 (brain penetration)
   - Status: Does not qualify (solubility too high)

3. **z001999**: FAIL ✗
   - logS: -1.522 (outside -3 to -2 range, too soluble)
   - bbb: 1 (brain penetration)
   - Status: Does not qualify (solubility too high)

4. **z002170**: PASS ✓
   - logS: -2.92 (within -3 to -2 range)
   - bbb: 1 (brain penetration)
   - Status: QUALIFIES

5. **z002746**: FAIL ✗
   - logS: -0.6286 (outside -3 to -2 range, too soluble)
   - bbb: 1 (brain penetration)
   - Status: Does not qualify (solubility too high)

6. **z002052**: PASS ✓
   - logS: -2.7894 (within -3 to -2 range)
   - bbb: 1 (brain penetration)
   - Status: QUALIFIES

7. **z002003**: FAIL ✗
   - logS: -3.6 (outside -3 to -2 range, too insoluble)
   - bbb: 1 (brain penetration)
   - Status: Does not qualify (solubility too low)

8. **z002216**: PASS ✓
   - logS: -2.16 (within -3 to -2 range)
   - bbb: 1 (brain penetration)
   - Status: QUALIFIES

## Summary
- Total compounds tested: 8/20
- Qualifying compounds found: 4
- Budget fully utilized: 8/8 assays

### Qualifying Compounds (meet both criteria):
1. z002040: logS = -2.3374, bbb = 1
2. z002170: logS = -2.92, bbb = 1  
3. z002052: logS = -2.7894, bbb = , bbb = 1
4. z002216: logS = -2.16, bbb = 1

### Failed Compounds:
- z003001: logS too high (-0.137), bbb = 1
- z001999: logS too high (-1.522), bbb = 1
- z002746: logS too high (-0.6286), bbb = 1
- z002003: logS too low (-3.6), bbb = 1

### Observations:
1. All tested compounds had bbb = 1, suggesting brain penetration is common in this dataset
2. The main challenge was finding compounds with logS in the narrow range of -3 to -2
3. Compounds with clogp around 1.5-3.0 seemed to have the best chance of appropriate solubility
4. tpsa values varied widely among successful compounds (34.9 to 98.0)