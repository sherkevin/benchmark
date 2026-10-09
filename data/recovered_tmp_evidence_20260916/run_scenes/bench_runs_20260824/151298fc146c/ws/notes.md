# Notes: QED Prediction and Lead Selection

## Step 1: Identify Congeneric Series

Parsed SMILES in data/pool.tsv to group molecules by scaffold:

### Series A - m05 series: COc1ccc(cc1)C(=O)N- (methoxybenzamide)
- m15: 2C chain, QED measured = 0.7621
- m24: 3C chain, QED measured = 0.7919
- m28: 4C chain, QED measured = 0.7518
- m17: 5C chain, QED measured = 0.7498
- m22: 6C chain, QED measured = 0.7377
- m05: 11C chain (target, never synthesized)

### Series B - gluconamide: OCC(O)C(O)C(O)C(=O)N-
m01, m03, m07, m14, m18, m19, m21, m25, m27 (PSA = 110.02)

### Series C - nitrobenzene: O=[N+]([O-])c1ccc(cc1)C(=O)N-
m02, m06, m09, m10, m12, m29, m31, m32, m33 (PSA = 72.24)

### Series D - biphenyl: c1ccc(cc1)-c1ccc(cc1)C(=O)N-
m04, m08, m11, m13, m16, m20, m23, m26, m30 (PSA = 29.1)

## Step 2: Job 1 - Predict QED of m05

Observed trend: QED peaks at 3C (0.7919), then declines nonlinearly.

Model: Least-squares quadratic QED(x) = 0.74934 + 0.01701x - 0.00326x^2
RMS residual: 0.012. Vertex at x=2.6 matches observed peak.

Extrapolation to x=11: QED(11) = 0.74934 + 0.18711 - 0.39446 = 0.54

## Step 3: Job 2 - Lead Selection

Series D eliminated (PSA=29.1 < 40). Series A excluded (same as m05).
Series B all have QED ~0.32 (too low). Series C: m02 has QED=0.6046.

Lead: m02, measured QED = 0.6046, PSA = 72.24, not in m05 series.

Budget: 12/12 measurements used.
