# QED Prediction Analysis

## Target Molecule
- ID: m05
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- Alkyl chain length: 11 carbons

## Search Strategy
- Used scaffold substring: `COc1ccc(cc1)C(=O)N`
- Found 6 total matches (including target)
- Budget used: 2 units (2 pages of search)

## Congeners Found
All share the same scaffold `COc1ccc(cc1)C(=O)N` with varying alkyl chain lengths:

1. m15: C2 chain (ethyl) - COc1ccc(cc1)C(=O)NCC
2. m24: C3 chain (propyl) - COc1ccc(cc1)C(=O)NCCC  
3. m28: C4 chain (butyl) - COc1ccc(cc1)C(=O)NCCCC
4. m17: C5 chain (pentyl) - COc1ccc(cc1)C(=O)NCCCCC
5. m22: C6 chain (hexyl) - COc1ccc(cc1)C(=O)NCCCCCC
6. m05: C11 chain (undecyl) - COc1ccc(cc1)C(=O)NCCCCCCCCCCC (target)

## QED Measurements
Measured QED values:

| Molecule | Chain Length (C) | QED |
|----------|------------------|-----|
| m15      | 2                | 0.7621 |
| m24      | 3                | 0.7919 |
| m28      | 4                | 0.7518 |
| m17      | 5                | 0.7498 |
| m22      | 6                | 0.7377 |

Target m05 confirmed unavailable for measurement (probe_unavailable).

## Trend Analysis
The QED values show:
1. Increase from C2 (0.7621) to C3 (0.7919) - peak at C3
2. Steady decrease from C3 to C6: 0.7919 → 0.7518 → 0.7498 → 0.7377

This suggests a quadratic relationship with a maximum around C3 chain length.

## Decline Analysis
Rate of QED decrease per additional carbon:
- C3→C4: -0.0401
- C4→C5: -0.0020 (anomalously small)
- C5→C6: -0.0121

Average decline from C3 to C6: (0.7919 - 0.7377) / 3 = 0.01807 per carbon
Most recent decline (C5→C6): -0.0121 per carbon

## Extrapolation to C11 (Target)
Target chain length: C11 (5 carbons beyond last measured C6)

Using most recent decline rate (C5→C6 = -0.0121 per carbon):
Predicted QED at C11 = 0.7377 - (5 × 0.0121) = 0.7377 - 0.0605 = 0.6772

Using average decline from C3 to C6 (0.01807 per carbon):
Predicted QED at C11 = 0.7377 - (5 × 0.01807) = 0.7377 - 0.09035 = 0.64735

Considering the trend shows decreasing QED with increasing chain length beyond C3, and the most recent measured decline (C5→C6) was -0.0121, this seems representative of ongoing decline rate.

## Final Prediction
Using the most recent decline rate (C5→C6 = -0.0121 per carbon):
Predicted QED at C11 = 0.7377 - (5 × 0.0121) = 0.7377 - 0.0605 = 0.6772

Rationale:
1. The C5→C6 decline (-0.0121) represents the most recent trend
2. Molecular descriptors (alogp, mw, rotb) show consistent linear changes with chain length
3. At C11, alogp=4.956 (beyond optimal range), rotb=12 (beyond optimal), supporting continued QED decline
4. The prediction of 0.6772 represents a reasonable extrapolation beyond measured range

## Budget Summary
- Search: 2 units (2 pages)
- Measurements: 5 units (m15, m24, m28, m17, m22)
- Total used: 7 units of 12 available