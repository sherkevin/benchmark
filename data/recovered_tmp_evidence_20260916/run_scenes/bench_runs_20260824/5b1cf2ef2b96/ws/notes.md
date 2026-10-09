# Notes: Medicinal Chemistry Analysis

## Budget Usage
- 9 list_pool calls (pages 1-9): 9 budget units
- 5 measure_qed calls (m05 series): 5 budget units  
- 3 measure_qed calls (lead candidates): 3 budget units
- Total: 17/17 budget used

## Congeneric Series Identification

### m05's Series (methoxybenzamide)
- **Scaffold**: `COc1ccc(cc1)C(=O)N`
- **Target**: m05, chain_len = 11 (never synthesized)
- **Pool members**:
  - m15: chain 2, SMILES `COc1ccc(cc1)C(=O)NCC`
  - m24: chain 3, SMILES `COc1ccc(cc1)C(=O)NCCC`
  - m28: chain 4, SMILES `COc1ccc(cc1)C(=O)NCCCC`
  - m17: chain 5, SMILES `COc1ccc(cc1)C(=O)NCCCCC`
  - m22: chain 6, SMILES `COc1ccc(cc1)C(=O)NCCCCCC`

### Other Series Identified
1. **Nitrobenzamide** (`O=[N+]([O-])c1ccc(cc1)C(=O)N`): m09, m02, m12, m32, m33, m29, m31, m06, m10 (chains 2-10)
2. **Sugar-like** (`OCC(O)C(O)C(O)C(=O)N`): m14, m27, m01, m19, m03, m21, m18, m07, m25 (chains 2-10)
3. **Biphenyl** (`c1ccc(cc1)-c1ccc(cc1)C(=O)N`): m13, m11, m04, m30, m23, m20, m26, m08, m16 (chains 2-10)

## Job 1: QED Prediction for m05

### Measured QED Values (m05's series)
| mol_id | chain_len | QED |
|--------|-----------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend Analysis
The QED shows a slight peak at chain length 3 (0.7919), then declines steadily through chains 4-6. The decline accelerates slightly, suggesting a non-linear (concave down) relationship.

### Polynomial Fits Compared
1. **Degree 2, all 5 points** (chosen): QED = **0.5416**, RMSE = 0.01188
   - Coefficients: 0.74932 + 0.017024x - 0.003264x²
   - Captures the peak near chain 2.6 and subsequent decline
2. Degree 1, all 5 points: QED = 0.695, RMSE = 0.01308
   - Doesn't capture the curvature, overpredicts
3. Degree 2, chains 3-6 only: QED = 0.9378, RMSE = 0.00539
   - Unreliable: parabola curves upward on declining segment, gives unrealistically high QED
4. Degree 1, chains 3-6 only: QED = 0.6508, RMSE = 0.00883

**Chosen model**: Degree-2 polynomial on all 5 measured points. This best captures the observed non-linear trend (rise, peak, decline) and extrapolates physically sensibly — a chain-11 molecule with alogp ~5.0 would have notably reduced drug-likeness.

**Predicted QED for m05**: 0.5416

## Job 2: Lead Compound Selection

### Constraint Screening
- **PSA ≥ 40**: Used `screen_candidates()` — 18 molecules pass (all nitro and sugar series members)
- **Not in m05's series**: All 18 PSA-passing candidates are from other series (m05's series has PSA = 38.33 < 40, so none pass PSA screen anyway)
- **QED ≥ 0.6**: Must measure — prioritized nitro series (more drug-like alogp, 1 aromatic ring)

### Lead Candidates Measured
| mol_id | series | PSA | QED | passes all 3? |
|--------|--------|-----|-----|---------------|
| m02 | nitro, chain 3 | 72.24 | 0.6046 | ✅ YES |
| m12 | nitro, chain 4 | 72.24 | 0.4709 | ❌ QED < 0.6 |
| m33 | nitro, chain 6 | 72.24 | 0.4591 | ❌ QED < 0.6 |

### Chosen Lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Scaffold: nitrobenzamide, chain 3
- Measured QED: **0.6046** (≥ 0.6 ✓)
- PSA: **72.24** (≥ 40 ✓)
- Series: nitrobenzamide ≠ m05's methoxybenzamide ✓

The shorter-chain nitro members had better QED, consistent with moderate lipophilicity being favorable for drug-likeness.
