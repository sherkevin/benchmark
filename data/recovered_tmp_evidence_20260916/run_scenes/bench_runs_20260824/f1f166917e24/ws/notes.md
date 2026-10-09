# Task Notes

Starting the medicinal chemistry task. Will document findings here as I work through the problem.

## Compound Pool Analysis
- Found 33 compounds total
- Identified congeneric series for m05 (methoxyphenyl-carbonyl-amide with varying N-alkyl chain lengths):
  - m15: COc1ccc(cc1)C(=O)NCC (chain length: 2 carbons)
  - m24: COc1ccc(cc1)C(=O)NCCC (chain length: 3 carbons)
  - m28: COc1ccc(cc1)C(=O)NCCCC (chain length: 4 carbons)
  - m17: COc1ccc(cc1)C(=O)NCCCCC (chain length: 5 carbons)
  - m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length: 6 carbons)
  - m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain length: 11 carbons)

## Measurements Made
- m05: probe_unavailable (has not been synthesized)
- m15: QED = 0.7621
- m24: QED = 0.7919

## Task 1 - Predicting QED for m05
m05 could not be measured directly. Based on the trend from m15 (QED 0.7621) and m24 (QED 0.7919), there was an increase from chain length 2 to 3. However, m05 has a much longer chain (11 carbons), which typically decreases drug-likeness due to increased hydrophobicity. I estimate the QED for m05 to be approximately 0.65.

## Task 2 - Finding a suitable lead compound
Compounds with PSA ≥ 40:
- Sugar-like compounds (PSA = 110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrobenzene compounds (PSA = 72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

None of these are in the same congeneric series as m05. Unfortunately, I ran out of budget to measure their QED values. I cannot definitively identify which one has QED ≥ 0.6.

## Budget Usage
- Pages viewed: 9 (9 units)
- Successful measurements: 2 (m15, m24 - 2 units)  
- Failed measurement (m05): 0 units
- Remaining budget: 1 unit, but attempted measurement of m28 consumed it without returning a value
- Total: 12 units (exhausted)