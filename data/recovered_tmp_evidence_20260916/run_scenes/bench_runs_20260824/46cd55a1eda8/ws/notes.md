# Notes

## Task 1: Predict QED for m05

m05 (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`) has never been synthesized, so I need to predict its QED from its congeneric series.

### Congeneric series identification
Molecules with the same scaffold (methoxy-benzamide `COc1ccc(cc1)C(=O)N-`) differing only in N-alkyl chain length:
- m15: C2 chain
- m24: C3 chain
- m28: C4 chain
- m17: C5 chain
- m22: C6 chain
- m05: C13 chain (target)

### Measurements of congeneric series
| mol_id | Chain length | QED |
|--------|-------------|-----|
| m15 | C2 | 0.7621 |
| m24 | C3 | 0.7919 |
| m28 | C4 | 0.7518 |
| m17 | C5 | 0.7498 |
| m22 | C6 | 0.7377 |

### Trend analysis
- QED peaks at C3 (0.7919)
- Gradual decline from C3 to C6: ~0.018 per carbon
- Extrapolating to C13 (10 carbons from peak): significant decrease expected

Given m05's properties (mw=305.46, alogp=4.956, rotb=12), the long alkyl chain significantly impacts drug-likeness negatively. Linear extrapolation from C3-C6 trend suggests ~0.62, but accounting for non-linear effects of extreme logP and chain length, I estimate **QED ≈ 0.45**.

## Task 2: Find a lead molecule

Criteria:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. NOT in m05's congeneric series (exclude m05, m15, m17, m22, m24, m28)

### Measurements for candidate molecules
| mol_id | psa | QED | Meets criteria? |
|--------|-----|-----|-----------------|
| m01 | 110.02 | 0.3187 | No (QED < 0.6) |
| m02 | 72.24 | 0.6046 | **Yes** ✓ |
| m09 | 72.24 | 0.5826 | No (QED < 0.6) |
| m14 | 110.02 | 0.3237 | No (QED < 0.6) |

### Selected lead: m02
- **mol_id**: m02
- **SMILES**: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- **QED**: 0.6046 (measured) ≥ 0.6 ✓
- **psa**: 72.24 ≥ 40 ✓
- **Not in m05 series**: m02 is a nitro-benzamide, m05 is a methoxy-benzamide ✓

## Budget usage
Total measurements: 9 out of 12
- 5 for m05 congeneric series (m15, m24, m28, m17, m22)
- 4 for Task 2 candidates (m01, m02, m09, m14)
