# Notes on QED Prediction for m05

## Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This is a 4-methoxybenzamide with an N-alkyl chain
- The chain is 11 carbons long (CCCCCCCCCCC = undecyl)

## Strategy
1. Find the scaffold by identifying the common substructure
2. Search the pool for congeners (same scaffold, different chain lengths)
3. Measure QED of congeners to establish the trend
4. Extrapolate to predict QED for m05 (11-carbon chain)

## Step 1: Identify the scaffold
The target SMILES is: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

The scaffold (without the variable chain) should be: `COc1ccc(cc1)C(=O)N`
The variable part is the N-alkyl chain: `CCCCCCCCCCC` (11 carbons)

## Step 2: Search for congeners
Searched with query `COc1ccc(cc1)C(=O)N` - found 6 total matches (including m05).

### Congeners found (same series as m05):
| mol_id | SMILES | Chain | Chain Length |
|--------|--------|-------|--------------|
| m15 | COc1ccc(cc1)C(=O)NCC | CC | 2 carbons |
| m24 | COc1ccc(cc1)C(=O)NCCC | CCC | 3 carbons |
| m28 | COc1ccc(cc1)C(=O)NCCCC | CCCC | 4 carbons |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | CCCCC | 5 carbons |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | CCCCCC | 6 carbons |

## Step 3: Measure QED of congeners
| mol_id | Chain Length (n) | QED |
|--------|-----------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Step 4: Analyze the trend

Looking at the data:
- n=2: QED = 0.7621
- n=3: QED = 0.7919 (peak)
- n=4: QED = 0.7518 (drop)
- n=5: QED = 0.7498 (slight drop)
- n=6: QED = 0.7377 (continuing drop)

The trend shows:
1. QED peaks at n=3 (propyl chain)
2. After the peak, QED decreases as chain length increases
3. The decrease appears to be roughly linear from n=3 onwards

### Fitting a trend

From n=3 to n=6, the QED values are:
- n=3: 0.7919
- n=4: 0.7518 (drop of 0.0401)
- n=5: 0.7498 (drop of 0.0020)
- n=6: 0.7377 (drop of 0.0121)

Average drop per carbon from n=3 to n=6: (0.7919 - 0.7377) / 3 = 0.0181 per carbon

This is not perfectly linear - there's variability. Let me fit a simple linear regression from n=3 onwards:

Using points (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377):

Linear regression: QED = a + b*n

Mean n = 4.5, Mean QED = 0.7578

Sum of (n - mean_n)(QED - mean_QED):
(3-4.5)(0.7919-0.7578) = -1.5 * 0.0341 = -0.05115
(4-4.5)(0.7518-0.7578) = -0.5 * -0.0060 = 0.0030
(5-4.5)(0.7498-0.7578) = 0.5 * -0.0080 = -0.0040
(6-4.5)(0.7377-0.7578) = 1.5 * -0.0201 = -0.03015

Sum = -0.0823

Sum of (n - mean_n)^2:
(-1.5)^2 + (-0.5)^2 + (0.5)^2 + (1.5)^2 = 2.25 + 0.25 + 0.25 + 2.25 = 5.0

Slope b = -0.0823 / 5.0 = -0.01646

Intercept a = mean_QED - b * mean_n = 0.7578 - (-0.01646 * 4.5) = 0.7578 + 0.07407 = 0.83187

So: QED = 0.83187 - 0.01646 * n (for n >= 3)

For n=11 (m05):
QED = 0.83187 - 0.01646 * 11 = 0.83187 - 0.18106 = 0.65081

However, this linear extrapolation might be too aggressive. The trend might plateau at longer chain lengths.

### Alternative: Consider the overall pattern

Looking at the full pattern including n=2:
- The QED rises from n=2 to n=3, then falls
- This suggests an optimal chain length around 3 carbons
- For longer chains, lipophilicity increases, which typically reduces QED

Given that:
- From n=3 to n=6, QED drops by 0.0542 over 3 carbons
- That's about 0.018 per carbon on average

Extrapolating from n=6 to n=11 (5 more carbons):
If the trend continues linearly: 0.7377 - 5*0.018 = 0.7377 - 0.09 = 0.6477

But QED is bounded between 0 and 1, and the rate of decrease might slow as chains get very long (the effect of adding one more carbon diminishes).

A more conservative estimate might be to assume the rate of decrease slows:
- Perhaps QED approaches an asymptote around 0.5-0.6 for very long chains

Given the data and the extrapolation distance (from n=6 to n=11 is a big gap), I'll estimate:
**Predicted QED for m05 (n=11): ~0.62-0.65**

Let me refine this. Using the linear model from n=3 to n=6:
QED ≈ 0.832 - 0.0165*n

For n=11: QED ≈ 0.832 - 0.0165*11 = 0.832 - 0.1815 = 0.6505

I'll predict **0.65** as the QED for m05.

## Final Answer
- **target_qed**: 0.65
- **congener_ids**: ["m15", "m17", "m22", "m24", "m28"]
