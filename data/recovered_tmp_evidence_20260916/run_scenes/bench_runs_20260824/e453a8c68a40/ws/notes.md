# Notes for QED Prediction Task

## Initial Analysis

Target molecule: m05 with SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (12-carbon chain)

From examining SMILES patterns in the pool, I've identified the following molecules as belonging to the same series as m05 (same scaffold with methoxy benzene carbonyl, differing only in alkyl chain length):

1. m15: `COc1ccc(cc1)C(=O)NCC` (2-carbon chain)
2. m24: `COc1ccc(cc1)C(=O)NCCC` (3-carbon chain)
3. m28: `COc1ccc(cc1)C(=O)NCCCC` (4-carbon chain)
4. m17: `COc1ccc(cc1)C(=O)NCCCCC` (5-carbon chain)
5. m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6-carbon chain)

These are the "short-chain" members mentioned in the task. m05 has a 12-carbon chain, so we need to extrapolate beyond the measured range.

Other series in the pool appear to be:
- OCC(O)C(O)C(O)C(=O)N... (polyol series)
- O=[N+]([O-])c1ccc(cc1)C(=O)N... (nitrobenzene series)
- c1ccc(cc1)-c1ccc(cc1)C(=O)N... (biphenyl series)

## Job 1: Predicting m05 QED

I measured all 5 short-chain members of m05's congeneric series:
1. m15 (2C): QED = 0.7621
2. m24 (3C): QED = 0.7919  
3. m28 (4C): QED = 0.7518
4. m17 (5C): QED = 0.7498
5. m22 (6C): QED = 0.7377

**Trend analysis:**
- QED peaks at 3-carbon chain (0.7919)
- After the peak, QED decreases with increasing chain length
- From 3C to effect (3 carbons): decrease of 0.0542 total, average -0.01807 per carbon
- The decrease shows some variability: -0.0401 (3C→4C), -0.0020 (4C→5C), -0.0121 (5C→6C)

**Extrapolation to m05 (12C):**
- m05 has 12-carbon chain, 6 carbons beyond the last measured point (6C)
- Using the average decrease rate of -0.01807 per carbon from 3C-6C: 0.7377 - (6 × 0.01807) = 0.6293
- Using the last observed decrease rate (5C→6C: -0.0121): 0.7377 - (6 × 0.0121) = 0.6651
- Considering the high alogp of m05 (4.956 vs 3.005 for 6C), which suggests reduced drug-likeness
- **Prediction: 0.63** (conservative estimate accounting for extended alkyl chain)

## Job 2: Finding Lead Compound

**Constraints:**
1. QED ≥ 0.6 (must measure)
2. psa ≥ 40 (from data)
3. Not in m05's series (from SMILES)

**Candidate screening:**
- Biphenyl series (psa = 29.1) and m05's series (psa = 38.33) fail psa ≥ 40 constraint
- Polyol series (psa = 110.02) and nitrobenzene series (psa = 72.24) satisfy psa constraint

**Measurements made (7 remaining after Job 1):**
1. m01 (polyol): QED = 0.3187 (< 0.6)
2. m03 (polyol): QED = 0.3282 (< 0.6)
3. m02 (nitrobenzene): QED = 0.6046 (≥ 0.6) - VALID CANDIDATE
4. m09 (nitrobenzene): QED = 0.5826 (< 0.6)
5. m12 (nitrobenzene): QED = 0.4709 (< 0.6)
6. m32 (nitrobenzene): QED = 0.4683 (< 0.6)
7. m07 (polyol): QED = 0.3233 (< 0.6)

**Selected lead: m02**
- QED = 0.6046 (≥ 0.6, measured)
- psa = 72.24 (≥ 40, from data)
- Not in m05's series: scaffold is nitrobenzene (O=[N+]([O-])c1ccc(cc1)C(=O)NCCC) vs methoxybenzene for m05 series
- Satisfies all three constraints

## Final Deliverables
- Predicted QED for m05: 0.63
- Lead compound: m02 with measured QED 0.6046
- Congeneric series members: m15, m24, m28, m17, m22