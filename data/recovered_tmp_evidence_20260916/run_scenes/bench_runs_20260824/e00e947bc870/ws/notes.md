# Analysis Notes

## Job 1: Predicting QED for m05

### Identifying Congeneric Series for m05
The target molecule m05 has SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC
It has a methoxyphenyl group attached to a carbonyl, then an amide nitrogen connected to an 11-carbon chain.

Looking for molecules with the same scaffold (methoxyphenyl-C(=O)-N-) but different chain lengths:
- m15: COc1ccc(cc1)C(=O)NCC (chain length: 2 C after N) - QED: 0.7621
- m24: COc1ccc(cc1)C(=O)NCCC (chain length: 3 C after N) - QED: 0.7919
- m28: COc1ccc(cc1)C(=O)NCCCC (chain length: 4 C after N) - QED: 0.7518
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain length: 5 C after N) - QED: 0.7498
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain length: 6 C after N) - QED: 0.7377
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain length: 11 C after N) - target, not measurable

So the congener series consists of: m15, m24, m28, m17, m22, and m05 (target).

### Fitting Trend for QED Prediction
Chain lengths (x): [2, 3, 4, 5, 6]
QED values (y): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

I'll use fit_trend to predict the QED for chain length 11.

Predicted QED for m05 (chain length 11): 0.5416

## Job 2: Finding a Lead Compound

### Screening Candidates
Using screen_candidates() to find molecules with psa ≥ 40, resulting in 18 candidates.

### Eliminating Molecules from Same Series as m05
Molecules with the same scaffold as m05 (methoxyphenyl-C(=O)-N-) were excluded:
- m15, m24, m28, m17, m22, m05

### Testing QED Values for Remaining Candidates
- m01: QED = 0.3187 (< 0.6, doesn't meet criteria)
- m02: QED = 0.6046 (≥ 0.6, meets criteria!)

### Final Lead Selection
m02 meets all three criteria:
1. measured QED ≥ 0.6: 0.6046 ≥ 0.6 ✓
2. psa ≥ 40: 72.24 ≥ 40 ✓  
3. not in the same series as m05: Has nitrophenyl group instead of methoxyphenyl ✓

Therefore, m02 is the selected lead compound.