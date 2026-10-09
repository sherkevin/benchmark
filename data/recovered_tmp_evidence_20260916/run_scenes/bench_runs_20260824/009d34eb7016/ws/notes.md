# Triage Notes — Compound Selection for ADMET Assays

## Target profile (BOTH must hold)
- measured logS in [-3, -2] (moderate aqueous solubility)
- measured bbb = 1 (brain penetration)

## Budget
- 8 assays total, 20 candidates → must pre-filter with cheap descriptors.

## Descriptor-based heuristics
- **BBB (bbb=1):** Favor clogp ~1.5–4.5, TPSA <90 (ideally <70), HBD ≤2, MW <400.
- **logS in [-3,-2]:** Avoid very high lipophilicity (clogp >5 → too insoluble, logS<-3)
  and very low lipophilicity / very polar (clogp <0 or TPSA >100 → too soluble, logS>-2).

## Candidate scan (mw, clogp, tpsa, hbd, hba, rotb, rings)

| mol_id | mw | clogp | tpsa | hbd | rings | notes |
|--------|------|-------|------|-----|-------|-------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 3 | borderline TPSA, mw high — possible |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | clogp 8.84 → very insoluble (logS<-3), MW>400 — exclude |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 2 | clogp -1.04 → too soluble (logS>-2) — exclude |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 2 | very polar, HBD 5, TPSA 120 → too soluble, no BBB — exclude |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 5 | good BBB profile, moderate clogp — **test** |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 4 | good BBB profile, clogp 3.58 may push logS~-3 — **test** |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 2 | ester, clogp 4.28, low TPSA — borderline logS — **test** |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 4 | similar to z002003, low TPSA — borderline logS — **test** |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 3 | very low TPSA, clogp 4.14 — **test** (likely BBB) |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | small, moderate clogp — possible, logS maybe >-2 — **test** |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 5 | acetate ester of z001999; good BBB profile — **test** |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 3 | good BBB, clogp 3.0 → logS likely -3 to -2 — **test** |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 2 | TPSA 98, sulfonamide — BBB doubtful, logS>-2 — exclude |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 2 | TPSA 100 + nitro → low BBB, logS>-2 — exclude |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 3 | two imides, clogp 0.09 → logS>-2 — exclude |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | low TPSA, clogp 1.55 — logS maybe >-2 — borderline |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | clogp 3.56, TPSA 67 — borderline logS — **test** |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 4 | steroid, clogp 4.72, low TPSA — logS maybe -3 — **test** |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | low TPSA, clogp 2.9 — good — **test** |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 3 | TPSA 81 borderline, clogp 0.74 → logS>-2 — exclude |

## Selected for assay (8)
Priority order (best guess for BOTH criteria):
1. z002170 (clogp 3.0, TPSA 35, HBD 0) — strong logS/BBB candidate
2. z002003 (clogp 3.58, TPSA 43, HBD 0)
3. z003001 (clogp 2.9, TPSA 29, HBD 1)
4. z002011 (clogp 4.32, TPSA 30, HBD 0)
5. z002015 (clogp 4.14, TPSA 24, HBD 0)
6. z002764 (clogp 3.56, TPSA 67, HBD 2)
7. z002891 (clogp 4.72, TPSA 34, HBD 0)
8. z001999 (clogp 1.5, TPSA 42, HBD 1)

## Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002170 | -2.92 | 1 | **YES** (-3 <= -2.92 <= -2, bbb=1) |
| z002003 | -3.6 | 1 | No (logS < -3) |
| z003001 | -0.137 | 1 | No (logS > -2) |
| z002011 | -4.13 | 1 | No (logS < -3) |
| z002015 | -3.82 | 1 | No (logS < -3) |
| z002764 | -2.9008 | 0 | No (bbb=0; logS in range but no BBB) |
| z002891 | -4.5526 | 0 | No (bbb=0) |
| z001999 | -1.522 | 1 | No (logS > -2) |

Budget exhausted (8/8). No remaining assays available to test other candidates.

## Conclusion
Only **z002170** confirmed to satisfy BOTH criteria:
- logS = -2.92 (within [-3, -2])
- bbb = 1

Reported qualifying_ids = ["z002170"].

### Observations on descriptor-vs-measured mismatch
- clogp was a poor predictor of logS here: z003001 (clogp 2.9) measured logS -0.137
  (far more soluble than expected — likely the N-methyl amide + chloroaryl balance),
  and z002003 (clogp 3.58) measured logS -3.6 (less soluble than clogp suggested).
- BBB mostly tracked TPSA/HBD as expected; the two HBD-0 low-TPSA esters
  (z002764 had bbb=0, surprisingly) and the steroid z002891 (bbb=0).
