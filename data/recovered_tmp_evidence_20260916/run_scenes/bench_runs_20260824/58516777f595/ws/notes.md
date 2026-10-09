# Notes

## Task
Find all compounds with measured logS between -3 and -2 (inclusive) AND bbb = 1.
Budget: 8 assays total.

## Compound Analysis (20 compounds)

### Likely qualifiers (moderate clogP 1.5-3.5, low TPSA <60 for BBB):
- z002170: clogP=3.0, TPSA=34.9, MW=250.3
- z003001: clogP=2.9, TPSA=29.1, MW=237.7
- z001999: clogP=1.5, TPSA=41.9, MW=299.4
- z002040: clogP=1.77, TPSA=46.2, MW=217.3
- z002746: clogP=1.55, TPSA=30.2, MW=231.3
- z002003: clogP=3.58, TPSA=43.1, MW=308.8

### Possible but riskier:
- z002010: clogP=4.28, TPSA=29.5 (good BBB, solubility might be too low)
- z002011: clogP=4.32, TPSA=30.2 (good BBB, solubility might be too low)
- z002015: clogP=4.14, TPSA=23.6 (good BBB, solubility concern)
- z002891: clogP=4.72, TPSA=34.1 (good BBB, solubility concern)
- z002052: clogP=1.99, TPSA=65.1 (borderline TPSA for BBB)

### Unlikely:
- z000889: clogP=8.84 → way too lipophilic
- z001510: clogP=-1.04 → too hydrophilic
- z001543: clogP=-1.64, TPSA=119.6 → too hydrophilic, no BBB
- z002216: TPSA=98 → unlikely BBB
- z002683: TPSA=100.7 → unlikely BBB
- z002730: clogP=0.09 → logS probably > -2
- z015334: clogP=0.74, TPSA=81.6 → probably too soluble and no BBB
- z000364: TPSA=83.1 → unlikely BBB

## Selected 8 for assay:
1. z002170
2. z003001
3. z001999
4. z002040
5. z002746
6. z002003
7. z002010
8. z002011

## Assay Results:
| mol_id  | logS      | bbb | Qualifies? |
|---------|-----------|-----|------------|
| z002170 | -2.92     | 1   | YES (-3 ≤ -2.92 ≤ -2) |
| z003001 | -0.137    | 1   | NO (logS > -2, too soluble) |
| z001999 | -1.522    | 1   | NO (logS > -2, too soluble) |
| z002040 | -2.3374   | 1   | YES (-3 ≤ -2.3374 ≤ -2) |
| z002746 | -0.6286   | 1   | NO (logS > -2, too soluble) |
| z002003 | -3.6      | 1   | NO (logS < -3, too insoluble) |
| z002010 | -5.0097   | 1   | NO (logS < -3, too insoluble) |
| z002011 | -4.13     | 1   | NO (logS < -3, too insoluble) |

## Summary
- All 8 tested compounds had bbb=1 (all selected had low TPSA, good for BBB)
- Only z002170 and z002040 had logS in the target window [-3, -2]
- Compounds with clogP ~1.5-2.0 were too soluble (logS > -2): z003001, z001999, z002746
- Compounds with clogP ~3.5+ were too insoluble (logS < -3): z002003, z002010, z002011
- The sweet spot was clogP ~1.8-3.0

## Untested compounds (budget exhausted):
Remaining 12 compounds were not tested. Based on descriptors:
- Most with very low or very high clogP would likely fail the logS criterion
- Most with high TPSA (>70) would likely fail BBB
- Some compounds like z002052 (clogP=1.99, TPSA=65.1) or z002764 (clogP=3.56, TPSA=66.8) might have been worth testing

## Final Answer
Qualifying compounds: z002170, z002040
