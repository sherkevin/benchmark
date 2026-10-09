# Medicinal Chemistry Analysis Notes

## Catalogue Overview
33 molecules across 4 congeneric series:
1. **Sugar** (OCC(O)C(O)C(O)C(=O)N-): m14, m27, m01, m19, m03, m21, m18, m07, m25 — psa=110.02
2. **Nitrobenzamide** (O=[N+]([O-])c1ccc(cc1)C(=O)N-): m09, m02, m12, m32, m33, m29, m31, m06, m10 — psa=72.24
3. **Biphenyl** (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): m13, m11, m04, m30, m23, m20, m26, m08, m16 — psa=29.1
4. **Methoxybenzamide** (COc1ccc(cc1)C(=O)N-): m15, m24, m28, m17, m22, m05 — psa=38.33

## Task 1: QED Prediction for m05

m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (N-undecyl methoxybenzamide, C11 chain)

Measured QEDs in congeneric series:
| mol_id | Chain | QED    |
|--------|-------|--------|
| m15    | C2    | 0.7621 |
| m24    | C3    | 0.7919 |
| m28    | C4    | 0.7518 |
| m17    | C5    | 0.7498 |
| m22    | C6    | 0.7377 |

Linear regression on all 5 points: slope = -0.00909, intercept = 0.795
Predicted QED at C11: 0.795 - 0.00909 * 11 = 0.695 ~ 0.70

Rationale: QED decreases gradually with chain length due to increasing ALogP (4.956) and rotatable bonds (12).

## Task 2: Lead Selection

Constraints: QED >= 0.6, psa >= 40, not in m05 series.

Biphenyl series excluded (psa = 29.1 < 40). Methoxybenzamide series excluded (same as m05, and psa = 38.33 < 40).

Measured candidates:
- m02 (nitro, C3): QED = 0.6046, psa = 72.24 -> Selected as lead
- m12 (nitro, C4): QED = 0.4709 — too low
- m32 (nitro, C5): QED = 0.4683 — too low

Lead: m02 — QED 0.6046, psa 72.24, from nitrobenzamide series (different scaffold from m05).

## Budget Usage
- 9 pages listed (9 units)
- 8 QED measurements: m15, m24, m28, m17, m22, m02, m12, m32 (8 units)
- m05 measurement returned probe_unavailable (0 cost)
- Total: 17/17 units used
