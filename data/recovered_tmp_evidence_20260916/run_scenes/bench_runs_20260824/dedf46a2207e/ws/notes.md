# Notes

## Task
- 20 compounds, budget of 8 assays
- Qualify if: logS between -3 and -2 (inclusive) AND bbb = 1

## Compound Analysis (from TSV descriptors)

Compounds evaluated by descriptors for BBB (low TPSA <90, low HBD, moderate clogp 1-4, MW<400) and moderate solubility (clogp ~1-4):

**Likely candidates (assay these 8):**
1. z001999: clogp=1.5, TPSA=41.9, HBD=1, MW=299.4 → good BBB, moderate sol
2. z002003: clogp=3.58, TPSA=43.1, HBD=0, MW=308.8 → good BBB
3. z002040: clogp=1.77, TPSA=46.2, HBD=1, MW=217.3 → good BBB, small
4. z002052: clogp=1.99, TPSA=65.1, HBD=0, MW=369.4 → good BBB
5. z002170: clogp=3.0, TPSA=34.9, HBD=0, MW=250.3 → good BBB
6. z002746: clogp=1.55, TPSA=30.2, HBD=0, MW=231.3 → good BBB
7. z003001: clogp=2.9, TPSA=29.1, HBD=1, MW=237.7 → good BBB
8. z000364: clogp=2.87, TPSA=83.1, HBD=1, MW=399.4 → borderline BBB

**Skipped (likely won't qualify):**
- z000889: clogp=8.84 → too lipophilic, logS too low
- z001510: clogp=-1.04 → too hydrophilic, logS too high
- z001543: clogp=-1.64, TPSA=119.6 → too hydrophilic, poor BBB
- z002010: clogp=4.28 → might be too insoluble
- z002011: clogp=4.32 → might be too insoluble
- z002015: clogp=4.14 → might be too insoluble
- z002216: TPSA=98.0 → poor BBB
- z002683: TPSA=100.7 → poor BBB
- z002730: clogp=0.09 → probably too soluble
- z002764: HBD=2, TPSA=66.8 → moderate
- z002891: clogp=4.72 → too lipophilic
- z015334: clogp=0.74 → probably too soluble

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|-----------|
| z001999 | -1.522 | 1 | NO (logS > -2, too soluble) |
| z002003 | -3.6 | 1 | NO (logS < -3, too insoluble) |
| z002040 | -2.3374 | 1 | YES ✓ |
| z002052 | -2.7894 | 1 | YES ✓ |
| z002170 | -2.92 | 1 | YES ✓ |
| z002746 | -0.6286 | 1 | NO (logS > -2, too soluble) |
| z003001 | -0.137 | 1 | NO (logS > -2, too soluble) |
| z000364 | -0.9482 | 0 | NO (bbb=0) |

## Conclusion

3 compounds qualify: z002040, z002052, z002170
- All have logS in [-3, -2] and bbb=1
- Budget used: 8/8 assays

Untested compounds unlikely to qualify:
- High clogp (>4): z000889, z002010, z002011, z002015, z002891 → likely logS < -3
- Low clogp (<1): z001510, z001543, z002730, z015334 → likely logS > -2
- High TPSA (>90): z002216, z002683 → likely bbb=0
- z002764: clogp=3.56 similar to z002003 (logS=-3.6), likely just outside range
