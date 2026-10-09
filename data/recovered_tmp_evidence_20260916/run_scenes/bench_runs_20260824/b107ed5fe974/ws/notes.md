# Notes: QED Prediction for m05

## Search Strategy

Used the scaffold substring `COc1ccc(cc1)C(=O)N` to find all congeners in the same series as m05. This efficiently found all 6 matches in 2 pages (budget cost: 2).

## Molecules Found in the Series

| mol_id | SMILES | Chain | mw | alogp | rotb |
|--------|--------|-------|-----|-------|------|
| m15 | COc1ccc(cc1)C(=O)NCC | ethyl (C2) | 179.22 | 1.445 | 3 |
| m24 | COc1ccc(cc1)C(=O)NCCC | propyl (C3) | 193.25 | 1.835 | 4 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | butyl (C4) | 207.27 | 2.225 | 5 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | pentyl (C5) | 221.30 | 2.615 | 6 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | hexyl (C6) | 235.33 | 3.005 | 7 |
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | tridecyl (C13) | 305.46 | 4.956 | 12 |

## QED Measurements

Measured all 5 congeners (budget cost: 5, total budget used: 7/12):

| Chain Length (n) | mol_id | QED |
|------------------|--------|-----|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

## Trend Analysis

The QED vs chain length relationship shows:
1. **Peak at n=3**: QED = 0.7919 (m24)
2. **Declining trend after peak**: QED decreases as chain lengthens beyond propyl

The pattern suggests an inverted-U (parabolic) relationship, common in drug-likeness where moderate lipophilicity is optimal.

### Fitting a Quadratic Model

Using chain length (n) as x and QED as y:
- n=2: y=0.7621
- n=3: y=0.7919 (peak)
- n=4: y=0.7518
- n=5: y=0.7498
- n=6: y=0.7377

Fitting y = a + b*n + c*n²:

The decline from n=3 to n=6 shows:
- n=3→4: Δ = -0.0401
- n=4→5: Δ = -0.0020
- n=5→6: Δ = -0.0121

Average decline per carbon after n=3: ~0.018 per carbon

For extrapolation to n=13 (7 carbons beyond n=6):
- Linear extrapolation from n=6: 0.7377 - 7×0.018 ≈ 0.61
- But the relationship is likely non-linear with accelerating decline

### Alternative: Exponential Decay Model

After the peak, QED may decay toward a lower asymptote. The drop from peak (0.7919) to n=6 (0.7377) is 0.0542 over 3 carbons.

If we model the decline as approaching an asymptote around 0.5-0.6 (typical lower bound for drug-like molecules), the extrapolated value at n=13 would be:

Using a simple model: QED(n) = asymptote + (peak - asymptote) × exp(-k×(n-3))

Fitting roughly: at n=6, QED=0.7377
0.7377 = A + (0.7919-A)×exp(-3k)

With A≈0.55 and solving, we get QED(13) ≈ 0.55-0.60

### Final Prediction

Given:
- The clear downward trend from n=3 to n=6
- The significant extrapolation distance (n=6 to n=13)
- QED's sensitivity to molecular weight and lipophilicity (both increase with chain length)
- m05 has mw=305.46 and alogp=4.956, both outside optimal ranges

**Predicted QED for m05: 0.58**

This represents substantial degradation in drug-likeness due to the very long alkyl chain, consistent with the observed trend and the known behavior of QED metrics.

## Congener IDs in the Same Series as m05

m15, m24, m28, m17, m22
