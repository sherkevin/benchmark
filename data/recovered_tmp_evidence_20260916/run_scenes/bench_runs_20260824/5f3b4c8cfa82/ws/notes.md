# Notes

## Strategy
1. Used `group_by_scaffold()` (free) to identify 4 scaffold groups
2. Listed pool pages 1-4 to see descriptors (4 budget)
3. Measured 5 members of m05's congeneric series (5 budget)
4. Measured 3 Task 2 candidates from groups 2 and 3 (3 budget)
5. Total: 12/12 budget used

## Scaffold Groups
- **Group 1** (COc1ccc(cc1)C(=O)N): m15(c2), m24(c3), m28(c4), m17(c5), m22(c6), m05(c11) — PSA=38.33
- **Group 2** (O=[N+]([O-])c1ccc(cc1)C(=O)N): m09(c2), m02(c3), m12(c4), m32(c5), m33(c6), m29(c7), m31(c8), m06(c9), m10(c10) — PSA=72.24
- **Group 3** (OCC(O)C(O)C(O)C(=O)N): m14(c2), m27(c3), m01(c4), m19(c5), m03(c6), m21(c7), m18(c8), m07(c9), m25(c10) — PSA=110.02
- **Group 4** (c1ccc(cc1)-c1ccc(cc1)C(=O)N): m13(c2), m11(c3), m04(c4), m30(c5), m23(c6), m20(c7), m26(c8), m08(c9), m16(c10) — PSA=29.1

## Task 1: m05 QED Prediction

m05 has scaffold "COc1ccc(cc1)C(=O)N" with chain_len=11.
Measured series members:
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

Quadratic fit (degree 2, all 5 points): prediction at chain 11 = **0.5416**
- Coefficients: [0.74932, 0.017024, -0.003264]
- RMSE on given points: 0.01188

Linear fit (all 5 points): prediction at chain 11 = 0.695
Linear fit (last 3 points): prediction at chain 11 = 0.704

The quadratic fit is preferred because:
- It captures the curvature (QED peaks at chain 3 then declines)
- m05's descriptors (MW=305.46, ALOGP=4.956, ROTB=12) suggest significant penalties for high lipophilicity and flexibility
- The declining trend from chain 3→6 accelerates slightly, consistent with quadratic

Component analysis for m05 (chain 11):
- MW score: ~0.7-1.0 (MW=305 is in good range)
- ALOGP score: expected to drop significantly (ALOGP=4.956 is very high)
- ROTB score: expected to drop to near 0 (ROTB=12 is very high)
- PSA, HBA, HBD: same as other group 1 members (good scores)

**Predicted m05 QED: 0.54**

## Task 2: Lead Molecule Selection

Constraints: QED ≥ 0.6, PSA ≥ 40, not in m05's group

PSA analysis by group:
- Group 1 (m05's group): PSA=38.33 < 40 → excluded (both constraints)
- Group 2: PSA=72.24 ≥ 40 ✓
- Group 3: PSA=110.02 ≥ 40 ✓  
- Group 4: PSA=29.1 < 40 → excluded

Measured candidates:
| mol_id | group | QED    | PSA     | QED≥0.6? |
|--------|-------|--------|---------|----------|
| m12    | 2     | 0.4709 | 72.24   | No       |
| m01    | 3     | 0.3187 | 110.02  | No       |
| m19    | 3     | 0.3244 | 110.02  | No       |

QED component analysis:
- m12 (group 2): ALERTS=0.0817 (nitro group structural alert) devastates QED
- m01/m19 (group 3): ALOGP~0.09, HBD~0.06 (sugar-like, very hydrophilic) drag QED down

Budget was exhausted before I could measure shorter-chain group 2 molecules (m09 chain 2, m02 chain 3). In group 1, ALERTS improved from 0.6397 (chains 4+) to 0.8425 (chains 2-3). If group 2 shows similar chain-dependent ALERTS improvement, shorter-chain molecules could have significantly higher QED.

**Best available measured option: m12** (QED=0.4709, PSA=72.24, different scaffold)
Note: This does not meet the QED ≥ 0.6 threshold. Shorter-chain group 2 molecules (m09, m02) would likely be better candidates but budget was exhausted before they could be measured.

## Budget Retrospective
Used 4 pages of listing (could have used 0-1 since group_by_scaffold gave all IDs) and 5 measurements on m05's series (could have used 3 for trend). Optimal strategy would have allocated more budget to Task 2 candidate screening.
