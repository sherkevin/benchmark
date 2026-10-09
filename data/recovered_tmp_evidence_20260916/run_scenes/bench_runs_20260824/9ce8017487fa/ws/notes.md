# Notes - Compound Triage

## Strategy
Target profile: logS ∈ [-3, -2] AND bbb = 1

For BBB penetration: TPSA < 90, HBD ≤ 3, moderate clogP (2-4)
For moderate solubility: avoid extreme clogP (very high = poor solubility, very low = too soluble)

## Compound Analysis

| mol_id | clogP | TPSA | HBD | BBB potential | Solubility potential |
|--------|-------|------|-----|---------------|---------------------|
| z000364 | 2.87 | 83.1 | 1 | Good | Moderate |
| z000889 | 8.84 | 29.5 | 1 | Good (low TPSA) | Poor (very high clogP) |
| z001510 | -1.04 | 72.7 | 1 | Moderate | Too soluble (low clogP) |
| z001543 | -1.64 | 119.6 | 5 | Poor (high TPSA, HBD) | Too soluble |
| z001999 | 1.5 | 41.9 | 1 | Good | Moderate |
| z002003 | 3.58 | 43.1 | 0 | Good | Moderate |
| z002010 | 4.28 | 29.5 | 0 | Good | Moderate-poor |
| z002011 | 4.32 | 30.2 | 0 | Good | Moderate-poor |
| z002015 | 4.14 | 23.6 | 0 | Good | Moderate-poor |
| z002040 | 1.77 | 46.2 | 1 | Good | Moderate |
| z002052 | 1.99 | 65.1 | 0 | Good | Moderate |
| z002170 | 3.0 | 34.9 | 0 | Good | Moderate |
| z002216 | 0.86 | 98.0 | 2 | Poor (high TPSA) | Too soluble |
| z002683 | 2.55 | 100.7 | 2 | Poor (high TPSA) | Moderate |
| z002730 | 0.09 | 83.6 | 1 | Moderate | Too soluble |
| z002746 | 1.55 | 30.2 | 0 | Good | Moderate |
| z002764 | 3.56 | 66.8 | 2 | Good | Moderate |
| z002891 | 4.72 | 34.1 | 0 | Good | Poor (high clogP) |
| z003001 | 2.9 | 29.1 | 1 | Good | Moderate |
| z015334 | 0.74 | 81.6 | 1 | Moderate | Too soluble |

## Assays to run (budget: 8)
Priority: Good BBB potential + moderate clogP for solubility

Top candidates: z001999, z002003, z002040, z002052, z002170, z002746, z002764, z003001

## Assay Results

| mol_id | logS | bbb | Qualifies (logS ∈ [-3,-2] AND bbb=1)? |
|--------|------|-----|--------------------------------------|
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002040 | -2.3374 | 1 | YES ✓ |
| z002052 | -2.7894 | 1 | YES ✓ |
| z002170 | -2.92 | 1 | YES ✓ |
| z002746 | -0.6286 | 1 | NO (logS > -2) |
| z002764 | -2.9008 | 0 | NO (bbb = 0) |
| z003001 | -0.137 | 1 | NO (logS > -2) |

## Qualifying Compounds
- z002040: logS = -2.3374, bbb = 1
- z002052: logS = -2.7894, bbb = 1
- z002170: logS = -2.92, bbb = 1

Budget used: 8/8 assays
