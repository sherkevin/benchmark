# Task Notes

## Part 1: Predict QED of m05

### Identifying Congeners of m05
- m05 has scaffold "COc1ccc(cc1)C(=O)N" and chain length 11
- Identified congeners with same scaffold but different chain lengths:
  - m15: chain length 2 (COc1ccc(cc1)C(=O)NCC)
  - m24: chain length 3 (COc1ccc(cc1)C(=O)NCCC)
  - m28: chain length 4 (COc1ccc(cc1)C(=O)NCCCC)
  - m17: chain length 5 (COc1ccc(cc1)C(=O)NCCCCC)
  - m22: chain length 6 (COc1ccc(cc1)C(=O)NCCCCCC)

### Measuring QED of Congeners
- m15: chain length 2, QED = 0.7621
- m24: chain length 3, QED = 0.7919
- m28: chain length 4, QED = 0.7518
- m17: chain length 5, QED = 0.7498
- m22: chain length 6, QED = 0.7377

### Trend Analysis and Prediction
Using fit_trend with polynomial degree 2:
- xs = [2, 3, 4, 5, 6] (chain lengths)
- ys = [0.7621, 0.7919, 0.7518, 0.7498, 0.7377] (QED values)
- Query point: x_query = 11 (for m05)
- Result: predicted QED = 0.5416

The QED decreases as the chain length increases beyond ~3 carbons.

## Part 2: Finding Lead Compound

### Strategy
First, I used screen_candidates() to find molecules with PSA ≥ 40.
Then I identified which of these were NOT in the same series as m05.
Finally, I planned to measure QED for promising candidates.

### Compounds with PSA ≥ 40 (from screen_candidates):
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

### Series Membership Check
I determined which compounds have different scaffolds from m05:
- Sugar-like compounds: m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrobenzene compounds: m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurement Results
I measured QED for compounds with different scaffolds and PSA ≥ 40:
- m01: QED = 0.3187 (too low)
- m03: QED = 0.3282 (too low)
- m14: QED = 0.3237 (too low)

Unfortunately, I exhausted my measurement budget before testing any of the nitrobenzene compounds, which were likely better candidates. Based on structural diversity and typical properties of nitrobenzene compounds in drug design, I selected m02 as the most likely candidate to meet all criteria, though I could not verify its QED experimentally.