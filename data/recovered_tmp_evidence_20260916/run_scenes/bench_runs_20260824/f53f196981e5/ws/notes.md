# Notes on QED Prediction and Lead Selection

## Initial Observations

From data/pool.tsv, I see 33 molecules with 5 different scaffolds:

1. **Methoxy-phenyl scaffold**: `COc1ccc(cc1)C(=O)NCC...` (m05, m15, m17, m22, m24, m28)
   - Characteristic: methoxy group (COc) attached to phenyl ring
   - m05 target: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (C12 chain)

2. **Nitro-phenyl scaffold**: `O=[N+]([O-])c1ccc(cc1)C(=O)NCC...` (m02, m06, m09, m10, m12, m29, m31, m32, m33)
   - Characteristic: nitro group `O=[N+]([O-])`

3. **Polyhydroxy scaffold**: `OCC(O)C(O)C(O)C(=O)NCC...` (m01, m03, m07, m14, m18, m19, m21, m25, m27)
   - Characteristic: multiple hydroxyl groups `OCC(O)C(O)C(O)`

4. **Biphenyl scaffold**: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCC...` (m04, m08, m11, m13, m16, m20, m23, m26, m30)
   - Characteristic: biphenyl `c1ccc(cc1)-c1ccc(cc1)`

## Job 1: Identify m05's series

m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxy-phenyl amide)
- Chain: `CCCCCCCCCCC` (C12 alkyl chain)

Looking for molecules with same scaffold:
- m15: `COc1ccc(cc1)C(=O)NCC` (C2 chain)
- m17: `COc1ccc(cc1)C(=O)NCCCCC` (C5 chain)  
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` (C6 chain)
- m24: `COc1ccc(cc1)C(=O)NCCC` (C3 chain)
- m28: `COc1ccc(cc1)C(=O)NCCCC` (C4 chain)

All have the exact same scaffold `COc1ccc(cc1)C(=O)N` followed by varying alkyl chains.

**m05 series members**: m15, m17, m22, m24, m28 (all methoxy-phenyl scaffold)

## Measurement Strategy

Budget: 12 measurements total for both jobs.
Need to measure m05 series members to establish trend, then extrapolate to m05 (C12 chain).
Also need to find a lead compound meeting constraints.

First, I measured the m05 series members to understand QED trend vs chain length.

## m05 Series Measurements

| mol_id | SMILES fragment | Alkyl chain length | QED measured |
|--------|-----------------|-------------------|--------------|
| m15 | NCC | C2 | 0.7621 |
| m24 | NCCC | C3 | 0.7919 |
| m28 | NCCCC | C4 | 0.7518 |
| m17 | NCCCCC | C5 | 0.7498 |
| m22 | NCCCCCC | C6 | 0.7377 |

Target m05: `NCCCCCCCCCCCC` = C12 chain

Observations:
1. QED generally decreases with increasing chain length after C3
2. Peak appears at C3 chain (m24: 0.7919)
3. From C3 to C6, QED decreases from 0.7919 to 0.7377 (drop of ~0.0542 over 3 carbons = ~0.0181 per carbon)

## Trend Analysis

The data shows:
- C2: 0.7621
- C3: 0.7919 (peak)
- C4: 0.7518 
- C5: 0.7498
- C6: 0.7377

Pattern appears non-linear. The QED rises from C2 to C3, then declines from C3 onward.
For extrapolation to C12, I need to model this trend.

## Job 2: Lead Compound Search

Constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40  
3. not in same series as m05

Checking psa values by scaffold:
- Methoxy-phenyl (m05 series): psa = 38.33 < 40 (fails constraint 2)
- Nitro-phenyl: psa = 72.24 ≥ 40 ✓
- Polyhydroxy: psa = 110.02 ≥ 40 ✓  
- Biphenyl: psa = 29.1 < 40 (fails constraint 2)

So only nitro-phenyl and polyhydroxy scaffolds qualify for constraint 2.

### Measurements for Lead Search

Budget used: 5 for m05 series + 6 for lead search = 11 total, 1 remaining.

Nitro-phenyl series measurements:
- m02 (C3): QED = 0.6046 ≥ 0.6 ✓
- m09 (C2): QED = 0.5826 < 0.6
- m12 (C4): QED = 0.4709 < 0.6
- m32 (C5): QED = 0.4683 < 0.6

Polyhydroxy series measurements:
- m01: QED = 0.3187 < 0.6
- m14: QED = 0.3237 < 0.6

**Current best lead candidate**: m02 with QED = 0.6046
- psa = 72.24 ≥ 40 ✓
- Not in m05 series ✓
- QED ≥ 0.6 ✓

Only 1 measurement remaining. Used it to measure m06 (nitro-phenyl C10): QED = 0.4011 < 0.6.
Budget exhausted (12/12).

**Final lead choice**: m02
- QED measured: 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓  
- Not in m05 series ✓

## m05 QED Prediction

Data for m05 series (methoxy-phenyl):
- C2 (m15): 0.7621
- C3 (m24): 0.7919 (peak)
- C4 (m28): 0.7518
- C5 (m17): 0.7498  
- C6 (m22): 0.7377
- Target: C12 (m05)

Trend analysis: QED peaks at C3, then declines. Post-peak slopes:
- C3→C4: -0.0401 per carbon
- C4→C5: -0.0020 per carbon
- C5→C6: -0.0121 per carbon
- Average: -0.01807 per carbon

Extrapolating from C6 to C12 (Δn=6):
ΔQED = 6 × -0.01807 = -0.1084
Predicted QED at C12 = 0.7377 - 0.1084 = 0.6293

Given slope variability, prediction range: 0.62-0.64. 
**Final prediction**: QED = 0.629

## Congener IDs

m05 series members (same scaffold, varying alkyl chain):
- m15 (C2), m17 (C5), m22 (C6), m24 (C3), m28 (C4)