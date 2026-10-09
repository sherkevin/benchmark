# Notes - Compound Screening Task

## Task Summary
- 20 compounds in data/candidates.tsv
- Budget: 8 ADMET assays
- Qualification criteria: logS between -3 and -2 (inclusive), AND bbb = 1

## Selection Strategy
For BBB penetration (bbb=1): need low TPSA (<90, ideally <70), low HBD (≤1), moderate clogp
For logS between -3 and -2: moderate lipophilicity (clogp ~2-4)

## Compounds prioritized for assay
Selected compounds with tpsa < 70, hbd ≤ 1, and clogp between 1.5-4.5:
1. z002003 (clogp=3.58, tpsa=43.1, hbd=0)
2. z002011 (clogp=4.32, tpsa=30.2, hbd=0)
3. z002170 (clogp=3.0, tpsa=34.9, hbd=0)
4. z003001 (clogp=2.9, tpsa=29.1, hbd=1)
5. z001999 (clogp=1.5, tpsa=41.9, hbd=1)
6. z002746 (clogp=1.55, tpsa=30.2, hbd=0)
7. z002040 (clogp=1.77, tpsa=46.2, hbd=1)
8. z002052 (clogp=1.99, tpsa=65.1, hbd=0)

## Assay Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002170 | -2.92 | 1 | NO (logS < -3) |
| z003001 | -0.137 | 1 | NO (logS > -2) |
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002746 | -0.6286 | 1 | NO (logS > -2) |
| z002040 | -2.3374 | 1 | YES |
| z002052 | -2.7894 | 1 | YES |

## Qualifying Compounds
- z002040: logS = -2.3374, bbb = 1
- z002052: logS = -2.7894, bbb = 1

Budget used: 8/8 assays
