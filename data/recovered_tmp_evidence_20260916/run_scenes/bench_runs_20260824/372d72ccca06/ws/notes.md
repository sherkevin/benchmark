# Task Notes — Compound Triage

## Goal
Identify compounds that satisfy BOTH:
1. measured `logS` between -3 and -2 inclusive
2. measured `bbb` = 1

## Budget
8 assays total (20 candidates). Must be selective.

## Strategy
- Use cheap descriptors (mw, clogp, tpsa, hbd, hba) to prioritize candidates likely to satisfy the target profile.
- Target profile: moderate solubility (logS ~ -3 to -2) and BBB penetrant (bbb=1).
- BBB penetration heuristic: tpsa < ~70-90, hbd <= 1-2, clogp ~ 1-4, mw < ~450.
- Solubility heuristic: logS ~ -3 to -2 usually corresponds to moderate clogp (~1-4), not too many HBDs/HBAs.

## Candidate analysis (pre-screen)

Let me reason about each compound's likely solubility and BBB potential:

| mol_id | mw | clogp | tpsa | hbd | hba | rings | notes |
|--------|-----|-------|------|-----|-----|-------|-------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | 3 | tpsa high, BBB unlikely; clogp ok -> maybe sol? |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | 2 | very high clogp -> insoluble; BBB ok? tpsa low |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | 2 | very polar -> too soluble (logS > -2 likely) |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | 2 | very polar, many HBD -> too soluble, no BBB |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | 5 | tpsa low, clogp ok -> good BBB + sol candidate |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | 4 | tpsa low, no HBD, clogp ok -> good candidate |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | 2 | tpsa low, no HBD, clogp high-ish -> candidate |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | 4 | similar to z002003, clogp slightly high |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | 3 | tpsa very low, no HBD, clogp high -> BBB yes, sol? |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | 2 | balanced -> good candidate |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | 5 | tpsa moderate, clogp ok -> candidate |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | 3 | tpsa low, no HBD, clogp ok -> good candidate |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | 2 | tpsa high, HBD=2 -> BBB unlikely |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | 2 | tpsa high, HBD=2, nitro -> BBB unlikely |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | 3 | tpsa high, clogp low -> maybe too soluble |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | 2 | tpsa low, no HBD, clogp ok -> good candidate |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | 4 | tpsa ok, HBD=2 -> BBB borderline |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | 4 | tpsa low, no HBD, clogp high -> steroid-like, candidate |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | 2 | tpsa low, clogp ok -> good candidate |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | 3 | tpsa high -> BBB borderline |

## Prioritized list for assay (best 8)
Strong BBB + plausible solubility candidates:
1. z001999 — tpsa 41.9, clogp 1.5, hbd 1 (well-balanced)
2. z002003 — tpsa 43.1, clogp 3.58, hbd 0
3. z002011 — tpsa 30.2, clogp 4.32, hbd 0
4. z002015 — tpsa 23.6, clogp 4.14, hbd 0
5. z002040 — tpsa 46.2, clogp 1.77, hbd 1
6. z002170 — tpsa 34.9, clogp 3.0, hbd 0
7. z002746 — tpsa 30.2, clogp 1.55, hbd 0
8. z003001 — tpsa 29.1, clogp 2.9, hbd 1

Also strong but cut: z002010 (clogp 4.28), z002052 (tpsa 65.1), z002891 (clogp 4.72), z002764 (hbd 2).

## Assay results (8 assays, budget exhausted)

| mol_id | logS | bbb | in [-3,-2]? | qualifies? |
|--------|------|-----|-------------|------------|
| z001999 | -1.522 | 1 | no (> -2) | NO |
| z002003 | -3.6 | 1 | no (< -3) | NO |
| z002011 | -4.13 | 1 | no (< -3) | NO |
| z002015 | -3.82 | 1 | no (< -3) | NO |
| z002040 | -2.3374 | 1 | YES | **YES** |
| z002170 | -2.92 | 1 | YES | **YES** |
| z002746 | -0.6286 | 1 | no (> -2) | NO |
| z003001 | -0.137 | 1 | no (> -2) | NO |

## Conclusion
Confirmed qualifying compounds (both conditions met, verified by real assays):
- z002040 (logS=-2.3374, bbb=1)
- z002170 (logS=-2.92, bbb=1)

Budget (8/8) exhausted; no further assays possible. Reported only confirmed qualifiers, as required.
