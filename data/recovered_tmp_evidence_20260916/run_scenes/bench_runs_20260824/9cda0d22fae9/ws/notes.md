# Triage Notes

## Target profile
A compound qualifies only if:
1. measured `logS` between -3 and -2 inclusive
2. measured `bbb` = 1

Budget: 8 assays total.

## Strategy
The catalogue gives cheap descriptors (mw, clogp, tpsa, hbd, hba, rotb, rings) but NOT solubility or BBB.
I'll use these descriptors to prioritize compounds likely to hit BOTH criteria:

- **logS in [-3, -2]:** moderately soluble. General logS estimate ~0.5-0.8*clogp - constants. A rough guide: clogp around 1.5-3.5 often corresponds to logS in this range. Very high clogp (e.g. >5) -> too insoluble (logS < -3); very low clogp (e.g. <0) -> too soluble (logS > -2).
- **bbb=1:** crosses BBB. Favors: low tpsa (<70-90), low hbd (<3), clogp in ~1.5-3.5 (not too high). Penalizes: high tpsa, high hbd, charged/zwitterionic, very high mw.

## Per-compound assessment

| mol_id | mw | clogp | tpsa | hbd | hba | rotb | rings | notes |
|--------|-----|------|------|-----|-----|------|-------|-------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | 5 | 3 | tpsa high (83), borderline BBB; clogp good for logS |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | 12 | 2 | clogp way too high -> logS far < -3; many rotb |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | 0 | 2 | xanthine-like; clogp very low -> logS too high (>-2) |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | 4 | 2 | tpsa very high + 5 hbd -> no BBB; logS too high |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | 1 | 5 | tpsa low, clogp ~1.5 -> candidate for both! |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | 1 | 4 | tpsa low, hbd 0, clogp 3.58 -> candidate (logS maybe ~-2.5) |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | 8 | 2 | tpsa low but clogp 4.28 -> logS likely < -3 borderline; 8 rotb |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | 1 | 4 | tpsa low, clogp 4.32 -> logS maybe too low; borderline |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | 6 | 3 | tpsa low but clogp 4.14 + 6 rotb -> logS borderline low |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | 2 | 2 | tpsa ok, clogp 1.77 -> candidate (logS ~-2 to -2.5) |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | 2 | 5 | tpsa ok, clogp 1.99 -> candidate |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | 1 | 3 | tpsa low, clogp 3.0 -> strong candidate (logS ~-2.5) |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | 3 | 2 | tpsa very high, 2 hbd -> BBB unlikely; logS maybe too high |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | 3 | 2 | tpsa very high, nitro+phenols -> BBB unlikely |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | 1 | 3 | tpsa high, clogp very low -> logS too high; BBB unlikely |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | 2 | 2 | tpsa low, clogp 1.55 -> candidate (logS ~-2) |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | 2 | 4 | tpsa ok, 2 hbd borderline, clogp 3.56 -> candidate |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | 1 | 4 | tpsa low but clogp 4.72 -> logS likely < -3 |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | 2 | 2 | tpsa low, clogp 2.9 -> strong candidate |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | 5 | 3 | tpsa high-ish, clogp 0.74 -> logS too high; BBB borderline |

## Ranked candidates to assay (best BBB + logS-window first)
1. z002170 (clogp 3.0, tpsa 34.9, hbd 0) — excellent BBB profile, logS likely in window
2. z003001 (clogp 2.9, tpsa 29.1, hbd 1) — excellent
3. z002003 (clogp 3.58, tpsa 43.1, hbd 0) — good, logS may be near -3 edge
4. z002040 (clogp 1.77, tpsa 46.2, hbd 1) — good, logS may be near -2 edge
5. z002746 (clogp 1.55, tpsa 30.2, hbd 0) — good, logS near -2 edge
6. z002764 (clogp 3.56, tpsa 66.8, hbd 2) — ok
7. z002052 (clogp 1.99, tpsa 65.1, hbd 0) — ok
8. z001999 (clogp 1.5, tpsa 41.9, hbd 1) — ok, logS near -2 edge

Order chosen to maximize coverage of the logS window (-3 to -2) with BBB-favorable descriptors.

## Assay results (8 assays, budget exhausted)

| mol_id | logS | bbb | qualifies? |
|--------|------|-----|-----------|
| z002170 | -2.92 | 1 | YES (logS in [-3,-2], bbb=1) |
| z003001 | -0.137 | 1 | no — logS > -2 (too soluble) |
| z002003 | -3.6 | 1 | no — logS < -3 (too insoluble) |
| z002040 | -2.34 | 1 | YES |
| z002746 | -0.6286 | 1 | no — logS > -2 |
| z002764 | -2.9008 | 0 | no — bbb=0 |
| z002052 | -2.7894 | 1 | YES |
| z001999 | -1.522 | 1 | no — logS > -2 |

## Confirmed qualifying compounds
- z002170
- z002040
- z002052

These are the only compounds confirmed by a real measure_admet call to satisfy both
logS in [-3,-2] inclusive and bbb=1. Other compounds were not assayed (budget limit)
and therefore are not reported as qualifying.
