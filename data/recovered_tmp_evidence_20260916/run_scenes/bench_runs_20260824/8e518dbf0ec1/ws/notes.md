# Analysis Notes

## Task Overview
- Job 1: Predict QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - this molecule was never synthesized
- Job 2: Pick a backup lead compound that meets all three constraints:
  1. Measured QED ≥ 0.6
  2. PSA ≥ 40
  3. Not in the same series as m05

## Step 1: Identify the congeneric series for m05
Looking at m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (methoxyphenyl group with amide and 11-carbon chain)

The general structure is: methoxyphenyl-C(=O)-NH-(alkyl chain with 11 carbons)
Let me identify other molecules with similar scaffolds (same core, different chain lengths):

- m15: COc1ccc(cc1)C(=O)NCC (chain with 2 carbons) 
- m24: COc1ccc(cc1)C(=O)NCCC (chain with 3 carbons)
- m28: COc1ccc(cc1)C(=O)NCCCC (chain with 4 carbons)
- m17: COc1ccc(cc1)C(=O)NCCCCC (chain with 5 carbons)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (chain with 6 carbons)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (chain with 11 carbons)

So the congeners of m05 appear to be: m15, m24, m28, m17, m22, m05

## Step 2: Measure QED values for the series containing m05
- m15: QED = 0.7621
- m24: QED = 0.7919
- m28: QED = 0.7518
- m17: QED = 0.7498
- m22: QED = 0.7377
- m05: probe_unavailable (as expected)

Chain lengths and corresponding QED values:
- Chain length 2 (m15): QED = 0.7621
- Chain length 3 (m24): QED = 0.7919
- Chain length 4 (m28): QED = 0.7518
- Chain length 5 (m17): QED = 0.7498
- Chain length 6 (m22): QED = 0.7377

Using fit_trend with xs=[2,3,4,5,6], ys=[0.7621,0.7919,0.7518,0.7498,0.7377], predicting for x_query=11:
Predicted QED for m05 = 0.5416

## Step 3: Find a lead compound satisfying all three constraints

Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the same series as m05

First, I used screen_candidates() to get molecules with PSA ≥ 40:
m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

Then I eliminated those in the same series as m05 (COc1ccc(cc1)C(=O)N...):
- m15, m24, m28, m17, m22, m05 (removed)

Remaining candidates with PSA ≥ 40 and not in m05 series:
Carbohydrate amides: m01, m03, m07, m14, m18, m19, m21, m25, m27
Nitrophenyl amides: m02, m06, m09, m10, m12, m29, m31, m32, m33

I measured the following with QED results:
- m01: QED = 0.3187 (< 0.6)
- m03: QED = 0.3282 (< 0.6)
- m06: QED = 0.4011 (< 0.6)
- m10: QED = 0.3724 (< 0.6)
- m12: QED = 0.4709 (< 0.6)
- m07: QED = 0.3233 (< 0.6)
- m31: QED = 0.4252 (< 0.6)

I ran out of my measurement budget (12 total) and did not measure:
- m02, m09, m29, m32, m33 (nitrophenyl amides)
- m14, m18, m19, m21, m25, m27 (carbohydrate amides)

Based on the pattern observed, the carbohydrate amides I measured all had low QED values (~0.32), so the others likely also have low QED. Among the nitrophenyl amides I measured, the highest was m12 with QED = 0.4709.

Among the unmeasured nitrophenyl amides, m09 has the shortest chain (NCC) and m02 has NCCC. Based on trends in similar series, shorter chains sometimes have higher QED values. If forced to make a selection with incomplete data, m09 would be my best guess, but I cannot confirm its QED is ≥ 0.6 without measurement.

However, since I must provide an answer and I've run out of measurements, I'll select the molecule that I believe most likely meets all criteria based on structural reasoning. Since I can't measure further, I'll select m09 tentatively, though I haven't confirmed its QED.

Actually, since I cannot validate the QED of any candidate, I need to acknowledge that with the available data I could not identify a compound that meets all three criteria. However, I'll go with the compound that structurally seems most likely to have high QED among those I didn't measure: m09.