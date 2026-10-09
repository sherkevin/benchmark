# Notes for Medicinal Chemistry Task

## Initial Analysis

Target molecule m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxy-phenyl-C(=O)N- (amide with methoxy benzene)
- Chain length: 11 carbons in alkyl chain (NCCCCCCCCCCC = N-C11)

Pool contains 33 molecules. After scanning all pages (1-9), I've identified several congeneric series:

1. **Methoxy-phenyl series** (same as m05):
   - m15: `COc1ccc(cc1)C(=O)NCC` (2 Cs)
   - m24: `COc1ccc(cc1)C(=O)NCCC` (3 Cs)
   - m28: `COc1ccc(cc1)C(=O)NCCCC` (4 Cs)
   - m17: `COc1ccc(cc1)C(=O)NCCCCC` (5 Cs)
   - m22: `COc1ccc(cc1)C(=O)NCCCCCC` (6 Cs)
   - m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (11 Cs) - target, unsynthesized

2. **Nitro-phenyl series**: `O=[N+]([O-])c1ccc(cc1)C(=O)N...`
   - m09: NCC (2 Cs)
   - m12: NCCCC (4 Cs)
   - m32: NCCCCC (5 Cs)
   - m33: NCCCCCC (6 Cs)
   - m29: NCCCCCCC (7 Cs)
   - m31: NCCCCCCCC (8 Cs)
   - m06: NCCCCCCCCC (9 Cs)
   - m10: NCCCCCCCCCC (10 Cs)
   - m02: NCCC (3 Cs)

3. **Biphenyl series**: `c1ccc(cc1)-c1ccc(cc1)C(=O)N...`
   - m13: NCC (2 Cs)
   - m11: NCCC (3 Cs)
   - m04: NCCCC (4 Cs)
   - m30: NCCCCC (5 Cs)
   - m23: NCCCCCC (6 Cs)
   - m20: NCCCCCCC (7 Cs)
   - m26: NCCCCCCCC (8 Cs)
   - m08: NCCCCCCCC (8 Cs?) Wait check: m08 has NCCCCCCCC (8 Cs)
   - m16: NCCCCCCCCCC (10 Cs)

4. **Polyol series**: `OCC(O)C(O)C(O)C(=O)N...` (no aromatic)
   - m14: NCC (2 Cs)
   - m27: NCCC (3 Cs)
   - m01: NCCCC (4 Cs)
   - m19: NCCCCC (5 Cs)
   - m21: NCCCCCCC (7 Cs)
   - m03: NCCCCCC (6 Cs)
   - m18: NCCCCCCCC (8 Cs)
   - m07: NCCCCCCCCC (9 Cs)
   - m25: NCCCCCCCCCC (10 Cs)

## Job 1: Predict QED for m05

I need to:
1. Identify all molecules in same series as m05 (methoxy-phenyl)
2. Measure their QED values
3. Fit a trend (polynomial) to chain length vs QED
4. Extrapolate to chain length 11 (m05)

Chain lengths for methoxy-phenyl series:
- m15: NCC = 2 Cs
- m24: NCCC = ГѓВўГўв‚¬вЂќ wait, actually: NCC = N-C-C = 2 carbons total? Actually N attached to C, then C: that's 2 carbons? Let me define chain length properly:
  SMILES: C(=O)NCC means amide N attached to CH2-CH3? Actually NCC = N-C-C, so 2 carbons in alkyl chain.
  Similarly NCCC = N-C-C-C = 3 carbons.
  So chain length = number of carbons in the N-alkyl chain.

Let me define:
- m15: NCC = 2 Cs (ethyl)
- m24: NCCC = 3 Cs (propyl)
- m28: NCCCC = 4 Cs (butyl)
- m17: NCCCCC = 5 Cs (pentyl)
- m22: NCCCCCC = 6 Cs (hexyl)
- m05: NCCCCCCCCCCC = 11 Cs (undecyl)

I measured QED for methoxy-phenyl series:
### Data Points (chain length vs QED)
- m15: chain length 2, QED = 0.7621
- m24: chain length 3, QED = 0.7919
- m28: chain length 4, QED = 0.7518
- m17: chain length 5, QED = 0.7498
- m22: chain length 6, QED = 0.7377

### Trend Analysis
Quadratic fit (degree=2): prediction for chain length 11 (m05) = 0.5416
Coefficients: 0.74932 + 0.017024*x - 0.003264*x²
RMSE = 0.01188

Linear fit (degree=1): prediction = 0.695
Coefficients: 0.79502 - 0.00909*x
RMSE = 0.01308

Quadratic has slightly lower RMSE and captures the peak at chain length 3 followed by decline. Biologically, long alkyl chains often reduce drug-likeness. I'll use quadratic prediction: 0.5416.

Budget: Used 9 on list_pool + 5 measurements = 14 used, leaving 3 for Job 2.

## Job 2: Find backup lead compound

Constraints:
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free)
3. Not in same series as m05 (free)

Need to check PSA constraint first (free via list_pool or screen_candidates).
Then check series membership (free, by SMILES inspection).
Then measure QED for promising candidates.

Let me start by using screen_candidates() to see which molecules have PSA ≥ 40.

### Job 2 Results

Used screen_candidates(): 18 molecules pass PSA ≥ 40. None are in m05's series (methoxy-phenyl series all have PSA=38.33 < 40).

Tested candidates:
- m01 (polyol series): QED = 0.3187 < 0.6
- m02 (nitro-phenyl series): QED = 0.6046 ≥ 0.6 ✓
- m09 (nitro-phenyl series): QED = 0.5826 < 0.6

Chosen lead: m02
- QED measured: 0.6046
- PSA: 72.24 ≥ 40 ✓
- Series: nitro-phenyl (not methoxy-phenyl) ✓

Budget summary:
- list_pool: 9 measurements
- measure_qed for Job 1: 5 measurements (m15, m24, m28, m17, m22)
- measure_qed for Job 2: 3 measurements (m01, m02, m09)
- Total: 17/17 measurements used