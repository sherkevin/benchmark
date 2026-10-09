# Notes - Compound Triage

## Task
- 20 compounds in data/candidates.tsv
- Need to find compounds with: logS between -3 and -2 (inclusive) AND bbb = 1
- Budget: 8 assays maximum

## Strategy
I'll prioritize compounds based on descriptors that might correlate with brain penetration and solubility:
- BBB penetration often correlates with: moderate TPSA (<90), moderate clogp (1-5), low HBD
- Solubility often correlates with: lower clogp, moderate TPSA

Let me select compounds with properties suggesting good BBB penetration first:

Candidates with TPSA < 90 and reasonable clogp:
- z000364: TPSA=83.1, clogp=2.87
- z000889: TPSA=29.5, clogp=8.84 (very high clogp, may have poor solubility)
- z001510: TPSA=72.7, clogp=-1.04 (low clogp, good solubility candidate)
- z001999: TPSA=41.9, clogp=1.5
- z002003: TPSA=43.1, clogp=3.58
- z002010: TPSA=29.5, clogp=4.28
- z002011: TPSA=30.2, clogp=4.32
- z002015: TPSA=23.6, clogp=4.14
- z002040: TPSA=46.2, clogp=1.77
- z002052: TPSA=65.1, clogp=1.99
- z002170: TPSA=34.9, clogp=3.0
- z002730: TPSA=83.6, clogp=0.09
- z002746: TPSA=30.2, clogp=1.55
- z002764: TPSA=66.8, clogp=3.56
- z002891: TPSA=34.1, clogp=4.72
- z003001: TPSA=29.1, clogp=2.9
- z015334: TPSA=81.6, clogp=0.74

High TPSA compounds (likely poor BBB):
- z001543: TPSA=119.6 (likely poor BBB)
- z002216: TPSA=98.0 (likely poor BBB)
- z002683: TPSA=100.7 (likely poor BBB)

I'll start by testing compounds with moderate TPSA and clogp values that suggest both reasonable solubility and BBB penetration.

## Assays Run

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z000364 | -0.9482 | 0 | No (logS too high, bbb=0) |
| z001999 | -1.522 | 1 | No (logS > -2) |
| z002003 | -3.6 | 1 | No (logS < -3) |
| z002011 | -4.13 | 1 | No (logS < -3) |
| z002015 | -3.82 | 1 | No (logS < -3) |
| z002040 | -2.3374 | 1 | **YES** (logS between -3 and -2, bbb=1) |
| z002170 | -2.92 | 1 | **YES** (logS between -3 and -2, bbb=1) |
| z003001 | -0.137 | 1 | No (logS > -2) |

## Conclusion

Two compounds qualify based on the target profile (logS between -3 and -2 inclusive, AND bbb=1):
- z002040: logS = -2.3374, bbb = 1
- z002170: logS = -2.92, bbb = 1

Budget used: 8/8 assays
