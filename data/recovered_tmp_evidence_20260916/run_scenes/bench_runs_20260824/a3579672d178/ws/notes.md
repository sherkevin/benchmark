# Task Notes

## Analysis of Congeneric Series

### Methoxyphenyl Series (Same as m05):
- m15: COc1ccc(cc1)C(=O)NCC (2 C after N)
- m24: COc1ccc(cc1)C(=O)NCCC (3 C after N)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 C after N)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 C after N)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 C after N)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11 C after N) - target to predict

### Other Series:
- Nitrophenyl series: m02, m06, m09, m10, m12, m29, m31, m32, m33
- Biphenyl series: m04, m08, m11, m13, m16, m20, m23, m26, m30
- Sugar-based series: m01, m03, m07, m14, m18, m19, m21, m25, m27

## Job 1 - Predict QED for m05
Measured QED values for the methoxyphenyl series:
- m15 (2 C after N): QED = 0.7621
- m24 (3 C after N): QED = 0.7919
- m28 (4 C after N): QED = 0.7518
- m17 (5 C after N): QED = 0.7498
- m22 (6 C after N): QED = 0.7377

Trend Analysis: The QED initially increases from 2C (0.7621) to 3C (0.7919), reaching a peak, then decreases consistently as the chain lengthens: 4C (0.7518), 5C (0.7498), 6C (0.7377). The rate of decrease appears to slow down slightly as the chain gets longer. Extrapolating this trend to 11C, the QED should continue to decrease gradually. My prediction for m05 (11C) is approximately 0.69.

## Job 2 - Find Lead Compound
Molecules with PSA ≥ 40:
- m01, m03, m07, m14, m18, m19, m21, m25, m27 (sugar-based series, all had low QED < 0.6)
- m02, m06, m09, m10, m12, m29, m31, m32, m33 (nitrophenyl series)

Measured QED values:
- m01: QED = 0.3187 (< 0.6, disqualified)
- m03: QED = 0.3282 (< 0.6, disqualified)
- m14: QED = 0.3237 (< 0.6, disqualified)
- m02: QED = 0.6046 (≥ 0.6, qualifies!)
- m12: QED = 0.4709 (< 0.6, disqualified)
- m09: QED = 0.5826 (< 0.6, disqualified)
- m32: QED = 0.4683 (< 0.6, disqualified)

m02 meets all three criteria:
1. Measured QED ≥ 0.6 (QED = 0.6046)
2. PSA ≥ 40 (PSA = 72.24)
3. Not in the same series as m05 (nitrophenyl vs methoxyphenyl)

Budget: Used all 12 measurements