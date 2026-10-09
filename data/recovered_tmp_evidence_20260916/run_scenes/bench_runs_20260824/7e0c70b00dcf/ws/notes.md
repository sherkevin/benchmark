# Notes: Predicting QED for m05

## Target molecule
- **m05**: SMILES = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is a methoxy-benzamide with a 13-carbon alkyl chain on the nitrogen
- Cannot be measured directly (probe_unavailable)

## Strategy
1. Search the pool for molecules with the same scaffold
2. The scaffold is: `COc1ccc(cc1)C(=O)N` - the methoxy-benzamide core
3. Congeners will differ only in the alkyl chain length

## Search results
Searched for `COc1ccc(cc1)C(=O)N` - found 6 total matches across 2 pages.

### Congeners in the same series (sorted by chain length):
| mol_id | SMILES | Chain | Chain length |
|--------|--------|-------|--------------|
| m15 | COc1ccc(cc1)C(=O)NCC | ethyl | 2 |
| m24 | COc1ccc(cc1)C(=O)NCCC | propyl | 3 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | butyl | 4 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | pentyl | 5 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | hexyl | 6 |
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | tridecyl | 13 (target) |

**Congeners to measure**: m15, m24, m28, m17, m22 (5 molecules)

## QED Measurements
Budget used: 2 for search, 5 for measurements = 7 total (1 remaining)

| mol_id | Chain length | QED |
|--------|--------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis
The QED vs chain length relationship is **non-linear**:
- QED increases from chain length 2 to 3 (0.7621 → 0.7919)
- QED peaks at chain length 3 (0.7919)
- QED then decreases steadily from chain length 3 to 6

The decline from n=3 to n=6:
- n=3→4: -0.0401
- n=4→5: -0.0020
- n=5→6: -0.0121
- Total decline over 3 carbons: 0.0542

This pattern suggests a quadratic/inverted-U relationship where QED peaks at an intermediate chain length (~3 carbons) and decreases as the chain grows longer. This is consistent with drug-likeness principles: longer alkyl chains increase molecular weight and lipophilicity (alogP), eventually pushing the molecule outside optimal ranges.

## Extrapolation to m05 (chain length 13)
The target has a 13-carbon chain, which is 7 carbons longer than the longest measured congener (m22, n=6).

Given:
- The decreasing trend from n=3 onwards
- The rate of decline averages ~0.018 QED units per carbon from n=3 to n=6
- m05 has MW=305.46 and alogP=4.956, both approaching less favorable ranges

Extrapolating the trend:
- From n=6 (QED=0.7377) to n=13 is 7 additional carbons
- If decline continues at ~0.015-0.020 per carbon: 7 × 0.017 ≈ 0.12 decrease
- Predicted QED ≈ 0.7377 - 0.12 ≈ 0.62

However, the decline rate may slow as it approaches a floor. A more conservative estimate accounting for potential plateauing: **QED ≈ 0.58-0.62**

**Final prediction: 0.60**

This reflects the systematic decrease in QED with increasing chain length beyond the optimal n=3, extrapolated over 7 additional carbons to reach the target's n=13.
