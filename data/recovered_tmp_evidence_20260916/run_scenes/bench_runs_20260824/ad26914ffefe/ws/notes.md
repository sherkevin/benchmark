# Notes for QED Prediction Task

## Initial Analysis

Target molecule m05: SMILES = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

This appears to be a methoxybenzamide with a long N-alkyl chain (13 carbons in the chain? Let's count: NCCCCCCCCCCC = N + 12 C's = 13 total carbons including the N-attached carbon).

From examining the SMILES patterns in the pool, I can see several scaffold families:

1. **Methoxybenzamides** (like m05): Pattern `COc1ccc(cc1)C(=O)N...` 
   - m15: `COc1ccc(cc1)C(=O)NCC` (chain: NCC = 2C)
   - m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain: NCCCCC = 5C)
   - m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain: NCCCCCC = 6C)
   - m24: `COc1ccc(cc1)C(=O)NCCC` (chain: NCCC = 
   - m28: `COc1ccc(cc1)C(=O)NCCCC` (chain: NCCCC = 4C)

2. **Nitrobenzamides**: Pattern `O=[N+]([O-])c1ccc(cc1)C(=O)N...`
   - Many molecules with varying chain lengths

3. **Biphenylamides**: Pattern `c1ccc(cc1)-c1ccc(cc1)C(=O)N...`

4. **Polyhydroxy amides**: Pattern `OCC(O)C(O)C(O)C(=O)N...`

Based on scaffold analysis, m05 belongs to the methoxybenzamide series. Let me identify all molecules in this series from the pool.

## Congeneric Series for m05

Found 6 molecules with methoxybenzamide scaffold:
- m15: `COc1ccc(cc1)C(=O)NCC` (chain: NCC = 2C)
- m24: `COc1ccc(cc1)C(=O)NCCC` (chain: NCCC = 3C)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (chain: NCCCC = 4C)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (chain: NCCCCC = 5C)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (chain: NCCCCCC = 6C)
- m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (chain: NCCCCCCCCCCC = 11C)

Note: m05 has never been synthesized (probe_unavailable), so we need to measure the other 5 congeners and extrapolate to chain length 11C.

Chain length counting: Each "C" in the chain after N represents one carbon atom. So:
- m15: NCC = 2 carbons
- m24: NCCC = 3 carbons
- m28: NCCCC = 4 carbons
- m17: NCCCCC = 5 carbons
- m22: NCCCCCC = 6 carbons
- m05: NCCCCCCCCCCC = 11 carbons

We need to measure QED for m15, m24, m28, m17, m22 to establish the trend vs chain length.

## Job 2 Analysis

Need to find a molecule that:
1. Measured QED ≥ 0.6
2. psa ≥ 40 (from data)
3. Not in same series as m05 (not methoxybenzamide)

PSA values by scaffold:
- Methoxybenzamides: psa = 38.33 (< 40) → exclude all
- Nitrobenzamides: psa = 72.24 (≥ 40) → candidates
- Biphenylamides: psa = 29.1 (< 40) → exclude all
- Polyhydroxy amides: psa = 110.02 (≥ 40) → candidates

So potential candidates are all nitrobenzamides and polyhydroxy amides. We need to measure QED for some of these to find one with QED ≥ 0.6.

Total measurement budget: 12 measurements for both jobs.

## Measurement Plan
1. Measure all 5 congeners of m05 series: m15, m24, m28, m17, m22 (5 measurements)
2. Have 7 measurements left for Job 2
3. Need to find at least one molecule with QED ≥ 0.6 from nitrobenzamide or polyhydroxy amide series

Given the budget, I should measure strategically to maximize chances of finding QED ≥ 0.6.

## Job 1 Measurements Completed

Chain length vs QED data for methoxybenzamide series:
- m15 (2C): QED = 0.7621
- m24 (3C): QED = 0.7919  
- m28 (4C): QED = 0.7518
- m17 (5C): QED = 0.7498
- m22 (6C): QED = 0.7377

Chain lengths: 2, 3, 4, 5, 6 carbons
QED values: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377

Need to extrapolate to m05 with chain length = 11 carbons.

## Job 2 Measurements

Measured candidates for Job 2:
- m02 (nitrobenzamide): QED = 0.6046 ✓ (≥ 0.6, psa=72.24, not in m05 series)
- m01 (polyhydroxy amide): QED = 0.3187 ✗ (< 0.6)
- m09 (nitrobenzamide): QED = 0.5826 ✗ (< 0.6)
- m12 (nitrobenzamide): QED = 0.4709 ✗ (< 0.6)
- m32 (nitrobenzamide): QED = 0.4683 ✗ (< 0.6)
- m33 (nitrobenzamide): QED = 0.4591 ✗ (< 0.6)

So m02 is a valid lead compound that satisfies all three constraints.

Measurements used: 11 out of 12 total (1 remaining).

## Final Measurements and Analysis

Used last measurement on m06 (nitrobenzamide): QED = 0.4011 ✗ (< 0.6).

So m02 remains the only valid lead found.

Total measurements used: 12/12.

## Job 1: Extrapolation for m05 QED

Data points for methoxybenzamide series:
- Chain 2C: QED=0.7621, alogp=1.445
- Chain 3C: QED=0.7919, alogp=1.835 (peak)
- Chain 4C: QED=0.7518, alogp=2.225
- Chain 5C: QED=0.7498, alogp=2.615
- Chain 6C: QED=0.7377, alogp=3.005

Target m05: Chain 11C, alogp=4.956

Trend analysis:
1. QED peaks at chain length 3C (0.7919) then decreases.
2. From 3C to 6C: alogp increases by 1.17 (1.835→3.005), QED decreases by 0.0542 (0.7919→0.7377).
3. Rate: 0.0463 QED decrease per unit alogp increase.
4. From 6C to 11C: alogp increases by 1.951 (3.005→4.956).
5. Extrapolated decrease: 1.951 × 0.0463 = 0.0904.
6. Predicted QED for m05: 0.7377 - 0.0904 = 0.6473.

Considering measurement uncertainty and nonlinearity at extreme chain lengths, I predict QED ≈ 0.645 for m05.

This aligns with chemical intuition: very long hydrophobic chains (11C) reduce drug-likeness, but the methoxybenzamide scaffold maintains relatively high QED compared to other scaffolds (e.g., nitrobenzamide m06 with alogp=4.075 has QED=0.4011).