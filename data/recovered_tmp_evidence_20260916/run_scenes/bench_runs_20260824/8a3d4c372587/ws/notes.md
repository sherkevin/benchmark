# Notes

## Task
Find compounds with logS in [-3, -2] AND bbb = 1. Budget: 8 assays out of 20 candidates.

## Analysis of Candidates

Key rules of thumb for prioritization:
- BBB penetration: TPSA < ~60-70, moderate clogp (1-4), low MW, low HBD
- logS in [-3, -2] (moderate solubility): moderate clogp (~1.5-4.0), not too polar, not too lipophilic

### Compounds excluded from testing:
- z000889: clogp=8.84 → far too insoluble (logS << -3)
- z001510: clogp=-1.04, tpsa=72.7 → too soluble, unlikely BBB
- z001543: clogp=-1.64, tpsa=119.6 → far too soluble, no BBB
- z002216: tpsa=98.0 → unlikely BBB
- z002683: tpsa=100.7 → unlikely BBB
- z002730: clogp=0.09, tpsa=83.6 → too soluble, unlikely BBB
- z002764: tpsa=66.8, hbd=2 → borderline BBB
- z000364: tpsa=83.1 → unlikely BBB
- z015334: tpsa=81.6 → unlikely BBB
- z002010: clogp=4.28, tpsa=29.5 → good BBB but might be too insoluble
- z002015: clogp=4.14, tpsa=23.6 → good BBB but might be too insoluble
- z002052: tpsa=65.1 → borderline BBB

### 8 Selected for Assaying:
1. z002746 (clogp=1.55, tpsa=30.2, mw=231.3)
2. z001999 (clogp=1.5, tpsa=41.9, mw=299.4)
3. z002040 (clogp=1.77, tpsa=46.2, mw=217.3)
4. z003001 (clogp=2.9, tpsa=29.1, mw=237.7)
5. z002170 (clogp=3.0, tpsa=34.9, mw=250.3)
6. z002003 (clogp=3.58, tpsa=43.1, mw=308.8)
7. z002011 (clogp=4.32, tpsa=30.2, mw=325.8)
8. z002891 (clogp=4.72, tpsa=34.1, mw=314.5)

## Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|-----------|
| z002746 | -0.63 | 1 | No (logS > -2) |
| z001999 | -1.52 | 1 | No (logS > -2) |
| z002040 | -2.34 | 1 | **YES** ✓ |
| z003001 | -0.14 | 1 | No (logS > -2) |
| z002170 | -2.92 | 1 | **YES** ✓ |
| z002003 | -3.60 | 1 | No (logS < -3) |
| z002011 | -4.13 | 1 | No (logS < -3) |
| z002891 | -4.55 | 0 | No (both fail) |

### Qualifying compounds:
- **z002040**: CCC1(c2ccccc2)CCC(=O)NC1=O — logS=-2.34, bbb=1
- **z002170**: Cc1ccccc1-n1c(C)nc2ccccc2c1=O — logS=-2.92, bbb=1

### Observations:
- Lower clogp compounds (1.5-1.77) had logS values too high (too soluble): z002746, z001999
- Higher clogp compounds (3.58+) had logS values too low (too insoluble): z002003, z002011, z002891
- Sweet spot was clogp ~1.8-3.0: z002040 (1.77) and z002170 (3.0) qualified
- z003001 (clogp=2.9) was surprisingly too soluble (logS=-0.14) — perhaps its structure confers unusual solubility
- All tested compounds except z002891 had bbb=1, suggesting the TPSA cutoff I used was effective
