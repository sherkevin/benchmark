# Notes: QED Prediction and Lead Selection

## Congeneric Series Identification

**m05 scaffold**: `COc1ccc(cc1)C(=O)N-` (methoxybenzamide) with N-alkyl chain of 11 carbons.

### Same series as m05 (methoxybenzamide, varying N-alkyl chain):
| mol_id | SMILES | Chain Length | alogp |
|--------|--------|-------------|-------|
| m15 | COc1ccc(cc1)C(=O)NCC | C2 | 1.445 |
| m24 | COc1ccc(cc1)C(=O)NCCC | C3 | 1.835 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | C4 | 2.225 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | C5 | 2.615 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | C6 | 3.005 |
| m05 (target) | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | C11 | 4.956 |

### Other series identified:
- **Sugar polyol** (`OCC(O)C(O)C(O)C(=O)N-`): m14, m27, m01, m19, m03, m21, m18, m07, m25
- **Nitrobenzene** (`O=[N+]([O-])c1ccc(cc1)C(=O)N-`): m09, m02, m12, m32, m33, m29, m31, m06, m10
- **Biphenyl** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): m13, m11, m04, m30, m23, m20, m26, m08, m16

---

## Job 1: QED Prediction for m05

### Measured QED values (methoxybenzamide series):
| mol_id | Chain | QED |
|--------|-------|-----|
| m15 | C2 | 0.7621 |
| m24 | C3 | 0.7919 |
| m28 | C4 | 0.7518 |
| m17 | C5 | 0.7498 |
| m22 | C6 | 0.7377 |

### Observed trend:
- QED peaks at C3 (0.7919), then declines steadily.
- C3 to C4: sharp drop of -0.040
- C4 to C5 to C6: gradual decline of -0.002 then -0.012
- Overall C3 to C6 decline: -0.054 over 3 carbons

### Model fitted: Exponential decay (all 5 points)
Using least-squares regression on ln(QED) vs chain length:
- ln(QED) = -0.2286 - 0.0120 * chain
- QED = 0.7957 * 0.9881^chain

### Extrapolation to C11:
QED(C11) = 0.7957 * 0.9881^11 = 0.7957 * 0.8766 = 0.697

### Additional models considered:
- Linear fit (C3-C6): slope = -0.018/carbon, QED(C11) ~ 0.65
- Exponential (C4-C6 only): QED(C11) ~ 0.70
- Quadratic (all 5 pts): QED(C11) ~ 0.54 (overfits noise)

### Final prediction:
Taking the exponential model (0.70) and applying a small downward correction (-0.03) for expected acceleration of logP penalty at logP ~5.0:

**Predicted QED for m05 = 0.67**

---

## Job 2: Lead Compound Selection

### Constraints:
1. Measured QED >= 0.6
2. PSA >= 40
3. Not in m05 series

### Pre-filtering (free checks):
- Biphenyl series: PSA = 29.1 < 40, eliminated entirely
- Sugar polyol series: PSA = 110.02 >= 40, candidates
- Nitrobenzene series: PSA = 72.24 >= 40, candidates

### Measurements for lead selection (7 remaining budget):
| mol_id | Series | Chain | QED | Pass? |
|--------|--------|-------|-----|-------|
| m09 | Nitro | C2 | 0.5826 | No (QED < 0.6) |
| m02 | Nitro | C3 | 0.6046 | Yes |
| m12 | Nitro | C4 | 0.4709 | No |
| m32 | Nitro | C5 | 0.4683 | No |
| m14 | Sugar | C2 | 0.3237 | No |
| m27 | Sugar | C3 | 0.3330 | No |
| m01 | Sugar | C4 | 0.3187 | No |

### Selected lead: m02
- SMILES: O=[N+]([O-])c1ccc(cc1)C(=O)NCCC
- Measured QED: 0.6046 (>= 0.6)
- PSA: 72.24 (>= 40)
- Series: Nitrobenzene (not m05 series)

All three constraints satisfied.

## Budget Summary
- 5 measurements: m05 series (m15, m24, m28, m17, m22)
- 7 measurements: lead candidates (m09, m02, m12, m32, m14, m27, m01)
- Total: 12/12
