# Notes — QED Prediction and Lead Selection

## 1. Congeneric Series Identification

**Target m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N-` (4-methoxybenzamide)
- N-alkyl chain: 11 carbons

Paged through all 33 pool molecules (9 pages) and identified molecules sharing the same scaffold by matching the `COc1ccc(cc1)C(=O)N-` fragment, differing only in N-alkyl chain length.

### Methoxyphenyl series (congeners of m05):
| mol_id | Chain (C) | MW     | ALogP | PSA   | Rotb | Arom |
|--------|-----------|--------|-------|-------|------|------|
| m15    | 2         | 179.22 | 1.445 | 38.33 | 3    | 1    |
| m24    | 3         | 193.25 | 1.835 | 38.33 | 4    | 1    |
| m28    | 4         | 207.27 | 2.225 | 38.33 | 5    | 1    |
| m17    | 5         | 221.30 | 2.615 | 38.33 | 6    | 1    |
| m22    | 6         | 235.33 | 3.005 | 38.33 | 7    | 1    |
| **m05**| **11**    | **305.46** | **4.956** | **38.33** | **12** | **1** |

Each additional CH2 adds: MW ~ +14, ALogP ~ +0.39, Rotb = +1. PSA and Arom are constant.

### Other series identified:
- **Nitrophenyl** (O=[N+]([O-])c1ccc(cc1)C(=O)N-): m09, m02, m12, m32, m33, m29, m31, m06, m10 (PSA=72.24)
- **Sugar acid** (OCC(O)C(O)C(O)C(=O)N-): m14, m27, m01, m19, m03, m21, m18, m07, m25 (PSA=110.02)
- **Biphenyl** (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): m13, m11, m04, m30, m23, m20, m26, m08, m16 (PSA=29.1)

## 2. QED Measurements

### Budget usage:
- 9 pages of list_pool = 9 budget units
- 5 methoxyphenyl congener measurements = 5 budget units  
- 3 nitrophenyl lead candidate measurements = 3 budget units
- Total: 17/17 (exhausted)

### Methoxyphenyl series QED:
| mol_id | Chain (C) | QED    |
|--------|-----------|--------|
| m15    | 2         | 0.7621 |
| m24    | 3         | 0.7919 |
| m28    | 4         | 0.7518 |
| m17    | 5         | 0.7498 |
| m22    | 6         | 0.7377 |

### Lead candidate measurements (nitrophenyl):
| mol_id | Chain (C) | QED    |
|--------|-----------|--------|
| m12    | 4         | 0.4709 |
| m32    | 5         | 0.4683 |
| m33    | 6         | 0.4591 |

## 3. Trend Fitting for m05 QED Prediction

### Observed trend:
- QED peaks at n=3 (0.7919) then declines
- Decline: n=3→4: -0.040, n=4→5: -0.002, n=5→6: -0.012
- Overall nonlinear: rise from n=2→3 then accelerating decline

### Model: Quadratic fit (least-squares) on all 5 data points

QED(n) = a + b*(n-4) + c*(n-4)^2

Using centered variable x = n - 4 (n_bar = 4):
- b = -0.00909
- c = -0.003264  
- a = 0.76519

**Fitted model: QED(n) = 0.76519 - 0.00909*(n-4) - 0.003264*(n-4)^2**

Fit quality: max error ~0.021 at n=3, all others <0.014.

### Extrapolation to m05 (n=11):

QED(11) = 0.76519 - 0.00909*7 - 0.003264*49
        = 0.76519 - 0.06363 - 0.15994
        = **0.54**

### Why quadratic:
1. Task explicitly says relationship is NOT linear
2. Quadratic is simplest model capturing observed curvature (peak + accelerating decline)
3. Negative c coefficient is physically motivated: ALogP=4.956 incurs severe QED penalty, Rotb=12 is penalized
4. Alternatives considered: linear (0.70, ruled out by task), cubic (unstable extrapolation), exponential decay (0.66, poor fit at n=4)

## 4. Lead Compound Selection

### Pre-filtering (free):
- Methoxyphenyl: excluded (constraint 3; also PSA=38.33<40)
- Biphenyl: excluded (PSA=29.1<40)
- Nitrophenyl: PSA=72.24 >= 40, not methoxyphenyl -- ELIGIBLE
- Sugar acid: PSA=110.02 >= 40, not methoxyphenyl -- ELIGIBLE

### Outcome:
Measured 3 mid-chain nitrophenyl compounds (favorable ALogP 2.1-2.9). All had QED ~0.46-0.47, below 0.6 threshold. The nitro group imposes significant QED penalty. Budget exhausted before sugar acid compounds could be tested.

Best available lead: **m12** (QED=0.4709, PSA=72.24, nitrophenyl).

## 5. Final Answers
- **Predicted QED for m05**: 0.54
- **Lead compound**: m12 (QED=0.4709)
- **Congeners of m05**: m15, m24, m28, m17, m22
