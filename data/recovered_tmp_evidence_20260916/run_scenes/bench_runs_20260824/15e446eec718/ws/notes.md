# Triage Notes

## Target profile
A compound qualifies ONLY if BOTH:
1. measured `logS` between -3 and -2 inclusive
2. measured `bbb` = 1

Budget: 8 assays total. Each `measure_admet(mol_id)` returns logS and bbb.

## Strategy
The catalogue has no solubility/BBB data. I triaged the 20 candidates on
lipophilicity/polarity heuristics (MW, clogp, TPSA, HBD) — BBB favours small,
moderately lipophilic (clogp ~2-4), low-TPSA (<70), low-HBD (<=2) compounds;
logS in [-3,-2] favours neutral, moderately lipophilic (clogp ~2.5-3.5)
compounds of moderate MW. The two criteria converge on the same chemical
space: small-to-moderate, neutral, moderately lipophilic.

## Assays performed (8/8 budget used)

| # | mol_id | clogp | TPSA | HBD | MW   | logS     | bbb | Qualifies? |
|---|--------|-------|------|-----|------|----------|-----|------------|
| 1 | z002003| 3.58  | 43.1 | 0   | 308.8| -3.6     | 1   | NO (logS < -3) |
| 2 | z003001| 2.9   | 29.1 | 1   | 237.7| -0.137   | 1   | NO (logS > -2; basic amine ionizes) |
| 3 | z002170| 3.0   | 34.9 | 0   | 250.3| -2.92    | 1   | **YES** |
| 4 | z002052| 1.99  | 65.1 | 0   | 369.4| -2.7894  | 1   | **YES** (heroin, known BBB drug) |
| 5 | z001999| 1.5   | 41.9 | 1   | 299.4| -1.522   | 1   | NO (logS > -2; morphine too soluble) |
| 6 | z002010| 4.28  | 29.5 | 0   | 339.5| -5.0097  | 1   | NO (logS << -3; ester too lipophilic) |
| 7 | z002764| 3.56  | 66.8 | 2   | 318.3| -2.9008  | 0   | NO (logS in range but bbb=0 — 2 phenols/HBD2 blocked BBB) |
| 8 | z000364| 2.87  | 83.1 | 1   | 399.4| -0.9482  | 0   | NO (logS > -2 and bbb=0 — high TPSA blocked BBB) |

## Qualifying compounds (confirmed by real assays)

1. **z002170** — logS = -2.92 (in [-3,-2]), bbb = 1. QUALIFIES.
2. **z002052** — logS = -2.7894 (in [-3,-2]), bbb = 1. QUALIFIES.

These two (z002170, a tosyl-methyl quinazolinone; z002052, diacetylmorphine/heroin)
are the only compounds among the 8 assayed that satisfy BOTH criteria.

## Candidates NOT assayed (budget exhausted)
Remaining 12 candidates (z000889, z001510, z001543, z002011, z002015, z002040,
z002216, z002683, z002730, z002746, z002891, z015334) were triaged out by
heuristic screening as lower-priority than the 8 assayed; no budget remained.
None can be reported as qualifying without a confirming assay, so they are
excluded from the answer by the task's requirement that every reported
compound be confirmed by a real `measure_admet` call.

## Key observations
- Neutral moderately-lipophilic compounds (clogp ~2-3.5, low TPSA, HBD 0-1)
  were the sweet spot for BOTH criteria.
- Ionizable/basic amines (z003001, tertiary amine) inflated solubility well
  above -2 → logS fails. Esters/amides on basic scaffolds (z002010) stayed
  far below -3.
- TPSA/HBD drove BBB: z002764 had logS in range but bbb=0 (2 phenol HBDs,
  TPSA 66.8). z000364 bbb=0 (TPSA 83.1) and was also too soluble.
- z002052 (heroin) and z001999 (morphine) bracketed the range: heroin's
  acetyl esterification lowered logS from morphine's -1.52 to -2.79, crossing
  into the qualifying window while retaining bbb=1.

## Final answer
qualifying_ids = ["z002052", "z002170"]
