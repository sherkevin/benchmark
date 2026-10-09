# Notes on QED Prediction Task

## Task Summary
- Target molecule: `m05` with SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- QED cannot be measured directly (probe_unavailable)
- Need to find congeners (same scaffold, different chain lengths) and extrapolate QED

## Strategy
1. Search for molecules with the scaffold portion of the target SMILES
2. Identify which ones are in the same series (same scaffold, varying N-alkyl chain length)
3. Measure QED for the congeners found
4. Fit a trend and extrapolate to predict m05's QED

## Target Analysis
Target SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- This appears to be an amide with a methoxy-phenyl group and a long alkyl chain
- The chain part is `CCCCCCCCCCC` (11 carbons)
- Scaffold likely: `COc1ccc(cc1)C(=O)N` with varying chain lengths

## Budget
- 12 units total (search pages + measurements)
- Used: 2 for search + 5 for measurements = 7 units

## Search Results
Found 6 molecules matching scaffold `COc1ccc(cc1)C(=O)N`:
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11 carbons) - TARGET (cannot measure)
- m15: `COc1ccc(cc1)C(=O)NCC` (2 carbons)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons)
- m24: `COc1ccc(cc1)C(=O)NCCC` (3 carbons)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4 carbons)

**Congeners in same series as m05**: m15, m17, m22, m24, m28

## QED Measurements
| Mol ID | Chain Length | QED |
|--------|-------------|-----|
| m15    | 2           | 0.7621 |
| m24    | 3           | 0.7919 |
| m28    | 4           | 0.7518 |
| m17    | 5           | 0.7498 |
| m22    | 6           | 0.7377 |

## Trend Analysis
The QED values show a non-linear pattern:
- Increases from chain 2 (0.7621) to chain 3 (0.7919) - peak
- Decreases from chain 3 to 4 (0.7518)
- Continues decreasing through chain 5 (0.7498) and 6 (0.7377)

This is an inverted-U shape, typical for drug-likeness vs. chain length relationships.
QED penalizes both too low and too high lipophilicity (among other factors).

### Fitting a Model
The data from chain lengths 2-6 shows:
- Peak at n=3
- Decline thereafter

For extrapolation to n=11, I need to consider:
1. The decline from n=3 to n=6: 0.7919 → 0.7377 (drop of 0.0542 over 3 units)
2. Average decline rate from peak: ~0.018 per carbon

If this linear decline continued:
- At n=11: 0.7919 - 0.018*(11-3) = 0.7919 - 0.144 = 0.6479

But the decline might not be linear - it could be quadratic or asymptotic.

Looking at the differences:
- 3→4: -0.0401
- 4→5: -0.0020
- 5→6: -0.0121

The decline is not monotonic in rate. Let me fit a quadratic model:
QED = a*n² + b*n + c

Using points (2, 0.7621), (3, 0.7919), (6, 0.7377):
- 4a + 2b + c = 0.7621
- 9a + 3b + c = 0.7919
- 36a + 6b + c = 0.7377

Solving:
From eq1 and eq2: 5a + b = 0.0298
From eq2 and eq3: 27a + 3b = -0.0542

From first: b = 0.0298 - 5a
Substitute: 27a + 3(0.0298 - 5a) = -0.0542
27a + 0.0894 - 15a = -0.0542
12a = -0.1436
a = -0.01197

b = 0.0298 - 5(-0.01197) = 0.0298 + 0.05985 = 0.08965

c = 0.7621 - 4a - 2b = 0.7621 - 4(-0.01197) - 2(0.08965)
c = 0.7621 + 0.04788 - 0.1793 = 0.63068

Quadratic: QED = -0.01197*n² + 0.08965*n + 0.63068

Check at n=3: -0.01197*9 + 0.08965*3 + 0.63068 = -0.10773 + 0.26895 + 0.63068 = 0.7919 ✓
Check at n=5: -0.01197*25 + 0.08965*5 + 0.63068 = -0.29925 + 0.44825 + 0.63068 = 0.77968 (actual 0.7498)

The quadratic fit using just 3 points doesn't perfectly match all data. Let me try a different approach.

### Alternative: Linear regression on declining portion (n=3 to n=6)
Points: (3, 0.7919), (4, 0.7518), (5, 0.7498), (6, 0.7377)

Mean n = 4.5, Mean QED = 0.7578

Slope = Σ((n-mean_n)*(QED-mean_QED)) / Σ((n-mean_n)²)
= ((3-4.5)*(0.7919-0.7578) + (4-4.5)*(0.7518-0.7578) + (5-4.5)*(0.7498-0.7578) + (6-4.5)*(0.7377-0.7578))
  / ((3-4.5)² + (4-4.5)² + (5-4.5)² + (6-4.5)²)
= ((-1.5)*0.0341 + (-0.5)*(-0.006) + (0.5)*(-0.008) + (1.5)*(-0.0201)) / (2.25 + 0.25 + 0.25 + 2.25)
= (-0.05115 + 0.003 - 0.004 - 0.03015) / 5
= -0.0823 / 5
= -0.01646

Intercept = 0.7578 - (-0.01646)*4.5 = 0.7578 + 0.07407 = 0.83187

Linear model for declining portion: QED = 0.83187 - 0.01646*n

At n=11: QED = 0.83187 - 0.01646*11 = 0.83187 - 0.18106 = 0.6508

### Another approach: Exponential decay from peak
If QED decays exponentially from the peak after n=3:
QED(n) = QED_peak * exp(-k*(n-3)) + baseline

This is getting complex. Let me use a simpler pragmatic approach.

### Pragmatic approach
The data shows QED declining from 0.7919 at n=3 to 0.7377 at n=6.
The average decline is (0.7919 - 0.7377) / 3 = 0.0181 per carbon.

If we extrapolate linearly from n=6:
QED(11) ≈ 0.7377 - 0.0181*(11-6) = 0.7377 - 0.0905 = 0.6472

However, QED typically has a floor effect - it can't go below 0 and the decline often slows.
Looking at the data, the decline from 4→5 was only 0.002, suggesting the relationship isn't purely linear.

Given the task says "do not just quote the nearest measured molecule" and "fit the trend", I'll use the linear regression on the declining portion:

**Prediction: QED ≈ 0.65**

This accounts for:
1. The peak at chain length 3
2. The systematic decline from 3→6
3. Extrapolation to chain length 11

The congener_ids for answer.json: ["m15", "m17", "m22", "m24", "m28"]
