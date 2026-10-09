# Notes

## Task Overview
- Task 1: Report QED of m05 (not synthesized, need to predict from congeneric series)
- Task 2: Pick one molecule with QED >= 0.6, psa >= 40, NOT in same series as m05
- Budget: 12 measurements total

## Understanding m05's Congeneric Series
m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - has a methoxy-phenyl-amide scaffold with N-alkyl chain

Looking at the pool, molecules with similar scaffold pattern `COc1ccc(cc1)C(=O)N...`:
- m05: NCCCCCCCCCCC (13 carbons in alkyl chain)
- m15: NCC (2 carbons)
- m17: NCCCCC (5 carbons)
- m22: NCCCCCC (6 carbons)
- m24: NCCC (3 carbons)
- m28: NCCCC (4 carbons)

These all share the same scaffold and differ only in N-alkyl chain length.

## Plan
1. First, use group_by_scaffold to confirm scaffold groupings
2. Measure QED for molecules in m05's series to build a trend
3. Predict m05's QED using fit_trend
4. Find a molecule not in m05's series with psa >= 40 and measure its QED

## Molecules with psa >= 40 (from pool.tsv):
- m01: psa=110.02 (scaffold: OCC(O)C(O)C(O)C(=O)N...)
- m03: psa=110.02
- m07: psa=110.02
- m14: psa=110.02
- m18: psa=110.02
- m19: psa=110.02
- m21: psa=110.02
- m25: psa=110.02
- m27: psa=110.02
- m02: psa=72.24 (nitro-phenyl-amide series)
- m06: psa=72.24
- m09: psa=72.24
- m10: psa=72.24
- m12: psa=72.24
- m29: psa=72.24
- m31: psa=72.24
- m32: psa=72.24
- m33: psa=72.24

m05 has psa=38.33, so molecules in its series also have psa=38.33.

Need to find molecules NOT in m05's series with psa >= 40.

## Measurements Taken

### m05 series (for Task 1 trend):
- m15 (chain_len=2): QED=0.7621
- m24 (chain_len=3): QED=0.7919
- m28 (chain_len=4): QED=0.7518
- m17 (chain_len=5): QED=0.7498
- m22 (chain_len=6): QED=0.7377

### Trend prediction for m05 (chain_len=11):
Using quadratic fit: prediction = 0.5416

### Task 2 candidates measured:
- m01 (psa=110.02): QED=0.3187 - FAILS (QED < 0.6)
- m02 (psa=72.24): QED=0.6046 - PASSES all criteria!
- m14 (psa=110.02): QED=0.3237 - FAILS (QED < 0.6)
- m09 (psa=72.24): QED=0.5826 - FAILS (QED < 0.6)

## Final Answers
- Task 1: m05 QED = 0.5416 (predicted from trend)
- Task 2: m02 qualifies (QED=0.6046, psa=72.24, not in m05's series)
