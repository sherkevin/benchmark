# ADMET Screening Notes

## Task Overview
- 20 compounds in `data/candidates.tsv`
- Need to identify compounds with:
  1. Measured logS between -3 and -2 (inclusive)
  2. Measured bbb = 1 (brain penetration)
- Budget: 8 assays total (8 calls to `measure_admet`)
- Must confirm each reported compound with actual assay measurements

## Initial Analysis
The compounds have various descriptors:
- mw (molecular weight)
- clogp (calculated logP)
- tpsa (topological polar surface area)
- hbd (hydrogen bond donors)
- hba (hydrogen bond acceptors)
- rotb (rotatable bonds)
- rings (number of rings)

## Strategy
1. Analyze the descriptors to prioritize compounds most likely to meet criteria
2. Target compounds likely to have good solubility (-3 ≤ logS ≤ -2) AND brain penetration (bbb=1)
3. Use all 8 assays efficiently
4. Document all assay results

## Notes on Criteria
- logS range -3 to -2 means moderate solubility (10⁻³ to 10⁻² M)
- bbb=1 means the compound crosses blood-brain barrier
- Compounds with extreme logP or high polar surface area may be poor candidates

## Compound Analysis

Based on typical ADME properties for brain penetration:
- MW should be < 400-450 Da
- clogP ideally 2-3 for balance of solubility/permeability
- TPSA < 60-70 Å² for good permeability
- HBD ≤ .2 for good permeability
- Rotatable bonds < 10 for good oral bioavailability

Looking at compounds:
1. z000364: MW 399.4, clogP 2.87, TPSA 83.1 (high), HBD 1 - TPSA high, borderline
2. z000889: MW 430.7, clogP 8.84 (very high), TPSA 29.5, HBD 1 - clogP too high
3. z001510: MW 180.2, clogP -1.04 (low), TPSA 72.7, HBD 1 - clogP very low
4. z001543: MW 286.3, clogP -1.64 (low), TPSA 119.6 (very high), HBD 5 - poor
5. z001999: MW 299.4, clogP 1.5, TPSA 41.9, HBD 1 - promising
6. z002003: MW 308.8, clogP 3.58, TPSA 43.1, HBD 0 - promising
7. z002010: MW 339.5, clogP 4.28, TPSA 29.5, HBD 0 - clogP high
8. z002011: MW 325.8, clogP 4.32, TPSA 30.2, HBD 0 - clogP high
9. z002015: MW 336.5, clogP 4.14, TPSA 23.6, HBD 0 - clogP high
10. z002040: MW 217.3, clogP 1.77, TPSA 46.2, HBD 1 - promising
11. z002052: MW 369.4, clogP 1.99, TPSA 65.1, HBD 0 - promising
12. z002170: MW 250.3, clogP 3.0, TPSA 34.9, HBD 0 - promising
13. z002216: MW 250.3, clogP 0.86, TPSA 98.0 (high), HBD 2 - TPSA high
14. z002683: MW 273.2, clogP 2.55, TPSA 100.7 (very high), HBD 2 - TPSA very high
15. z002730: MW 258.2, clogP 0.09, TPSA 83.6 (high), HBD 1 - TPSA high
16. z002746: MW 231.3, clogP 1.55, TPSA 30.2, HBD 0 - promising
17. z002764: MW 318.3, clogP 3.56, TPSA 66.8, HBD 2 - borderline TPSA
18. z002891: MW 314.5, clogP 4.72, TPSA 34.1, HBD 0 - clogP high
19. z003001: MW 237.7, clogP 2.9, TPSA 29.1, HBD 1 - promising
20. z015334: MW 306.3, clogP 0.74, TPSA 81.6 (high), HBD 1 - TPSA high

## Assay Results

### z002746
- logS: -0.6286 (outside target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: Does NOT qualify - logS too high (too soluble)

### z002003
- logS: -3.6 (outside target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: Does NOT qualify - logS too low (too insoluble)

### z002170
- logS: -2.92 (within target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: QUALIFIES!

### z003001
- logS: -0.137 (outside target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: Does NOT qualify - logS too high (too soluble)

### z001999
- logS: -1.522 (outside target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: Does NOT qualify - logS too high (too soluble)

### z002040
- logS: -2.3374 (within target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: QUALIFIES!

### z002052
- logS: -2.7894 (within target range -3 to -2)
- bbb: 1 (passes brain penetration)
- Status: QUALIFIES!

### z002764
- logS: -2.9008 (within target range -3 to -2)
- bbb: 0 (does NOT pass brain penetration)
- Status: Does NOT qualify - no brain penetration

## Summary
Used all 8 assays. Found 3 qualifying compounds:
1. z002170 - logS: -2.92, bbb: 1
2. z002040 - logS: -2.3374, bbb: 1  
3. z002052 - logS: -2.7894, bbb: 1

All qualifying compounds have been confirmed with actual assay measurements.

### Tested compounds and results:
- z002746: logS -0.6286, bbb 1 - logS too high
- z002003: logS -3.6, bbb 1 - logS too low
- z002170: logS -2.92, bbb 1 - QUALIFIES
- z003001: logS -0.137, bbb 1 - logS too high
- z001999: logS -1.522, bbb 1 - logS too high
- z002040: logS -2.3374, bbb 1 - QUALIFIES
- z002052: logS -2.7894, bbb 1 - QUALIFIES
- z002764: logS -2.9008, bbb 0 - no brain penetration

### Promising candidates based on properties:
- z001999 (MW 299.4, clogP 1.5, TPSA 41.9, HBD 1) - TESTED
- z002040 (MW 217.3, clogP 1.77, TPSA 46.2, HBD 1) - TESTED & QUALIFIES
- z002052 (MW 369.4, clogP 1.99, TPSA 65.1, HBD 0) - TESTED & QUALIFIES
- z002170 (MW 250.3, clogP 3.0, TPSA 34.9, HBD 0) - TESTED & QUALIFIES
- z002746 (MW 231.3, clogP 1.55, TPSA 30.2, HBD 0) - TESTED
- z003001 (MW 237.7, clogP 2.9, TPSA 29.1, HBD 1) - TESTED