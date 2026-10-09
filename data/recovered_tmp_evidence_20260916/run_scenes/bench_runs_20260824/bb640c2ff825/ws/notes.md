## Analysis Notes

### Job 1: Predicting QED of m05

Molecule m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) belongs to a congeneric series with the same scaffold (methoxyphenyl-C(=O)-N-alkyl) but varying alkyl chain lengths.

Congener molecules measured:
- m15: COc1ccc(cc1)C(=O)NCC (2 carbon chain) - QED = 0.7621
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbon chain) - QED = 0.7919
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbon chain) - QED = 0.7518
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbon chain) - QED = 0.7498
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbon chain) - QED = 0.7377

Trend analysis:
- QED peaks at 3-carbon chain length (0.7919)
- After peaking, QED decreases with increasing chain length
- From 3C to 6C: decrease of 0.0542 over 3 carbon units ≈ 0.018 per carbon
- Extrapolating to 11C (m05): 0.7919 - (8 * 0.018) = 0.6479

Therefore, predicted QED for m05: 0.6479

### Job 2: Finding a Lead Compound

Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

From the data, I identified molecules with PSA ≥ 40 that are not in the methoxyphenyl series:
- m01, m02, m03, m07, m14, m18, m19, m21, m25

Of these, I measured the QED values and found:
- m01: QED = 0.3187 (too low)
- m02: QED = 0.6046 (meets all criteria!)
- m03: QED = 0.3282 (too low)
- m07: QED = 0.3233 (too low)
- m14: QED = 0.3237 (too low)
- m18: QED = 0.3282 (too low)
- m19: QED = 0.3244 (too low)

m02 meets all three requirements:
1. QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Not in the same series as m05 (nitrobenzene vs methoxyphenyl) ✓

Therefore, m02 is selected as the lead compound.