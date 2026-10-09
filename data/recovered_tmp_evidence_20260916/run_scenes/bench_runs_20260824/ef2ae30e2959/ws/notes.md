# Notes

## Task Overview
- 33 molecules in pool, 4 per page (9 pages total)
- Budget: 17 units shared between list_pool and measure_qed
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (never synthesized, probe_unavailable)

## Congeneric Series Analysis (m05 series)
Scaffold: COc1ccc(cc1)C(=O)N- with varying N-alkyl chain lengths

Molecules in m05 series:
- m15: NCC (2 carbons), QED = 0.7621
- m24: NCCC (3 carbons), QED = 0.7919
- m28: NCCCC (4 carbons), QED = 0.7518
- m17: NCCCCC (5 carbons), QED = 0.7498
- m22: NCCCCCC (6 carbons), QED = 0.7377
- m05: NCCCCCCCCCCCC (12 carbons) - TARGET

## QED Trend for m05 Prediction
Peak at chain length 3 (QED=0.7919), then gradual decline.
Decline rate from length 3-6: ~0.0181 per carbon

Linear extrapolation to length 12:
- From length 6: 0.7377 - 6*0.0181 = 0.6291

**Predicted QED for m05: ~0.63**

## Task 2 - Lead Molecule Search
Criteria: QED ≥ 0.6, psa ≥ 40, NOT in m05 series

Scaffolds and PSA values:
- Scaffold A (m05 series): psa = 38.33 (< 40, excluded anyway)
- Scaffold B (OCC(O)C(O)C(O)C(=O)N-): psa = 110.02 ✓
- Scaffold C (O=[N+]([O-])c1ccc(cc1)C(=O)N-): psa = 72.24 ✓
- Scaffold D (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): psa = 29.1 (< 40, excluded)

## Measurements Performed
| mol_id | scaffold | psa | QED | Meets criteria? |
|--------|----------|-----|-----|-----------------|
| m15 | A (m05 series) | 38.33 | 0.7621 | No (same series) |
| m24 | A (m05 series) | 38.33 | 0.7919 | No (same series) |
| m28 | A (m05 series) | 38.33 | 0.7518 | No (same series) |
| m17 | A (m05 series) | 38.33 | 0.7498 | No (same series) |
| m22 | A (m05 series) | 38.33 | 0.7377 | No (same series) |
| m01 | B | 110.02 | 0.3187 | No (QED < 0.6) |
| m02 | C | 72.24 | 0.6046 | **YES** ✓ |
| m09 | C | 72.24 | 0.5826 | No (QED < 0.6) |
| m12 | C | 72.24 | N/A | Budget exhausted |

## Selected Lead Molecule
**m02** - O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- QED measured: 0.6046 (≥ 0.6 ✓)
- psa: 72.24 (≥ 40 ✓)
- Scaffold C, not in m05 series (Scaffold A) ✓

## Budget Usage
- list_pool: 9 pages = 9 units
- measure_qed: 8 measurements = 8 units
- Total: 17/17 units (budget exhausted)
