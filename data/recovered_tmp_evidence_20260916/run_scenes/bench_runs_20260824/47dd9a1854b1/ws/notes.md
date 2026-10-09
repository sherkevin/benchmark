# Task Notes

## Job 1 - Predicting QED for m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has the scaffold "COc1ccc(cc1)C(=O)N" with an N-alkyl chain of length 11. 

The congeners in the same series (same scaffold, different chain lengths) are:
- m15: chain length 2
- m24: chain length 3  
- m28: chain length 4
- m17: chain length 5
- m22: chain length 6

I measured their QED values:
- m15 (chain length 2): QED = 0.7621
- m24 (chain length 3): QED = 0.7919
- m28 (chain length 4): QED = 0.7518
- m17 (chain length 5): QED = 0.7498
- m22 (chain length 6): QED = 0.7377

I fitted a quadratic polynomial (degree=2) through these points to model the trend, which showed an initial increase in QED followed by a decrease as the chain lengthens. Using this model to extrapolate to chain length 11 (m05), I predicted the QED to be 0.5416.

## Job 2 - Finding a Lead Compound

I screened for molecules with psa ≥ 40, which gave me 18 candidates.
Then I eliminated those in the same series as m05 (m15, m24, m28, m17, m22).

I then measured the QED of the remaining candidates until I found one that had QED ≥ 0.6:
- m01: QED = 0.3187 (< 0.6) - failed
- m02: QED = 0.6046 (≥ 0.6) - passed!
- m03: QED = 0.3282 (< 0.6) - failed
- m06: QED = 0.4011 (< 0.6) - failed
- m14: QED = 0.3237 (< 0.6) - failed
- m27: QED = 0.3330 (< 0.6) - failed
- m10: QED = 0.3724 (< 0.6) - failed

m02 met all three criteria:
1. Measured QED ≥ 0.6 (actual: 0.6046)
2. psa ≥ 40 (actual: 72.24) 
3. Not in the same series as m05 (different scaffold: O=[N+]([O-])c1ccc(cc1)C(=O)N)

Therefore, m02 is the selected lead compound.