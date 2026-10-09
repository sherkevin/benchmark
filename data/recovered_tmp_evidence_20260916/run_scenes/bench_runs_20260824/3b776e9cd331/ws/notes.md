# Compound Triage Notes

## Task Requirements
- Find compounds with measured logS between -3 and -2 (inclusive) AND measured bbb = 1
- Budget: 8 assays total
- Need to return qualifying mol_ids

## Candidate Data Overview
- 20 compounds in data/candidates.tsv
- Compounds have mol_id, smiles, and descriptors (mw, clogp, tpsa, hbd, hba, rotb, rings)
- No solubility or brain penetration data initially

## Assay Results
- Testing started on 2026-08-24
- z000364: logS=-0.9482, bbb=0 (FAIL - logS too high, bbb=0)
- z001510: logS=-2.7372, bbb=1 (PASS - meets both criteria!) 
- z002003: logS=-3.6, bbb=1 (FAIL - logS too low < -3)
- z002010: logS=-5.0097, bbb=1 (FAIL - logS too low < -3)
- z002011: logS=-4.13, bbb=1 (FAIL - logS too low < -3)
- z002015: logS=-3.82, bbb=1 (FAIL - logS too low < -3)
- z001543: logS=-0.8547, bbb=1 (FAIL - logS too high > -2)
- z002040: logS=-2.3374, bbb=1 (PASS - meets both criteria!)

Qualifying compounds: [z001510, z002040]