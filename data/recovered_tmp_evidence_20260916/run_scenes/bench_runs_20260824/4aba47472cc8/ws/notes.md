# Notes for QED Prediction Task

## Task Overview
- Predict QED for m05 (unmeasurable, probe_unavailable)
- Pick a lead compound from pool with: QED ≥ 0.6, psa ≥ 40, not in m05's series
- Budget: 12 measurements total

## Step 1: Identify m05's congeneric series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: p-methoxy-benzamide (`COc1ccc(cc1)C(=O)N-`)
- Chain: 11-carbon alkyl chain (undecyl)

Congeners in pool (same scaffold, different chain lengths):
| Mol ID | SMILES fragment | Chain length (n) |
|--------|-----------------|------------------|
| m15 | NCC | 2 (ethyl) |
| m24 | NCCC | 3 (propyl) |
| m28 | NCCCC | 4 (butyl) |
| m17 | NCCCCC | 5 (pentyl) |
| m22 | NCCCCCC | 6 (hexyl) |

**Congener IDs: m15, m17, m22, m24, m28**

## Step 2: Measure QED for congeners

Measurements (used 5 of 12 budget):
| Mol ID | Chain length (n) | QED |
|--------|------------------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## Step 3: Fit trend and extrapolate to m05 (n=11)

**Observed trend:**
- QED peaks at n=3 (0.7919)
- Declines for n > 3
- Drop from n=3 to n=6: 0.7919 → 0.7377 (-0.0542 over 3 carbons)

**Fitting approach:**
Linear regression on all 5 points:
- Mean n = 4, Mean QED = 0.75866
- Slope = -0.00909 per carbon
- Intercept = 0.79502
- Model: QED(n) ≈ 0.79502 - 0.00909×n

**Extrapolation to n=11:**
QED(11) ≈ 0.79502 - 0.00909×11 = 0.79502 - 0.100 = **0.695**

Alternative check - linear decline from n=6:
- Average decline rate from n=3 to n=6: ~0.018/carbon
- From n=6 to n=11: 0.7377 - 5×0.018 ≈ 0.648

Given the quadratic nature (peak then decline), the true value is likely between 0.65-0.70.

**Prediction: m05 QED ≈ 0.69**

## Step 4: Find lead compound (QED ≥ 0.6, psa ≥ 40, not in m05 series)

**Filter by psa ≥ 40:**
- Polyol series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

**Exclude m05 series:** m15, m17, m22, m24, m28 (already excluded by psa filter since psa=38.33 < 40)

**Measurements for lead candidates (7 measurements):**
| Mol ID | Series | QED | Meets QED≥0.6? |
|--------|--------|-----|----------------|
| m01 | Polyol | 0.3187 | No |
| m02 | Nitro | 0.6046 | **Yes** |
| m09 | Nitro | 0.5826 | No |
| m14 | Polyol | 0.3237 | No |
| m27 | Polyol | 0.3330 | No |
| m12 | Nitro | 0.4709 | No |
| m32 | Nitro | 0.4683 | No |

**Selected lead: m02**
- QED measured: 0.6046 ≥ 0.6 ✓
- psa: 72.24 ≥ 40 ✓
- Not in m05 series (nitro-benzamide vs methoxy-benzamide) ✓

## Budget Summary
- 5 measurements for congeners (m15, m17, m22, m24, m28)
- 7 measurements for lead candidates (m01, m02, m09, m14, m27, m12, m32)
- Total: 12/12 measurements used
