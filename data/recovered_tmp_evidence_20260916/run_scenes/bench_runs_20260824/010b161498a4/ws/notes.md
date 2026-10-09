# Notes: QED Prediction and Lead Selection

## Task Summary
- Predict QED for m05 (unmeasurable, same series extrapolation needed)
- Pick a backup lead: QED ≥ 0.6, psa ≥ 40, NOT in m05's series
- Budget: 12 measurements total

## Step 1: Identify m05's congeneric series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: 4-methoxybenzoyl (`COc1ccc(cc1)C(=O)N-`)
- Chain: 13 carbons (CCCCCCCCCCC)

Congeners in pool (same scaffold, different chain length):
- m15: NCC (2 carbons)
- m24: NCCC (3 carbons)
- m28: NCCCC (4 carbons)
- m17: NCCCCC (5 carbons)
- m22: NCCCCCC (6 carbons)

m05 has 13 carbons - much longer than any measured congener (extrapolation required).

## Step 2: Measure the congeneric series

Measured all 5 congeners:
| mol_id | chain length | QED |
|--------|--------------|-----|
| m15    | 2            | 0.7621 |
| m24    | 3            | 0.7919 |
| m28    | 4            | 0.7518 |
| m17    | 5            | 0.7498 |
| m22    | 6            | 0.7377 |

**Trend observed**: QED peaks at chain length 3 (0.7919), then decreases as chain lengthens.

## Step 3: Extrapolate to m05 (chain length 13)

Fitted quadratic model to declining portion (chains 4, 5, 6):
- y = -0.00505x² + 0.04345x + 0.6588

At x=13:
- y = -0.00505(169) + 0.04345(13) + 0.6588
- y = -0.85345 + 0.56485 + 0.6588
- y = 0.3702

**Predicted QED for m05: 0.37**

## Step 4: Find backup lead compound

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40 (free from table)
3. NOT in m05's series (not 4-methoxybenzoyl)

Candidates with psa ≥ 40, not in m05 series:
- Polyol series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

Measurements performed:
| mol_id | psa | QED | Passes? |
|--------|-----|-----|---------|
| m01    | 110.02 | 0.3187 | No (QED < 0.6) |
| m02    | 72.24  | 0.6046 | YES! |
| m09    | 72.24  | 0.5826 | No (QED < 0.6) |
| m14    | 110.02 | 0.3237 | No (QED < 0.6) |

**Selected lead: m02**
- QED = 0.6046 (≥ 0.6 ✓)
- psa = 72.24 (≥ 40 ✓)
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` - nitrobenzoyl series, NOT methoxybenzoyl ✓

## Measurements used: 9 of 12
- Congeners: m15, m24, m28, m17, m22 (5 measurements)
- Lead candidates: m01, m02, m09, m14 (4 measurements)
- Total: 9 measurements

## Final Answer
- target_qed: 0.37 (predicted for m05 via quadratic extrapolation)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: [m15, m24, m28, m17, m22]
