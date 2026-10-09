# Task Analysis and Approach

## Task Overview
1. Predict QED for m05 (target molecule) which has never been synthesized
2. Pick a backup lead compound meeting three constraints:
   - Measured QED ≥ 0.6
   - PSA ≥ 40
   - Not in the same series as m05

Total budget: 12 measurements shared between both jobs.

## Initial Observations

### Target molecule m05:
SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Contains methoxybenzamide scaffold: `COc1ccc(cc1)C(=O)N`
- Has a very long alkyl chain: `CCCCCCCCCCC` (11 carbons)
- This is an N-alkyl chain attached to the amide nitrogen

### Identifying m05's congeneric series:
Looking at SMILES patterns, molecules with the same scaffold as m05 should have:
- `COc1ccc(cc1)C(=O)N` (methoxybenzamide) 
- Followed by varying lengths of alkyl chain (`CC...`)

From scanning the pool, I can see several molecules with this pattern:
- m15: `COc1ccc(cc1)C(=O)NCC` (2 carbons)
- m24: `COc1ccc(cc1)C(=O)NCCC` (3 carbons)  
- m28: `COc1ccc(cc1)C(=O)NCCCC` (4 carbons)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 carbons)
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 carbons)

m05 has 11 carbons in the alkyl chain.

## Strategy
1. First identify all molecules in m05's series (same scaffold, different alkyl chain lengths)
2. Measure these to understand QED trend vs alkyl chain length
3. Extrapolate to m05 (11 carbons) - note that pool only has short-chain members
4. For lead selection: filter by PSA ≥ 40 and not in m05 series, then measure promising candidates

## Job 1 Progress

### m05's congeneric series identified:
- m15: COc1ccc(cc1)C(=O)NCC (2 carbons) - QED: 0.7621
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbons) - QED: 0.7919  
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbons) - QED: 0.7518
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbons) - QED: 0.7498
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbons) - QED: 0.7377
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11 carbons) - NEED TO PREDICT

### Observed QED trend:
Chain length (carbons) → QED:
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518
- 5C: 0.7498
- 6C: 0.7377

The trend shows:
1. Initial increase from 2C to modeledC
2. Gradual decrease from 3C onward
3. Appears to be approaching an asymptote or continuing to decrease slowly

### Extrapolation considerations:
- The pool only has short-chain members (2-6 carbons)
- Need to extrapolate to 11 carbons
- The decrease from 3C to 6C is relatively gradual: -0.0542 over 3 carbons (~ -0.0181 per carbon)
- If this linear trend continues, by 11 carbons we'd see further decrease
- However, QED curves often flatten out at longer chain lengths

## Job 2 Progress

### Candidates meeting initial filters:
1. PSA ≥ 40 (excludes m05 series which has PSA = 38.33)
2. Not in m05 series (already satisfied by PSA filter)

Two scaffold families meet PSA ≥ 40:
1. **OCC series** (sugar-like, PSA = 110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
2. **O=[N+] series** (nitrobenzene, PSA = 72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements:
- OCC series (all low QED ~0.323-0.324):
  - m14 (2C): QED = 0.3237
  - m19 (5C): QED = 0.3244  
  - m07 (9C): QED = 0.3233

- Nitro series:
  - m09 (2C): QED = 0.5826 (< 0.6)
  - m02 (3C): QED = 0.6046 (≥ 0.6) ✓

### Lead candidate identified: m02
- QED = 0.6046 ≥ 0.6 ✓
- PSA = 72.24 ≥ 40 ✓  
- Not in m05 series (different scaffold: nitrobenzene vs methoxybenzene) ✓

## Job 1 Final Prediction

### Data for m05 series:
Chain length (carbons) → QED:
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518  
- 5C: 0.7498
- 6C: 0.7377

### Trend analysis:
- Clear peak at 3 carbons
- Gradual decrease from 3C to 6C
- Average decrease from peak (3C) to last point (6C): -0.0542 over 3 carbons = -0.0181 per carbon

### Extrapolation to m05 (11 carbons):
- m05 has 11C, which is 5 carbons beyond last measured (6C)
- If decrease continues at average rate: 5 × -0.0181 = -0.0905
- Predicted QED: 0.7377 - 0.0905 = 0.6472

### Supporting evidence from other series:
- Nitro series (O=[N+]) shows similar pattern: peak at 2-3C, then decrease with longer chains
- m10 (10C in nitro series): QED = 0.3724 vs m09 (2C): 0.5826 → confirms longer chains generally reduce QED
- m06 (9C in nitro series): QED = 0.4011

### Final prediction for m05 QED: 0.647

## Budget Summary
Total measurements used: 12/12
- m05 series: 5 measurements (m15, m24, m28, m17, m22)
- OCC series: 3 measurements (m14, m19, m07)  
- Nitro series: 4 measurements (m09, m02, m10, m06)
- Lead identified: m02 (QED = 0.6046)

## Conclusion
Both tasks completed successfully:

1. **Job 1**: Predicted QED for m05 as 0.647 based on extrapolation of its congeneric series trend
2. **Job 2**: Identified m02 as lead compound meeting all constraints:
   - Measured QED = 0.6046 ≥ 0.6
   - PSA = 72.24 ≥ 40
   - Not in m05 series (different nitrobenzene scaffold)

All 12 measurement budget units used efficiently.