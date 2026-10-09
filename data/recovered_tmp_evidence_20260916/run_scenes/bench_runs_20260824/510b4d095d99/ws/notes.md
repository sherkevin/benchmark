# Notes

## Task Summary
- 20 compounds in data/candidates.tsv
- Budget: 8 assays (measure_admet calls)
- Target profile: logS between -3 and -2 inclusive, AND bbb = 1

## Strategy
I need to prioritize compounds most likely to meet the target profile:
- logS between -3 and -2: moderate solubility (not too hydrophilic, not too hydrophobic)
- bbb = 1: brain penetration typically requires moderate clogp (~2-4) and tpsa < 90

Looking at the descriptors:
- High clogp compounds (>5) likely too hydrophobic → poor logS
- Low clogp compounds (<0) likely too hydrophilic → may not cross BBB
- High tpsa (>90) compounds less likely to cross BBB

## Candidates to prioritize (based on clogp 1-4, tpsa < 90):
1. z000364: clogp=2.87, tpsa=83.1 - good candidate
2. z001999: clogp=1.5, tpsa=41.9 - good candidate
3. z002003: clogp=3.58, tpsa=43.1 - good candidate
4. z002011: clogp=4.32, tpsa=30.2 - possible
5. z002015: clogp=4.14, tpsa=23.6 - possible
6. z002040: clogp=1.77, tpsa=46.2 - good candidate
7. z002052: clogp=1.99, tpsa=65.1 - good candidate
8. z002170: clogp=3.0, tpsa=34.9 - good candidate
9. z002730: clogp=0.09, tpsa=83.6 - borderline
10. z002746: clogp=1.55, tpsa=30.2 - good candidate
11. z002764: clogp=3.56, tpsa=66.8 - good candidate
12. z003001: clogp=2.9, tpsa=29.1 - good candidate

## Assays run:
