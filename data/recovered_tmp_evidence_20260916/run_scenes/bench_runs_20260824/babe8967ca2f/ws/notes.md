# Notes on QED Prediction for m05

## Target Molecule
- **m05**: SMILES = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is a methoxy-phenyl amide with a 13-carbon alkyl chain

## Strategy
1. Search for the scaffold `COc1ccc(cc1)C(=O)N` to find all congeners in the same series
2. The congeners will differ only in the alkyl chain length
3. Measure QED for each congener found
4. Fit a trend and extrapolate to predict QED for the 13-carbon chain

## Search Results

### Search: Scaffold `COc1ccc(cc1)C(=O)N`
Found 6 total matches across 2 pages:

| mol_id | SMILES | Chain | Chain Length | MW | logP |
|--------|--------|-------|--------------|-----|------|
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | C13 | 13 | 305.46 | 4.956 |
| m15 | COc1ccc(cc1)C(=O)NCC | C2 | 2 | 179.22 | 1.445 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | C5 | 5 | 221.30 | 2.615 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | C6 | 6 | 235.33 | 3.005 |
| m24 | COc1ccc(cc1)C(=O)NCCC | C3 | 3 | 193.25 | 1.835 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | C4 | 4 | 207.27 | 2.225 |

## QED Measurements

Measured all congeners (excluding target m05):

| mol_id | Chain Length | QED |
|--------|--------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Trend Analysis

The QED trend shows:
- Peak at chain length 3 (QED = 0.7919)
- Decline from n=3 to n=6

Pattern:
- n=2 → n=3: +0.0298 (increase)
- n=3 → n=4: -0.0401 (sharp drop)
- n=4 → n=5: -0.0020 (small drop)
- n=5 → n=6: -0.0121 (moderate drop)

### Fitting a Model

**Linear regression on n=3 to n=6:**
- Slope ≈ -0.0165 per carbon
- Extrapolated to n=13: QED ≈ 0.62

**Quadratic model (vertex at n=3, QED=0.792):**
- QED(n) ≈ -0.006(n-3)² + 0.792
- For n=13: QED ≈ 0.19

**Chemical consideration:**
The target m05 has:
- MW = 305.46 (high, but under 350 threshold)
- logP = 4.956 (quite high, optimal is ~2-3)
- rotb = 12 (many rotatable bonds)

Compared to m22 (n=6, QED=0.7377, logP=3.0), m05 has significantly higher logP (4.96 vs 3.0), which should incur substantial QED penalty.

## Final Prediction

Given:
1. The observed declining trend from n=3 to n=6
2. Non-linear penalties in QED for extreme values (high logP, high MW)
3. The target is far outside the measured range (n=13 vs measured n=2-6)

I predict a QED that accounts for:
- The declining trend (~0.62 from linear)
- Additional penalty for extreme hydrophobicity

**Predicted QED for m05: 0.58**

This reflects:
- Linear extrapolation from n=3-6 trend: ~0.62
- Quadratic model suggests steeper decline: ~0.19
- Chemical properties (logP=4.96, rotb=12) indicate moderate penalty
- Final estimate balances these: 0.58

The congener IDs in the same series as m05 are: m15, m24, m28, m17, m22

