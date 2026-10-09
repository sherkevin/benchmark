# Notes

## Strategy
- Target: logS between -3 and -2 (inclusive), bbb = 1
- Budget: 8 assays out of 20 compounds
- BBB favors: low TPSA (<70), low HBD (0-1), moderate clogp (2-5), MW < 450
- logS in [-3,-2] favors: moderate clogp (~2-4.5). Too high → insoluble (logS < -3). Too low → too soluble (logS > -2).

## Compound Analysis & Selection

### Eliminated (unlikely to qualify):
- z000889: clogp 8.84 → far too lipophilic, logS way below -3
- z001510: clogp -1.04, TPSA 72.7 → too hydrophilic, logS > -2
- z001543: clogp -1.64, TPSA 119.6 → too hydrophilic, no BBB
- z002216: clogp 0.86, TPSA 98.0 → too high TPSA for BBB, likely too soluble
- z002683: TPSA 100.7 → too high for BBB
- z002730: clogp 0.09 → likely too soluble
- z015334: clogp 0.74 → likely too soluble
- z000364: TPSA 83.1 → borderline for BBB
- z002040: clogp 1.77, TPSA 46.2 → might be too soluble (logS > -2)
- z002891: clogp 4.72 → might be too insoluble (logS < -3)

### Selected for assay (8 picks):
1. z002003: clogp 3.58, TPSA 43.1, HBD 0 → strong candidate
2. z002170: clogp 3.0, TPSA 34.9, HBD 0 → strong candidate
3. z003001: clogp 2.9, TPSA 29.1, HBD 1 → strong candidate
4. z002011: clogp 4.32, TPSA 30.2, HBD 0 → good BBB, logS borderline
5. z002010: clogp 4.28, TPSA 29.5, HBD 0 → good BBB, logS borderline
6. z002015: clogp 4.14, TPSA 23.6, HBD 0 → good BBB, logS borderline
7. z002746: clogp 1.55, TPSA 30.2, HBD 0 → good BBB, solubility uncertain
8. z002764: clogp 3.56, TPSA 66.8, HBD 2 → moderate candidate

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002003 | -3.6 | 1 | No (logS < -3) |
| z002170 | -2.92 | 1 | **YES** |
| z003001 | -0.137 | 1 | No (logS > -2) |
| z002011 | -4.13 | 1 | No (logS < -3) |
| z002010 | -5.01 | 1 | No (logS < -3) |
| z002015 | -3.82 | 1 | No (logS < -3) |
| z002746 | -0.629 | 1 | No (logS > -2) |
| z002764 | -2.90 | 0 | No (bbb = 0) |

## Conclusion
Only **z002170** qualifies: logS = -2.92 (in [-3, -2]) and bbb = 1.
Budget exhausted (8/8 assays used).
