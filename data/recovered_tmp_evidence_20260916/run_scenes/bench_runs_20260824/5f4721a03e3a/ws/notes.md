# Notes

## Task 1: QED for m05
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (13-carbon alkyl chain)
- Has never been synthesized, returns probe_unavailable
- Need to predict QED from congeneric series

Congeneric series measurements (scaffold: `COc1ccc(cc1)C(=O)N-`):
- m15 (C2): QED = 0.7621
- m24 (C3): QED = 0.7919
- m28 (C4): QED = 0.7518
- m17 (C5): QED = 0.7498
- m22 (C6): QED = 0.7377

Trend analysis:
- QED peaks at C3 (0.7919)
- Then decreases with increasing chain length
- From C3 to C6: decrease of ~0.054 over 3 carbons
- Extrapolating to C13: significant decrease expected due to long alkyl chain

Prediction for m05 (C13): The long alkyl chain will significantly reduce drug-likeness.
Based on the downward trend and typical QED penalties for high lipophilicity and molecular weight,
I estimate m05 QED ≈ 0.52

## Task 2: Find molecule with QED ≥ 0.6, psa ≥ 40, not in m05 series

Measurements:
- m01: QED = 0.3187, psa = 110.02 → QED too low
- m02: QED = 0.6046, psa = 72.24 → QUALIFIES! (QED ≥ 0.6, psa ≥ 40, different scaffold)
- m03: QED = 0.3282, psa = 110.02 → QED too low
- m09: QED = 0.5826, psa = 72.24 → QED too low

Selected lead: m02
- QED = 0.6046 (measured)
- psa = 72.24 (≥ 40)
- Scaffold: nitro-phenyl amide (different from m05's methoxy-phenyl amide)

## Budget used: 9 measurements (5 for congeneric series + 4 for Task 2 candidates)
