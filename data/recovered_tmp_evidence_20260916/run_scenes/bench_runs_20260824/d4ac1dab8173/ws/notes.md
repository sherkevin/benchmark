# Notes: QED Prediction for m05 and Lead Selection

## Job 1: Predict QED of m05

### Identifying the Congeneric Series

Target m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`:
- Scaffold: `COc1ccc(cc1)C(=O)N` (methoxybenzamide)
- N-alkyl chain length: 11 carbons

Pool molecules sharing this scaffold (confirmed via `describe_structure`):

| mol_id | chain_len | QED (measured) |
|--------|-----------|----------------|
| m15    | 2         | 0.7621         |
| m24    | 3         | 0.7919         |
| m28    | 4         | 0.7518         |
| m17    | 5         | 0.7498         |
| m22    | 6         | 0.7377         |

**Congener IDs: m15, m24, m28, m17, m22**

### Trend Analysis

The QED rises from C2 (0.7621) to C3 (0.7919), then steadily declines through C4-C6 (0.7518 → 0.7498 → 0.7377). This suggests a peak around C3 followed by a decline as increasing lipophilicity (alogp) starts penalizing drug-likeness.

### Extrapolation

I fitted a degree-2 polynomial (quadratic) to all 5 data points:

**QED(x) = 0.74932 + 0.017024·x − 0.003264·x²**

- RMSE on training points: 0.01188
- The negative x² coefficient reflects the accelerating penalty from increasing lipophilicity
- At x=11 (m05): **QED = 0.5416**

Comparison with alternatives:
- Linear fit (all 5 pts): predicts 0.695 — too optimistic as it ignores the curvature/accelerating decline
- Quadratic on C3-C6 only: predicts 0.9378 — physically impossible (parabola opens upward, would exceed 1.0)

The full 5-point quadratic is the most physically reasonable: m05 has alogp = 4.956, which is well outside the ideal range and would heavily penalize QED.

**Predicted QED for m05: 0.5416**

---

## Job 2: Lead Compound Selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40
3. Not in the m05 series (scaffold ≠ `COc1ccc(cc1)C(=O)N`)

### PSA Screening

| Series | Scaffold | PSA | Passes PSA ≥ 40? |
|--------|----------|-----|-------------------|
| Sugar | OCC(O)C(O)C(O)C(=O)N | 110.02 | ✅ |
| Nitrobenzene | O=[N+]([O-])c1ccc(cc1)C(=O)N | 72.24 | ✅ |
| Biphenyl | c1ccc(cc1)-c1ccc(cc1)C(=O)N | 29.10 | ❌ |
| Methoxybenzamide | COc1ccc(cc1)C(=O)N | 38.33 | ❌ |

Only sugar and nitro series pass the PSA constraint.

### QED Measurements of Candidates

**Nitro series** (measured middle-range alogp molecules first):

| mol_id | chain_len | alogp | QED | QED ≥ 0.6? |
|--------|-----------|-------|-----|------------|
| m02    | 3         | 1.735 | 0.6046 | ✅ |
| m09    | 2         | 1.345 | 0.5826 | ❌ |
| m12    | 4         | 2.125 | 0.4709 | ❌ |
| m32    | 5         | 2.515 | 0.4683 | ❌ |
| m33    | 6         | 2.905 | 0.4591 | ❌ |
| m29    | 7         | 3.295 | 0.4446 | ❌ |

**Sugar series** (spot check):

| mol_id | chain_len | alogp | QED | QED ≥ 0.6? |
|--------|-----------|-------|-----|------------|
| m14    | 2         | -2.802| 0.3237 | ❌ |

The sugar series has very poor QED due to extremely high PSA (110) and negative alogp values. The nitro series peaks around C3 (m02) where alogp is moderate.

### Selected Lead: **m02**

- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- QED measured: **0.6046** ≥ 0.6 ✅
- PSA: **72.24** ≥ 40 ✅
- Series: nitrobenzene (not m05's methoxybenzamide series) ✅

---

## Budget Usage

Total measurements: 12/12
- 5 for m05 series (m15, m24, m28, m17, m22)
- 7 for lead candidates (m12, m32, m33, m02, m09, m29, m14)
