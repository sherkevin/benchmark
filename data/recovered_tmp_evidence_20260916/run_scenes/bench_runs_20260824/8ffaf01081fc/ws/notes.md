# Task Notes

## Job 1: Predict QED for m05

Target molecule m05: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`

This is an N-alkyl chain attached to an amide group. The chain length is 12 carbons (CCCCCCCCCCCC).

Congener series identified (same scaffold `COc1ccc(cc1)C(=O)N`):
- m15: C2 chain (`NCC`)
- m24: C3 chain (`NCCC`)  
- m28: C4 chain (`NCCCC`)
- m17: C5 chain (`NCCCCC`)
- m22: C6 chain (`NCCCCCC`)

All are short-chain members (C2-C6), while m05 is C12 (extrapolation needed).

Chain lengths (number of carbons in alkyl chain):
- m15: 2 carbons → QED = 0.7621
- m24: 3 carbons → QED = 0.7919  
- m28: 4 carbons → QED = 0.7518
- m17: 5 carbons → QED = 0.7498
- m22: 6 carbons → QED = 0.7377
- m05: 12 carbons (target) → Need to predict

The data shows QED peaks at chain length 3 (0.7919) and then decreases with longer chains. Need to fit trend and extrapolate to chain length 12.

Trend fitting:
- Quadratic fit (degree=2) using all 5 points: predicted QED = 0.4836 (RMSE=0.01188)
- Linear fit (degree=1) using all 5 points: predicted QED = 0.6859 (RMSE=0.01308)
- Quadratic has lower RMSE and captures the peak-then-decline pattern
- Quadratic prediction (0.4836) selected as target_qed

## Job 2: Pick backup lead compound

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free descriptor)
3. not in same series as m05

Candidates from screen_candidates() with psa ≥ 40: m01, m02, m03, m06, m07, m09, m10, m12, m14, m18, m19, m21, m25, m27, m29, m31, m32, m33

Need to exclude those in same series as m05: m15, m24, m28, m17, m22 are NOT in the psa ≥ 40 list (all have psa=38.33), so all 18 candidates are potentially valid for constraint 3.

Measured:
- m01: QED = 0.3187 < 0.6 (not suitable)
- m02: QED = 0.6046 ≥ 0.6 ✓

m02 satisfies all constraints:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓ (from screen_candidates)
3. Not in same series as m05: m02 is nitrobenzamide (`O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`) while m05 is methoxybenzamide ✓

Selected lead: m02