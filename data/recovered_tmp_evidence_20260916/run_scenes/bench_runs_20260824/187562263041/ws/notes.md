# Notes: QED Prediction for m05 and Lead Compound Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: 4-methoxy-phenyl amide
- Chain: tridecyl (13 carbons)

Molecules in the same series (same scaffold, different chain lengths):
| mol_id | SMILES | Chain length |
|--------|--------|-------------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 (ethyl) |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 (propyl) |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 (butyl) |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 (pentyl) |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 (hexyl) |
| m05 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 13 (tridecyl) - TARGET |

### QED Measurements for Congeners

Measured all 5 available congeners:
| mol_id | Chain length | QED |
|--------|-------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend Analysis

The QED peaks at chain length 3 (m24: 0.7919), then decreases monotonically:
- n=3 to n=4: -0.0401
- n=4 to n=5: -0.0020
- n=5 to n=6: -0.0121

Overall decrease from n=3 to n=6: 0.0542 over 3 carbons ≈ 0.018 per carbon.

Linear regression on n=3,4,5,6 gives: QED ≈ 0.8319 - 0.01646 × n

At n=13 (linear extrapolation): QED ≈ 0.8319 - 0.01646 × 13 ≈ 0.618

### Non-linear Considerations

QED penalizes extreme molecular properties. m05 has alogp = 4.956, which is well outside the optimal range (~2-3). The QED formula includes desirability functions that impose stronger penalties for values far from optimal.

Given the non-linear nature of QED and the very long chain (13 carbons) leading to high lipophilicity, I adjusted the prediction downward from the linear estimate.

**Predicted QED for m05: 0.58**

## Job 2: Lead Compound Selection

### Constraints
1. QED ≥ 0.6 (must measure)
2. psa ≥ 40 (free from descriptors)
3. Not in m05's series (free from SMILES)

### Filtering by psa ≥ 40

Molecules with psa ≥ 40 (excluding m05 series which has psa=38.33):
- Nitro-phenyl series: m02, m06, m09, m10, m12, m29, m31, m32, m33 (psa=72.24)
- Sugar-like series: m01, m03, m07, m14, m18, m19, m21, m25, m27 (psa=110.02)

### QED Measurements for Candidates

With limited budget (17 total, 5 used for congeners = 12 remaining), measured strategic candidates with moderate alogp:

| mol_id | alogp | QED | Meets QED ≥ 0.6? |
|--------|-------|-----|------------------|
| m02 | 1.735 | 0.6046 | YES ✓ |
| m12 | 2.125 | 0.4709 | No |
| m32 | 2.515 | 0.4683 | No |

### Selected Lead: m02

Verification:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in m05 series (nitro-phenyl, not methoxy-phenyl) ✓

**Lead compound: m02 with measured QED = 0.6046**

## Budget Summary
- list_pool: 9 pages = 9 budget units
- measure_qed: 8 measurements = 8 budget units
- Total: 17/17 budget used
