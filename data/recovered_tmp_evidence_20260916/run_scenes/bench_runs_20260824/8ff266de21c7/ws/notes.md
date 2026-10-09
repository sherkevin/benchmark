# Notes: Predicting QED of m05

## Target molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxyphenyl amide (`COc1ccc(cc1)C(=O)N`) with an N-alkyl chain of 11 carbons
- Never synthesized — QED must be predicted by extrapolation

## Search strategy
- Searched for scaffold substring `COc1ccc(cc1)C(=O)N` in compound pool
- Found 6 total matches (including m05 itself) across 2 pages (cost: 2 budget units)

## Congeneric series members found
All share the scaffold `COc1ccc(cc1)C(=O)N` and differ only in N-alkyl chain length:

| mol_id | SMILES chain | N-alkyl C count | MW     | ALogP | ROTB | QED (measured) |
|--------|-------------|-----------------|--------|-------|------|----------------|
| m15    | NCC         | 2               | 179.22 | 1.445 | 3    | 0.7621         |
| m24    | NCCC        | 3               | 193.25 | 1.835 | 4    | 0.7919         |
| m28    | NCCCC       | 4               | 207.27 | 2.225 | 5    | 0.7518         |
| m17    | NCCCCC      | 5               | 221.30 | 2.615 | 6    | 0.7498         |
| m22    | NCCCCCC     | 6               | 235.33 | 3.005 | 7    | 0.7377         |
| **m05** (target) | NCCCCCCCCCCC | 11      | 305.46 | 4.956 | 12   | ???            |

## Budget used
- 2 search pages + 5 QED measurements = 7 out of 12 units

## Observed trend
- QED peaks at chain length 3 (m24: 0.7919) then declines
- Decline from n=3 to n=6: 0.7919 → 0.7377 (drop of 0.0542 over 3 CH2 additions)
- Step-wise changes: +0.0298 (2→3), -0.0401 (3→4), -0.0020 (4→5), -0.0121 (5→6)
- The overall shape is a peak followed by decline, consistent with MW desirability initially improving while ALogP/ROTB penalties grow

## Model: Quadratic fit on QED vs chain length (n)
Fitted on all 5 measured congeners:

**QED(n) = -0.003264·n² + 0.017022·n + 0.749324**

Goodness of fit:
| n | QED actual | QED predicted | Error  |
|---|-----------|--------------|--------|
| 2 | 0.7621    | 0.7703       | +0.008 |
| 3 | 0.7919    | 0.7710       | -0.021 |
| 4 | 0.7518    | 0.7652       | +0.013 |
| 5 | 0.7498    | 0.7528       | +0.003 |
| 6 | 0.7377    | 0.7340       | -0.004 |

Mean absolute error: 0.010

### Why quadratic?
- Captures the observed peak (maximum at n ≈ 2.6)
- Captures accelerating decline (negative quadratic coefficient)
- Physically motivated: ALogP and ROTB penalties in QED grow non-linearly
- Linear fit gives 0.695 but ignores the curvature; simple last-3-point extrapolations are unreliable

### Cross-check: ln(QED) quadratic
Fitting ln(QED) = -0.004257·n² + 0.022076·n - 0.28818 gives QED(11) = 0.571, consistent in magnitude.

## Extrapolation to n=11 (m05)
QED(11) = -0.003264 × 121 + 0.017022 × 11 + 0.749324
         = -0.39494 + 0.18724 + 0.74932
         = **0.542**

### Physical sanity check
- At n=11: ALogP=4.956 (near Lipinski cutoff of 5 — severe penalty), ROTB=12 (high flexibility penalty), MW=305 (still acceptable)
- QED ≈ 0.54 is reasonable for a highly lipophilic, flexible molecule
- The quadratic predicts QED→0 at n≈18 (an 18-carbon chain would give a waxy, extremely lipophilic compound)

## Prediction
**QED of m05 ≈ 0.54**

## Congener IDs
m15, m24, m28, m17, m22
