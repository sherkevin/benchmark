# Medicinal Chemistry Triage Notes

## Task
Identify compounds from a 20-compound collection that have:
- measured logS between -3 and -2 (inclusive)
- measured bbb = 1

Budget: 8 assays total.

## Strategy

### Initial Analysis
Reviewed descriptors for all 20 compounds in `data/candidates.tsv`. Key criteria for prioritization:
- **For logS in [-3, -2]**: Need moderate clogP and MW (not too lipophilic, not too hydrophilic)
- **For BBB = 1**: Need low TPSA (<70), low HBD (0-1), moderate clogP

### Batch 1: High clogP candidates (clogP 3.5-4.3)
Tested compounds with good BBB profiles but higher lipophilicity:

| mol_id  | clogP | MW    | TPSA | logS (measured) | bbb | Qualifies? |
|---------|-------|-------|------|-----------------|-----|------------|
| z002011 | 4.32  | 325.8 | 30.2 | -4.13           | 1   | No (logS too low) |
| z002003 | 3.58  | 308.8 | 43.1 | -3.60           | 1   | No (logS too low) |
| z002015 | 4.14  | 336.5 | 23.6 | -3.82           | 1   | No (logS too low) |
| z002010 | 4.28  | 339.5 | 29.5 | -5.01           | 1   | No (logS too low) |

**Learning**: All had bbb=1 but were too insoluble. Need lower clogP for the target logS range.

### Batch 2: Lower clogP candidates (clogP 1.5-3.0)
Shifted to compounds with lower lipophilicity:

| mol_id  | clogP | MW    | TPSA | logS (measured) | bbb | Qualifies? |
|---------|-------|-------|------|-----------------|-----|------------|
| z002170 | 3.00  | 250.3 | 34.9 | -2.92           | 1   | **YES** ✓ |
| z003001 | 2.90  | 237.7 | 29.1 | -0.137          | 1   | No (logS too high) |
| z002040 | 1.77  | 217.3 | 46.2 | -2.3374         | 1   | **YES** ✓ |
| z002746 | 1.55  | 231.3 | 30.2 | -0.6286         | 1   | No (logS too high) |

## Qualifying Compounds
1. **z002170** — logS = -2.92, bbb = 1
2. **z002040** — logS = -2.3374, bbb = 1

## Untested Compounds - Rationale for Exclusion
- **z000889** (clogP 8.84): Extremely lipophilic, certainly logS << -3
- **z001510** (clogP -1.04): Very hydrophilic, certainly logS >> -2
- **z001543** (clogP -1.64, TPSA 119.6): Sugar-like, very soluble, poor BBB
- **z002216** (clogP 0.86, TPSA 98.0): Too soluble, poor BBB
- **z002683** (clogP 2.55, TPSA 100.7): Poor BBB due to high TPSA
- **z002730** (clogP 0.09): Very soluble
- **z002891** (clogP 4.72): Too insoluble based on batch 1 pattern
- **z015334** (clogP 0.74, TPSA 81.6): Too soluble, questionable BBB
- **z000364** (clogP 2.87, TPSA 83.1): BBB questionable, MW 399 might push logS too low
- **z001999** (clogP 1.5, MW 299): Might qualify but uncertain; budget exhausted
- **z002052** (clogP 1.99, MW 369): Higher MW might push logS too low
- **z002764** (clogP 3.56, MW 318): Likely too insoluble based on batch 1 pattern
