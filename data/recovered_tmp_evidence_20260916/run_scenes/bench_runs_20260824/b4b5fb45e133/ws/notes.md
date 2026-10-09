# Triage notes

## Task
- 20 compounds in `data/candidates.tsv`. Budget = 8 `measure_admet` assays total.
- Each assay returns measured `logS` (log10 mol/L) and `bbb` (0/1).
- Qualify iff: logS in [-3, -2] inclusive AND bbb = 1.
- Deliverable: `answer.json` lists every confirmed qualifier (only those actually assayed).

## Selection strategy
Cannot assay all 20, so pick the 8 with highest P(both criteria).

### BBB (bbb=1) favored by: low TPSA (<~70), low HBD, MW<450, moderate clogp.
### logS in [-3,-2] (moderate solubility): General Solubility Equation logS ≈ 0.5 - clogp
  (for low-MP/amorphous). For rigid/crystalline solids (steroids, fused poly­cyclics,
  imides) MP correction pushes logS ~1-2 units lower.

### Per-compound triage
| mol_id | clogp | tpsa | hbd | mw | notes | verdict |
|--------|-------|------|-----|----|-------|---------|
| z000364 | 2.87 | 83.1 | 1 | 399 | heavily-OMe lignan; logS good, BBB borderline (TPSA/MW) | TEST |
| z000889 | 8.84 | 29.5 | 1 | 431 | clogp huge -> logS <<-3 | skip |
| z001510 | -1.04 | 72.7 | 1 | 180 | caffeine-like, very soluble -> logS>-2 | skip |
| z001543 | -1.64 | 119.6 | 5 | 286 | glycoside, hydrophilic, BBB=0 | skip |
| z001999 | 1.5 | 41.9 | 1 | 299 | morphinan (DXM/dextrorphan-like); rigid->high MP lowers logS; CNS | TEST |
| z002003 | 3.58 | 43.1 | 0 | 309 | GSE logS~-3.08 borderline low; BBB good | TEST |
| z002010 | 4.28 | 29.5 | 0 | 340 | clogp high -> logS<-3.5 | skip |
| z002011 | 4.32 | 30.2 | 0 | 326 | clogp high | skip |
| z002015 | 4.14 | 23.6 | 0 | 337 | clogp high | skip |
| z002040 | 1.77 | 46.2 | 1 | 217 | glutethimide-like (CNS sedative); imide ring high MP lowers logS to ~-3 | TEST |
| z002052 | 1.99 | 65.1 | 0 | 369 | diacetylmorphine-like (heroin, MW match); BBB++ ; logS~-2.4 | TEST |
| z002170 | 3.0 | 34.9 | 0 | 250 | GSE logS~-2.5; TPSA low BBB yes | TEST |
| z002216 | 0.86 | 98.0 | 2 | 250 | TPSA/hbd high -> BBB=0 | skip |
| z002683 | 2.55 | 100.7 | 2 | 273 | TPSA high -> BBB=0 | skip |
| z002730 | 0.09 | 83.6 | 1 | 258 | clogp~0 -> logS>-2 too soluble | skip |
| z002746 | 1.55 | 30.2 | 0 | 231 | planar heterocycle; GSE~-1.05 maybe too soluble | borderline |
| z002764 | 3.56 | 66.8 | 2 | 318 | bisphenol-fluorene lactone; logS~-3 borderline; hbd2 | TEST |
| z002891 | 4.72 | 34.1 | 0 | 315 | steroid clogp huge + high MP -> logS<-4 | skip |
| z003001 | 2.9 | 29.1 | 1 | 238 | PCP-like arylcyclohexylamine; CNS, BBB++ ; GSE logS~-2.4 | TEST |
| z015334 | 0.74 | 81.6 | 1 | 306 | clogp low -> logS>-2; TPSA borderline BBB | skip |

### Picked 8 to assay
1. z003001  2. z002052  3. z002170  4. z002040
5. z001999  6. z002003  7. z002764  8. z000364

Will run first 4, calibrate, then run next 4.

## Results

| mol_id | logS | bbb | qual? |
|--------|------|-----|-------|
| z003001 | -0.137 | 1 | NO (logS > -2) |
| z002052 | -2.7894 | 1 | YES |
| z002170 | -2.92 | 1 | YES |
| z002040 | -2.3374 | 1 | YES |
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002764 | -2.9008 | 0 | NO (bbb=0) |
| z000364 | -0.9482 | 0 | NO (bbb=0; logS > -2) |

Assays used: 8/8. Budget exhausted.

## Confirmed qualifiers (3)
- z002040  (logS -2.3374, bbb 1)
- z002052  (logS -2.7894, bbb 1)
- z002170  (logS -2.92,   bbb 1)

Notable: the actual measurement noise/jitter aside, the rest of the catalogue
was either too soluble (low clogp/low MW: z000364, z001999, z003001 — all logS > -2)
or too insoluble (z002003 -3.6), or failed BBB (z002764, z000364 with TPSA ~67/83).
GSE was unreliable across the board; only direct assay gave the truth.

## answer.json
Written with qualifying_ids = ["z002040","z002052","z002170"].

GSE badly overestimated z003001's lipophilicity-driven insolubility (predicted -2.4, got -0.14).
This aminoketone is more soluble than GSE suggests — possibly the basic amine raises apparent
solubility. Lesson: GSE is unreliable here; rely on measured values. Remaining candidates
(z001999, z002003, z002764, z000364) — run all 4; the prediction band still puts them in range.
