# Compound Triage Notes

## Task
Identify compounds with:
- logS between -3 and -2 (inclusive)
- bbb = 1

Budget: 8 assays

## Strategy
Looking at the descriptors, compounds that might have good brain penetration (bbb=1) typically have:
- Lower TPSA (ideally < 70-90)
- Moderate clogP (not too hydrophilic or lipophilic)
- Lower HBD count

Compounds with very high TPSA (>90) or high HBD are less likely to cross BBB.

## Candidates Analysis

| mol_id | clogp | tpsa | hbd | hba | Notes |
|--------|-------|------|-----|-----|-------|
| z000364 | 2.87 | 83.1 | 1 | 6 | Moderate TPSA |
| z000889 | 8.84 | 29.5 | 1 | 2 | Very high clogP, low TPSA - might be too lipophilic |
| z001510 | -1.04 | 72.7 | 1 | 3 | Low clogP, moderate TPSA |
| z001543 | -1.64 | 119.6 | 5 | 7 | High TPSA, many HBD - unlikely BBB |
| z001999 | 1.5 | 41.9 | 1 | 4 | Good profile for BBB |
| z002003 | 3.58 | 43.1 | 0 | 3 | Good profile for BBB |
| z002010 | 4.28 | 29.5 | 0 | 3 | Good profile for BBB |
| z002011 | 4.32 | 30.2 | 0 | 2 | Good profile for BBB |
| z002015 | 4.14 | 23.6 | 0 | 2 | Good profile for BBB |
| z002040 | 1.77 | 46.2 | 1 | 2 | Good profile for BBB |
| z002052 | 1.99 | 65.1 | 0 | 6 | Moderate TPSA |
| z002170 | 3.0 | 34.9 | 0 | 2 | Good profile for BBB |
| z002216 | 0.86 | 98.0 | 2 | 5 | High TPSA - unlikely BBB |
| z002683 | 2.55 | 100.7 | 2 | 5 | High TPSA - unlikely BBB |
| z002730 | 0.09 | 83.6 | 1 | 4 | Moderate TPSA |
| z002746 | 1.55 | 30.2 | 0 | 2 | Good profile for BBB |
| z002764 | 3.56 | 66.8 | 2 | 4 | Moderate TPSA |
| z002891 | 4.72 | 34.1 | 0 | 2 | Good profile for BBB |
| z003001 | 2.9 | 29.1 | 1 | 2 | Good profile for BBB |
| z015334 | 0.74 | 81.6 | 1 | 5 | Moderate TPSA |

## Selection for Assays
Focusing on compounds with TPSA < 70, HBD <= 1, and moderate clogP for BBB penetration:

Priority candidates (low TPSA, good clogP):
1. z001999 - tpsa 41.9, clogp 1.5
2. z002003 - tpsa 43.1, clogp 3.58
3. z002010 - tpsa 29.5, clogp 4.28
4. z002011 - tpsa 30.2, clogp 4.32
5. z002015 - tpsa 23.6, clogp 4.14
6. z002040 - tpsa 46.2, clogp 1.77
7. z002170 - tpsa 34.9, clogp 3.0
8. z002746 - tpsa 30.2, clogp 1.55
9. z002891 - tpsa 34.1, clogp 4.72
10. z003001 - tpsa 29.1, clogp 2.9

## Assay Results (8/8 budget used)

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002010 | -5.0097 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002040 | -2.3374 | 1 | **YES** |
| z002170 | -2.92 | 1 | **YES** |
| z002746 | -0.6286 | 1 | NO (logS > -2) |

## Qualifying Compounds
- **z002040**: logS = -2.3374, bbb = 1
- **z002170**: logS = -2.92, bbb = 1
