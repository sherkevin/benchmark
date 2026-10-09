# QED Prediction for m05

## Target Molecule
- **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- **Analysis**: Methoxybenzamide scaffold with N-alkyl chain of 11 carbons
- **Cannot be measured**: `measure_qed` returns `probe_unavailable`

## Search Strategy
1. **Scaffold substring**: Used `COc1ccc(cc1)C(=O)N` as query to find congeners
2. **Found 6 molecules** total (including target m05)
3. **Budget used**: 2 units for 2 pages of search results

## Congeners Found
All share scaffold `COc1ccc(cc1)C(=O)N` with varying alkyl chain lengths:
1. m15: `COc1ccc(cc1)C(=O)NCC` (2-carbon chain)
2. m24: `COc1ccc(cc1)C(=O)NCCC` (3-carbon chain)
3. m28: `COc1ccc(cc1)C(=O)NCCCC` (4-carbon chain)
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` (5-carbon chain)
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6-carbon chain)
6. m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11-carbon chain) - target

## QED Measurements
Measured QED values:
1. **m15** (2-carbon chain): QED = 0.7621
2. **m24** (3-carbon chain): QED = 0.7919
3. **m28** (4-carbon chain): QED = 0.7518
4. **m17** (5-carbon chain): QED = 0.7498
5. **m22** (6-carbon chain): QED = 0.7377

**Budget used**: 7 total units (2 for search, 5 for measurements)
**Budget remaining**: 5 units

## Data Analysis
Chain length (n) vs QED:
- n=2: 0.7621
- n=3: 0.7919 (peak)
- n=4: 0.7518
- n=5: 0.7498
- n=6: 0.7377
- n=11: ??? (target, need to predict)

**Observations**:
1. QED initially increases from n=2 to n=3 (0.7621 → 0.7919)
2. QED then decreases as chain length increases from n=3 to n=6
3. The trend appears to follow a downward curve after the peak at n=3

**Rate Analysis**:
- n=3 to n=4: -0.0401 (largest single-step decrease)
- n=4 to n=5: -0.0020 (smallest decrease)
- n=5 to n=6: -0.0121 (intermediate decrease)
- From peak (n=3) to n=6: total decrease of 0.0542 over 3 carbons = ~0.0181 per carbon on average
- Recent trend (n=4 to n=6): decrease of 0.0141 over 2 carbons = ~0.00705 per carbon

**Pattern**: After initial peak, QED decreases with increasing chain length, but the rate of decrease may be slowing.

## Prediction for m05 (n=11)
**Extrapolation considerations**:
1. Target has n=11, which is 5 carbons beyond last measured point (n=6)
2. QED clearly shows decreasing trend after initial peak at n=3
3. Rate of decrease appears variable: -0.0401, -0.0020, -0.0121
4. Average recent decrease rate (n=4 to n=6): ~0.00705 per carbon
5. Overall decrease rate (n=3 to n=6): ~0.0181 per carbon

**Possible extrapolation approaches**:
- Using recent rate (0.00705/carbon): 0.7377 - (5 × 0.00705) = 0.7025
- Using overall rate (0.0181/carbon): 0.7919 - (8 × 0.0181) = 0.6471
- Weighted average rate (~0.010/carbon): 0.7377 - (5 × 0.010) = 0.6877

**Final prediction rationale**:
The true trend likely follows a curve that decreases more rapidly initially and then slows. Given:
1. The irregular but generally decreasing pattern
2. The most recent decrease (n=5 to n=6) was -0.0121
3. QED values are bounded between 0 and 1
4. We're extrapolating well beyond measured range

A reasonable estimate is **0.685**, which accounts for continued decrease but at a slowing rate compared to the n=5 to n=6 change.

**Predicted QED for m05**: 0.685

## Final Budget Status
- **Total budget**: 12 units
- **Search used**: 2 units (2 pages for scaffold query `COc1ccc(cc1)C(=O)N`)
- **Measurements used**: 5 units (m15, m24, m28, m17, m22)
- **Total used**: 7 units
- **Remaining**: 5 units

## Deliverables Created
1. `notes.md` - This file with reasoning
2. `answer.json` - Contains:
   - `target_qed`: 0.685 (predicted QED for m05)
   - `congener_ids`: ["m05", "m15", "m17", "m22", "m24", "m28"]