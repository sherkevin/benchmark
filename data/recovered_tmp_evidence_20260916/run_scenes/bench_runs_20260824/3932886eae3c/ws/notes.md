# Analysis Notes

## Step 1: Identify congeneric series

Based on SMILES patterns:

### Series A: Methoxybenzamide (m05's series)
- Scaffold: `COc1ccc(cc1)C(=O)N` followed by alkyl chain
- Members: m05 (target), m15, m17, m22, m24, m28
- Target m05 has 12-carbon chain: `CCCCCCCCCCCC` (12 carbons after N)

### Series B: Nitrobenzamide
- Scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N`
- Members: m02, m06, m09, m10, m12, m29, m31, m32, m33

### Series C: Biphenylamide
- Scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N`
- Members: m04, m08, m11, m13, m16, m20, m23, m26, m30

### Series D: Polyol amide
- Scaffold: `OCC(O)C(O)C(O)C(=O)N`
- Members: m01, m03, m07, m14, m18, m19, m21, m25, m27

## Step 2: Determine chain lengths for m05's series

I need to count alkyl carbons in each methoxybenzamide:
- m15: NCC (2 carbons after N)
- m24: NCCC (3 carbons after N)
- m28: NCCCC (4 carbons after N)
- m17: NCCCCC (5 carbons after N)
- m22: NCCCCCC (6 carbons after N)
- m05: NCCCCCCCCCCCC (12 carbons after N)

So we have measurements at chain lengths: 2, 3, (4), 5, 6... need to extrapolate to 12.

## Step 3: Measured QED values for m05's series

Chain lengths and measured QED:
- m15: 2 carbons, QED = 0.7621
- m24: 3 carbons, QED = 0.7919
- m28: 4 carbons, QED = 0.7518
- m17: 5 carbons, QED = 0.7498
- m22: 6 carbons, QED = 0.7377

Target m05: 12 carbons (extrapolation needed)

Observations: QED appears to peak around 3 carbons (0.7919) and then decreases gradually.

## Step 4: Fit trend for extrapolation

Using fit_trend with chain lengths as x-values and QED as y-values to predict QED for chain length 12.

Chain lengths (x): [2, 3, 4, 5, 6]
QED values (y): [0.7621, 0.7919, 0.7518, 0.7498, 0.7377]

Predictions for m05 (chain length 12):
- Quadratic fit (degree=2): QED = 0.4836 (RMSE=0.01188)
- Linear fit (degree=1): QED = 0.6859 (RMSE=0.01308)

Decision: Use quadratic fit (0.4836) because:
1. Lower RMSE (0.01188 vs 0.01308)
2. Captures the peak-and-decline trend better
3. Biological properties often show optimal chain length followed by deterioration
4. Extrapolating from 6 to 12 carbons is a big jump - quadratic suggests stronger decline which seems plausible

## Step 5: Job 2 - Find backup lead

### Screening for PSA ≥ 40
Used screen_candidates(): 18 molecules pass PSA ≥ 40:
- Polyol amides (PSA=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrobenzamides (PSA=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Series constraint
None of the methoxybenzamides (m05's series) have PSA ≥ 40 (all have PSA=38.33), so all 18 candidates satisfy "not in m05's series".

### Measuring QED
Used remaining 7 measurements to test candidates:

1. m01 (polyol): QED = 0.3187 ✗
2. m02 (nitro): QED = 0.6046 ✓ (meets all criteria!)
3. m06 (nitro): QED = 0.4011 ✗
4. m09 (nitro): QED = 0.5826 ✗
5. m10 (nitro): QED = 0.3724 ✗
6. m12 (nitro): QED = 0.4709 ✗
7. m14 (polyol): QED = 0.3237 ✗

### Selected lead: m02
- QED = 0.6046 (≥ 0.6 ✓)
- PSA = 72.24 (≥ 40 ✓)
- Not in m05's series ✓ (nitrobenzamide vs methoxybenzamide)

All 12 measurements used.