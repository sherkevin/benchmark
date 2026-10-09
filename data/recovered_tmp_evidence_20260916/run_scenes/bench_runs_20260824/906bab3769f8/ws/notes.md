# Notes: QED Prediction and Lead Selection

## Catalogue Organization

Paged through all 33 molecules (9 pages at 4/page, using 9 budget units). Identified four congeneric series, all amides differing only in N-alkyl chain length:

| Series | Scaffold | Members | PSA |
|--------|----------|---------|-----|
| A (target) | COc1ccc(cc1)C(=O)N- | m15(C2), m24(C3), m28(C4), m17(C5), m22(C6), m05(C11) | 38.33 |
| B | O=[N+]([O-])c1ccc(cc1)C(=O)N- | m09, m02, m12, m32, m33, m29, m31, m06, m10 (C2-C10) | 72.24 |
| C | OCC(O)C(O)C(O)C(=O)N- | m14, m27, m01, m19, m03, m21, m18, m07, m25 (C2-C10) | 110.02 |
| D | c1ccc(cc1)-c1ccc(cc1)C(=O)N- | m13, m11, m04, m30, m23, m20, m26, m08, m16 (C2-C10) | 29.10 |

## Job 1: Predict QED of m05 (C11 chain)

### Measurements (5 budget units)

| mol_id | Chain | alogp | QED (measured) |
|--------|-------|-------|-----------------|
| m15 | C2 | 1.445 | 0.7621 |
| m24 | C3 | 1.835 | 0.7919 |
| m28 | C4 | 2.225 | 0.7518 |
| m17 | C5 | 2.615 | 0.7498 |
| m22 | C6 | 3.005 | 0.7377 |

### Trend Analysis

QED peaks at C3 (alogp ≈ 1.8, near the QED logP optimum) then declines monotonically:
- C2→C3: +0.030 (alogp moving into optimal range)
- C3→C4: −0.040 (alogp moving past optimum)
- C4→C5: −0.002 (gentle decline)
- C5→C6: −0.012 (decline accelerating)

The C3 peak is chemically real (alogp ~1.8 is near the QED logP desirability maximum). Beyond C3, increasing logP and rotatable bonds drive QED down. The decline accelerates because QED's logP penalty function is nonlinear (steepening at high logP).

### Fitted Model

Excluded the C3 peak (which reflects a local optimum, not the long-chain trend) and fit a downward-opening quadratic to the 4 points (C2, C4, C5, C6):

**QED(n) = −0.000830·n² + 0.000875·n + 0.76344**

Validation:
- n=2: predicted 0.7619, actual 0.7621 (error 0.0002)
- n=4: predicted 0.7537, actual 0.7518 (error 0.0019)
- n=5: predicted 0.7471, actual 0.7498 (error 0.0027)
- n=6: predicted 0.7388, actual 0.7377 (error 0.0011)
- n=3 (excluded): predicted 0.7586, actual 0.7919 (expected miss — the peak)

Cross-checked with other models:
- Linear (all 5 points): 0.695
- Quadratic in alogp (even points): 0.686
- Log(alogp) model: 0.721
- Linear (C3–C6 only): 0.660

### Extrapolation to m05 (n=11)

QED(11) = −0.000830×121 + 0.000875×11 + 0.76344 = **0.673 ≈ 0.67**

This is consistent with the physical expectation: m05 has alogp=4.956 (near 5, heavily penalized) and rotb=12 (high), so QED should be substantially lower than measured congeners. The quadratic captures the accelerating penalty better than a linear model, while remaining bounded (unlike higher-order polynomials).

## Job 2: Lead Selection

### Constraint Filtering (free, no budget used)

1. **PSA ≥ 40**: Eliminates Series D (PSA=29.10). Candidates: Series B (PSA=72.24) and Series C (PSA=110.02).
2. **Not in Series A**: Series B and C already satisfy this.
3. **QED ≥ 0.6**: Requires measurement.

### Candidate Prioritization

Series C (sugar-like) molecules have very negative alogp (−2.8 to +0.3) and many H-bond donors (4 OH groups), which strongly penalize QED. Unlikely to reach QED ≥ 0.6.

Series B (nitrophenyl) molecules with moderate alogp (1.3–2.5) and low rotatable bonds (3–6) are most drug-like. Selected three candidates with the most favorable descriptor profiles.

### Measurements (3 budget units)

| mol_id | Chain | alogp | rotb | QED | Pass? |
|--------|-------|-------|------|-----|-------|
| m02 | C3 | 1.735 | 4 | 0.6046 | ✓ |
| m12 | C4 | 2.125 | 5 | 0.4709 | ✗ |
| m32 | C5 | 2.515 | 6 | 0.4683 | ✗ |

### Result

**m02** is the chosen lead: QED = 0.6046 ≥ 0.6, PSA = 72.24 ≥ 40, and it belongs to Series B (not Series A).

The nitrophenyl series shows a steep QED drop from C2/C3 to C4+, likely due to the nitro group's electronic effects becoming more penalized as lipophilicity increases. m02 (C3) hits the sweet spot of moderate logP (1.735) and low rotatable bonds (4).

## Budget Summary

- Listing: 9 pages = 9 budget units
- Series A measurements: 5 molecules = 5 budget units
- Lead candidate measurements: 3 molecules = 3 budget units
- **Total: 17/17 budget used**
