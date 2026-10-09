# Notes — ADMET triage

## Task
- 20 compounds in `data/candidates.tsv` (mol_id, smiles, mw, clogp, tpsa, hbd, hba, rotb, rings).
- Catalogue does NOT include solubility or BBB.
- `measure_admet(mol_id)` returns measured `logS` (log10 mol/L) and `bbb` (0/1).
- **Budget: 8 assays total.**
- Target profile (must satisfy BOTH):
  1. measured logS in [-3, -2] inclusive
  2. measured bbb = 1
- Deliverables: `notes.md` and `answer.json` (qualifying_ids).

## Strategy
- Use descriptors as a pre-filter to spend assays wisely.
- For BBB penetration, typical heuristics: clogP ~1-4, TPSA < ~70-90, HBD low, MW < ~450-500.
- For solubility logS in [-3,-2]: moderate lipophilicity. logS roughly anti-correlates with clogP. Very low clogP (negative) -> high solubility (logS > -2). Very high clogP -> low solubility (logS < -3). We want middle ground.
- So ideal candidate: clogP moderate (~1-4), TPSA < ~90, HBD low, and not too large/small.
- Avoid compounds with very negative clogP (too soluble, logS likely > -2) and very high clogP (too insoluble, logS likely < -3).
- Avoid high TPSA / high HBD (poor BBB).

## Descriptor table (sorted)
See candidates.tsv. Key columns: mw, clogp, tpsa, hbd.

| mol_id  | mw    | clogp | tpsa  | hbd | hba | rings | notes |
|---------|-------|-------|-------|-----|-----|-------|-------|
| z000364 | 399.4 | 2.87  | 83.1  | 1   | 6   | 3     | |
| z000889 | 430.7 | 8.84  | 29.5  | 1   | 2   | 2     | clogp very high -> likely insoluble |
| z001510 | 180.2 | -1.04 | 72.7  | 1   | 3   | 2     | low mw, low clogp -> likely too soluble |
| z001543 | 286.3 | -1.64 | 119.6 | 5   | 7   | 2     | high tpsa, high hbd -> poor BBB |
| z001999 | 299.4 | 1.5   | 41.9  | 1   | 4   | 5     | good BBB profile |
| z002003 | 308.8 | 3.58  | 43.1  | 0   | 3   | 4     | good BBB profile |
| z002010 | 339.5 | 4.28  | 29.5  | 0   | 3   | 2     | high-ish clogp |
| z002011 | 325.8 | 4.32  | 30.2  | 0   | 2   | 4     | high-ish clogp |
| z002015 | 336.5 | 4.14  | 23.6  | 0   | 2   | 3     | good BBB, maybe insoluble? |
| z002040 | 217.3 | 1.77  | 46.2  | 1   | 2   | 2     | good BBB |
| z002052 | 369.4 | 1.99  | 65.1  | 0   | 6   | 5     | ok |
| z002170 | 250.3 | 3.0   | 34.9  | 0   | 2   | 3     | good BBB |
| z002216 | 250.3 | 0.86  | 98.0  | 2   | 5   | 2     | high tpsa -> poor BBB |
| z002683 | 273.2 | 2.55  | 100.7 | 2   | 5   | 2     | high tpsa -> poor BBB |
| z002730 | 258.2 | 0.09  | 83.6  | 1   | 4   | 3     | low clogp -> maybe too soluble |
| z002746 | 231.3 | 1.55  | 30.2  | 0   | 2   | 2     | good BBB |
| z002764 | 318.3 | 3.56  | 66.8  | 2   | 4   | 2     | borderline tpsa |
| z002891 | 314.5 | 4.72  | 34.1  | 0   | 2   | 4     | steroid, high clogp |
| z003001 | 237.7 | 2.9   | 29.1  | 1   | 2   | 2     | good BBB |
| z015334 | 306.3 | 0.74  | 81.6  | 1   | 5   | 3     | borderline tpsa |

## Triage rationale
BBB-favorable (TPSA < ~90, HBD low, clogP 1-4, MW reasonable) AND plausible logS in [-3,-2] window:
- z001999: clogp 1.5, tpsa 41.9, hbd 1 -> strong BBB candidate; logS likely moderate. PICK.
- z002003: clogp 3.58, tpsa 43.1, hbd 0 -> strong BBB; logS maybe lower. PICK.
- z002040: clogp 1.77, tpsa 46.2, hbd 1 -> good BBB. PICK.
- z002170: clogp 3.0, tpsa 34.9, hbd 0 -> good BBB, logS ~middle. PICK.
- z002746: clogp 1.55, tpsa 30.2, hbd 0 -> good BBB. PICK.
- z003001: clogp 2.9, tpsa 29.1, hbd 1 -> good BBB. PICK.
- z000364: clogp 2.87, tpsa 83.1, hbd 1 -> BBB borderline (tpsa high). Maybe.
- z002052: clogp 1.99, tpsa 65.1, hbd 0 -> ok BBB. PICK (2nd tier).
- z002010: clogp 4.28, tpsa 29.5 -> good BBB but clogp a bit high (solubility risk low end). 
- z002011: clogp 4.32, tpsa 30.2 -> similar.
- z002015: clogp 4.14, tpsa 23.6 -> good BBB, clogp high (insolubility risk).
- z002764: tpsa 66.8, hbd 2, clogp 3.56 -> borderline.
- z002891: steroid clogp 4.72 -> likely too insoluble.

## Assay plan (8 calls)
Prioritize the most likely qualifiers first, then borderline BBB candidates.

Tier 1 (best BBB + plausible solubility):
1. z001999
2. z002003
3. z002040
4. z002170
5. z002746
6. z003001
Tier 2:
7. z002052
8. z002010  (or z002011/z002015)

Will adjust if early assays reveal patterns (e.g., if a cluster confirms, spend remaining budget on similar).

## Assay results — batch 1 (4 assays)

| mol_id  | clogp | tpsa | hbd | logS     | bbb | verdict |
|---------|-------|------|-----|----------|-----|---------|
| z001999 | 1.5   | 41.9 | 1   | -1.522   | 1   | FAIL (logS > -2, too soluble) |
| z002003 | 3.58  | 43.1 | 0   | -3.6     | 1   | FAIL (logS < -3, too insoluble) |
| z002040 | 1.77  | 46.2 | 1   | -2.3374  | 1   | QUALIFIES |
| z002170 | 3.0   | 34.9 | 0   | -2.92    | 1   | QUALIFIES |

### Pattern observed (clogp -> logS)
- clogp 1.5  -> -1.522 (too soluble)
- clogp 1.77 -> -2.3374 ✓
- clogp 3.0  -> -2.92 ✓
- clogp 3.58 -> -3.6 (too insoluble)
Qualifying clogp window ~ [1.77, 3.0]. All 4 had bbb=1 (BBB-favorable descriptors held up).

## Remaining budget: 4 assays — batch 2 selection
Untested candidates whose clogp falls in the [1.77, 3.0] window (best solubility odds):
- z003001: clogp 2.9, tpsa 29.1, hbd 1 — strong (like z002170)
- z002052: clogp 1.99, tpsa 65.1, hbd 0 — good
- z000364: clogp 2.87, tpsa 83.1, hbd 1 — clogp good, BBB risk (high tpsa)
Just above window but higher polarity may raise solubility into range:
- z002764: clogp 3.56, tpsa 66.8, hbd 2 — borderline (z002003 clogp 3.58 was -3.6, but extra tpsa/hbd may push logS up)

These 4 use the remaining budget exactly.

## Assay results — batch 2 (4 assays) — BUDGET EXHAUSTED (8/8)

| mol_id  | clogp | tpsa | hbd | logS     | bbb | verdict |
|---------|-------|------|-----|----------|-----|---------|
| z003001 | 2.9   | 29.1 | 1   | -0.137   | 1   | FAIL (logS > -2, far too soluble — surprising vs clogp trend; HBA/amide raises solubility) |
| z002052 | 1.99  | 65.1 | 0   | -2.7894  | 1   | QUALIFIES |
| z000364 | 2.87  | 83.1 | 1   | -0.9482  | 0   | FAIL (too soluble AND bbb=0; many OMe/HBA groups raise solubility) |
| z002764 | 3.56  | 66.8 | 2   | -2.9008  | 0   | FAIL (logS in range but bbb=0 — high tpsa/hbd blocked BBB as predicted) |

## Summary
All 8 assays used. Confirmed qualifiers (real measure_admet calls, both criteria met):

| mol_id  | logS     | bbb |
|---------|----------|-----|
| z002040 | -2.3374  | 1   |
| z002170 | -2.92    | 1   |
| z002052 | -2.7894  | 1   |

### Lessons
- BBB descriptor heuristic (low tpsa, low hbd) held: z000364 (tpsa 83) and z002764 (tpsa 66.8, hbd 2) both got bbb=0. z002052 (tpsa 65.1 but hbd 0) got bbb=1.
- clogp alone is an imperfect logS proxy: z003001 (clogp 2.9) and z000364 (clogp 2.87) were both much more soluble than clogp suggested, due to extra H-bond acceptors / polar groups. z002052 (clogp 1.99, 6 HBA, high ring count) landed squarely in range.
- The 3 qualifiers all had hbd <= 1 and tpsa in a BBB-friendly zone, with logS confirmed in [-3,-2].
