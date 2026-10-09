# Notes

## Task
- 20 compounds in data/candidates.tsv
- Budget: 8 assays (measure_admet calls)
- Qualifying criteria: logS between -3 and -2 (inclusive) AND bbb = 1

## Strategy
Prioritized compounds with:
- **BBB potential**: TPSA < 90 (ideally <70), HBD ≤ 1, MW < 450, moderate cLogP (1-4)
- **logS in [-3,-2]**: moderate lipophilicity and MW

## Pre-assay Analysis
Selected 8 candidates based on descriptor-based heuristics:
- Compounds with best BBB profiles (low TPSA, low HBD): z002170, z003001, z002003, z002052, z001999, z002746, z002040, z002011
- Excluded compounds with clearly poor profiles: z000889 (cLogP=8.84), z001510 (cLogP=-1.04), z001543 (TPSA=119.6, HBD=5), z002216 (TPSA=98), z002683 (TPSA=100.7), z002730 (cLogP=0.09)

## Assay Results (8/8 used)

### Batch 1 (4 assays):
| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002170 | -2.92 | 1 | ✓ YES (logS in [-3,-2], bbb=1) |
| z003001 | -0.137 | 1 | ✗ (logS too high/soluble) |
| z002003 | -3.6 | 1 | ✗ (logS too low/insoluble) |
| z002052 | -2.7894 | 1 | ✓ YES (logS in [-3,-2], bbb=1) |

### Batch 2 (4 assays):
| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | ✗ (logS too high/soluble) |
| z000364 | -0.9482 | 0 | ✗ (logS too high AND bbb=0) |
| z002764 | -2.9008 | 0 | ✗ (logS in range but bbb=0) |
| z002011 | -4.13 | 1 | ✗ (logS too low/insoluble) |

## Key Observations
- logS was harder to predict from cLogP alone than expected (z003001 with cLogP=2.9 had logS=-0.137, way more soluble than predicted)
- BBB was somewhat predictable: all compounds with TPSA < 65 and HBD ≤ 1 got bbb=1, while z000364 (TPSA=83.1) and z002764 (TPSA=66.8, HBD=2) got bbb=0
- z002764 had ideal logS (-2.9) but failed BBB — unfortunate combination
- Only 2 out of 8 tested compounds qualified

## Qualifying Compounds
1. **z002170** — logS=-2.92, bbb=1 (cLogP=3.0, MW=250.3, TPSA=34.9, HBD=0)
2. **z002052** — logS=-2.7894, bbb=1 (cLogP=1.99, MW=369.4, TPSA=65.1, HBD=0)
