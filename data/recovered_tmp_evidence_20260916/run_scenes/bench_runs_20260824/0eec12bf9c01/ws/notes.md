# Notes

## Task Overview
- Predict QED of m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC, 11-carbon N-alkyl chain)
- Pick one molecule with QED ≥ 0.6, PSA ≥ 40, not in m05's congeneric series

## Congeneric Series Identification
Four series identified by scaffold:
1. **Methoxybenzamide** (COc1ccc(cc1)C(=O)N-): m05, m15, m17, m22, m24, m28 — PSA=38.33
2. **Nitrobenzene** (O=[N+]([O-])c1ccc(cc1)C(=O)N-): m02, m06, m09, m10, m12, m29, m31, m32, m33 — PSA=72.24
3. **Sugar-polyol** (OCC(O)C(O)C(O)C(=O)N-): m01, m03, m07, m14, m18, m19, m21, m25, m27 — PSA=110.02
4. **Biphenyl** (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): m04, m08, m11, m13, m16, m20, m23, m26, m30 — PSA=29.1

## Task 1: m05 QED Prediction

Measured all 5 available members of methoxybenzamide series:

| mol_id | N-alkyl C's | alogp | rotb | QED measured |
|--------|-------------|-------|------|-------------|
| m15    | 2           | 1.445 | 3    | 0.7621      |
| m24    | 3           | 1.835 | 4    | 0.7919      |
| m28    | 4           | 2.225 | 5    | 0.7518      |
| m17    | 5           | 2.615 | 6    | 0.7498      |
| m22    | 6           | 3.005 | 7    | 0.7377      |

m05 has 11 carbons, alogp=4.956, rotb=12.

Linear regression (all 5 points): QED(n) = 0.795 - 0.00909*n
→ QED(11) = 0.795 - 0.100 = **0.695**

Also computed:
- Logit model: 0.690
- Geometric mean desirability model: ~0.66-0.70
- C4-C6 rate extrapolation: 0.70

The QED peaks at C3 (alogp ≈ 1.8, near optimal) then declines. The decline is roughly linear in the measured range, but may accelerate slightly at high alogp/rotb. Estimate: **0.68** (slightly below linear to account for expected non-linearity at high alogp=4.96 and rotb=12).

## Task 2: Lead Selection

Constraints: QED ≥ 0.6, PSA ≥ 40, not methoxybenzamide series.

PSA ≥ 40 candidates: nitrobenzene (PSA=72.24) and sugar-polyol (PSA=110.02).
Biphenyl series has PSA=29.1 (excluded). Methoxybenzamide has PSA=38.33 (excluded, and same series as m05).

Sugar-polyol measured: m14 QED=0.3237, m01 QED=0.3187 — too low.

Nitrobenzene measurements:
- m09 (C2): QED = 0.5826 (< 0.6)
- m02 (C3): QED = 0.6046 (≥ 0.6) ✓
- m12 (C4): QED = 0.4709 (< 0.6)
- m32 (C5): QED = 0.4683 (< 0.6)

**m02 is the best candidate**: QED=0.6046, PSA=72.24, nitrobenzene scaffold (different from m05's methoxybenzamide).

## Budget Used
11 of 12 measurements used (m05 probe_unavailable = free):
m15, m24, m28, m17, m22, m09, m02, m12, m32, m14, m01

## Final Answers
- target_qed: 0.68 (predicted from congeneric series trend)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
