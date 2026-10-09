# Task Notes

## Task Summary
- Predict QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - has 12-carbon chain (CCCCCCCCCCCC)
- Find backup lead: QED >= 0.6, PSA >= 40, NOT in same series as m05
- Budget: 17 measurements total

## m05 Analysis
SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: COc1ccc(cc1)C(=O)N- (methoxy-benzamide)
- Chain: CCCCCCCCCCCC (12 carbons - dodecyl)

Need to find congeners with same scaffold but different chain lengths.

## Pool Exploration - Complete

All 33 molecules catalogued. Identified 4 distinct scaffolds:

### Scaffold 1: COc1ccc(cc1)C(=O)N- (methoxy-benzamide) - m05's series
- m15: NCC (2C) - psa=38.33
- m17: NCCCCC (5C) - psa=38.33
- m22: NCCCCCC (6C) - psa=38.33
- m24: NCCC (3C) - psa=38.33
- m28: NCCCC (4C) - psa=38.33
- m05: NCCCCCCCCCCC (12C) - TARGET, not measurable

### Scaffold 2: O=[N+]([O-])c1ccc(cc1)C(=O)N- (nitro-benzamide)
- m02, m06, m09, m10, m12, m29, m31, m32, m33

### Scaffold 3: OCC(O)C(O)C(O)C(=O)N- (sugar-amide)
- m01, m03, m07, m14, m18, m19, m21, m25, m27

### Scaffold 4: c1ccc(cc1)-c1ccc(cc1)C(=O)N- (biphenyl-amide)
- m04, m08, m11, m13, m16, m20, m23, m26, m30

## Congeners of m05 to measure: m15, m17, m22, m24, m28

## Measuring congeners to establish QED trend

Results:
| Mol ID | Chain length | QED |
|--------|-------------|-----|
| m15 | 2C (ethyl) | 0.7621 |
| m24 | 3C (propyl) | 0.7919 |
| m28 | 4C (butyl) | 0.7518 |
| m17 | 5C (pentyl) | 0.7498 |
| m22 | 6C (hexyl) | 0.7377 |

Trend analysis:
- Peak QED at 3C (0.7919)
- Declining trend from 3C to 6C
- Linear fit: QED ≈ 0.8319 - 0.01646 * chain_length
- For 12C (m05): predicted QED ≈ 0.6344

## Finding backup lead compound

Constraints:
1. QED >= 0.6 (must measure)
2. psa >= 40 (free - check from catalogue)
3. NOT in m05's series (COc1ccc(cc1)C(=O)N-)

Molecules with psa >= 40 (excluding m05 series which has psa=38.33):

Scaffold 2 (nitro-benzamide, psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
Scaffold 3 (sugar-amide, psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

Scaffold 4 (biphenyl-amide, psa=29.1) - EXCLUDED (psa < 40)

Need to measure candidates from Scaffold 2 and 3 to find QED >= 0.6.

## Measurements for lead search

Measured:
- m02: QED = 0.6046 ✓ (meets all constraints!)
- m12: QED = 0.4709 (fails QED >= 0.6)
- m33: QED = 0.4591 (fails QED >= 0.6)

## Selected lead: m02

Verification:
1. QED = 0.6046 >= 0.6 ✓
2. psa = 72.24 >= 40 ✓
3. Scaffold is nitro-benzamide, NOT methoxy-benzamide (m05 series) ✓

## Final Answers

- target_qed (m05 prediction): ~0.63 (extrapolated from linear fit of congeners)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
