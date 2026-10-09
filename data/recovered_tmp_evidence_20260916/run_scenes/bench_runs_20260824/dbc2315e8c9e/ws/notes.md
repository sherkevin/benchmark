# ADMET Screening Notes

## Task Overview
- Screen 20 compounds from data/candidates.tsv for ADMET properties
- Target profile: logS between -3 and -2 (inclusive) AND bbb = 1
- Only 8 assay calls available (budget constraint)
- Need to identify qualifying compounds through actual measurements

## Initial Analysis of Compounds
Looking at the 20 compounds, I should prioritize candidates likely to meet both criteria:
1. logS between -3 and -2 (moderate solubility)
2. bbb = 1 (good brain penetration)

From medicinal chemistry knowledge:
- logS range -3 to -2 suggests moderate lipophilicity compounds
- Brain penetration (bbb=1) typically requires:
  - Moderate molecular weight (ideally <500)
  - Low polar surface area (TPSA < 90 Å²)
  - Moderate lipophilicity (cLogP ~2-5)
  - Limited hydrogen bond donors (<3)
  
I'll examine the data and prioritize compounds based on these criteria.

## Compound Analysis

Based on the data, here's my assessment of promising candidates:

### Promising Candidates for Screening:
1. **z000364** - MW: 399.4, cLogP: 2.87, TPSA: 83.1, HBD: 1, HBA: 6
   - Reasonable MW, moderate cLogP, TPSA borderline high but HBD=1 good

2. **z001999** - MW: 299.4, cLogP: 1.5, TPSA: 41.9, HBD: 1, HBA: 4  
   - Good properties: low MW, moderate cLogP, low TPSA, HBD=1

3. **z002040** - MW: 217.3, cLogP: 1.77, TPSA: 46.2, HBD: 1, HBA: 2
   - Excellent properties: low MW, good cLogP, low TPSA, HBD=1

4. **z002052** - MW: 369.4, cLogP: 1.99, TPSA: 65.1, HBD: 0, HBA: 6
   - Good cLogP, reasonable MW, HBD=0 (good for bbb), TPSA moderate

5. **z002170** - MW: 250.3, cLogP:７: 3.0, TPSA: 34.9, HBD: 0, HBA: 2
   - Very promising: low MW, good cLogP, very low TPSA, HBD=0

6. **z002730** - MW: 258.2, cLogP: 0.09, TPSA: 83.6, HBD: 1, HBA: 4
   - Borderline: cLogP very low (might be too soluble), TPSA high

7. **z002746** - MW: 231.3, cLogP: 1.55, TPSA: 30.2, HBD: 0, HBA: 2
   - Very promising: low MW, good cLogP, very low TPSA, HBD=0

8. **z003001** - MW: 237.7, cLogP: 2.9, TPSA: 29.1, HBD: 1, HBA: 2
   - Excellent properties: low MW, good cLogP, very low TPSA, HBD=1

### Less Promising Candidates (likely not to qualify):
- **z000889** - cLogP 8.84 (too high - likely poor solubility)
- **z001510** - cLogP -1.04 (too low - likely too soluble)
- **z001543** - HBD=5, TPSA=119.6 (too polar for brain penetration)
- **z002003** - cLogP 3.58, but HBD=0 might be okay but TPSA low
- **z002010** - cLogP 4.28 (somewhat high)
- **z002011** - cLogP 4.32 (somewhat high)
- **z002015** - cLogP 4.14 (somewhat high)
- **z002216** - TPSA 98.0 (too high for brain penetration)
- **z002683** - TPSA 100.7 (too high for brain penetration)
- **z002764** - cLogP 3.56, HBD=2, TPSA 66.8 (borderline)
- **z002891** - cLogP 4.72 (high)
- **z015334** - TPSA 81.6 (borderline high)

## Screening Strategy
With only 8 assays available, I'll prioritize the most promising 8 compounds based on:
1. Moderate cLogP (1.5-3.5 range for likely logS -3 to -2)
2. Low TPSA (<60 for better brain penetration)
3. Low HBD count (≤1 ideal for brain penetration)
4. Reasonable molecular weight (<400)

My initial selection for testing (8 compounds):
1. z001999 - Excellent profile
2. z002040 - Excellent profile  
3. z002170 - Very promising
4. z002746 - Very promising
5. z003001 - Excellent profile
6. z000364 - Borderline but worth testing
7. z002052 - Worth testing despite HBA=6
8. z002730 - Borderline but worth testing due to cLogP=0.09 (might hit logS target)

## Assay Results

### Test 1: z002170
- **Result**: SUCCESS!
- logS: -2.92 (qualifies: -3 ≤ -2.92 ≤ -2)
- bbb: 1 (qualifies)
- **Status**: QUALIFYING COMPOUND

### Test 2: z002746
- **Result**: FAIL (logS out of range)
- logS: -0.6286 (too soluble, outside -3 to -2 range)
- bbb: 1 (good brain penetration)
- **Status**: NOT QUALIFYING

### Test 3: z003001
- **Result**: FAIL (logS out of range)
- logS: -0.137 (too soluble, outside -3 to -2 range)
- bbb: 1 (good brain penetration)
- **Status**: NOT QUALIFYING

### Test 4: z002040
- **Result**: SUCCESS!
- logS: -2.3374 (qualifies: -3 ≤ -2.3374 ≤ -2)
- bbb: 1 (qualifies)
- **Status**: QUALIFYING COMPOUND

### Test 5: z001999
- **Result**: FAIL (logS out of range)
- logS: -1.522 (too soluble, outside -3 to -2 range)
- bbb: 1 (good brain penetration)
- **Status**: NOT QUALIFYING

### Test 6: z002052
- **Result**: SUCCESS!
- logS: -2.7894 (qualifies: -3 ≤ -2.7894 ≤ -2)
- bbb: 1 (qualifies)
- **Status**: QUALIFYING COMPOUND

### Test 7: z000364
- **Result**: FAIL (both criteria)
- logS: -0.9482 (too soluble, outside -3 to -2 range)
- bbb: 0 (poor brain penetration)
- **Status**: NOT QUALIFYING

### Test 8: z002730
- **Result**: SUCCESS!
- logS: -2.6758 (qualifies: -3 ≤ -2.6758 ≤ -2)
- bbb: 1 (qualifies)
- **Status**: QUALIFYING COMPOUND

## Summary
Tested 8 compounds using all available assays. Found 4 qualifying compounds that meet both criteria (logS between -3 and -2 inclusive, and bbb=1):

1. **z002170** - logS: -2.92, bbb: 1
2. **z002040** - logS: -2.3374, bbb: 1  
3. **z002052** - logS: -2.7894, bbb: 1
4. **z002730** - logS: -2.6758, bbb: 1

Non-qualifying compounds tested:
- z002746 - logS: -0.6286 (too soluble)
- z003001 - logS: -0.137 (too soluble)
- z001999 - logS: -1.522 (too soluble)
- z000364 - logS: -0.9482 (too soluble), bbb: 0

All 8 assays used. No assays remaining.

## Task Completion
- Successfully screened 20 compounds with 8 assay budget
- Identified 4 qualifying compounds through actual measurements
- Updated answer.json with qualifying IDs
- Created comprehensive notes.md documenting the process