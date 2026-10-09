# Analysis Notes

## Job 1: Predicting QED of m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to the methoxyphenyl series with an N-undecyl chain (11 carbon atoms on the nitrogen). 

I identified the following congeners in the same series (same scaffold COc1ccc(cc1)C(=O)N, only differing in the N-alkyl chain length):
- m15: N-methyl (1 carbon), QED = 0.7621
- m24: N-propyl (3 carbon), QED = 0.7919
- m28: N-butyl (4 carbon), QED = 0.7518
- m17: N-pentyl (5 carbon), QED = 0.7498
- m22: N-hexyl (6 carbon), QED = 0.7377

Trend analysis: QED initially increases from methyl (0.7621) to propyl (0.7919), then decreases as the chain lengthens further: butyl (0.7518), pentyl (0.7498), hexyl (0.7377).

Since m05 has an N-undecyl chain (11 carbon atoms), which is significantly longer than the measured compounds (1-6 carbon atoms), and considering the decreasing trend observed after the propyl derivative, I extrapolate that m05 would have a QED of approximately 0.68.

## Job 2: Selecting a Lead Compound

I looked for molecules that satisfy all three constraints:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in the same series as m05

From the data, molecules with psa ≥ 40 include those with the trihydroxybutyramide scaffold (OCC(O)C(O)C(O)C(=O)N) and those with the nitrobenzene scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N).

I measured the QED of several candidates:
- m02: QED = 0.6046, psa = 72.24, has nitrobenzene scaffold ✓
- m12: QED = 0.4709, psa = 72.24, has nitrobenzene scaffold (QED < 0.6) 
- m09: QED = 0.5826, psa = 72.24, has nitrobenzene scaffold (QED < 0.6)
- m06: QED = 0.4011, psa = 72.24, has nitrobenzene scaffold (QED < 0.6)
- m32: QED = 0.4683, psa = 72.24, has nitrobenzene scaffold (QED < 0.6)
- m14: QED = 0.3237, psa = 110.02, has trihydroxybutyramide scaffold (QED < 0.6)
- m01: QED = 0.3187, psa = 110.02, has trihydroxybutyramide scaffold (QED < 0.6)

Only m02 meets all three criteria, making it the ideal lead compound.