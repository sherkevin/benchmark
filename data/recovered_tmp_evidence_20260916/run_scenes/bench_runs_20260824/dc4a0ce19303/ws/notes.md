# Notes — QED Prediction and Lead Selection

## Job 1: Predict QED of m05

### Identifying the congeneric series for m05
m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
Scaffold: `COc1ccc(cc1)C(=O)N-` (methoxyphenyl amide), N-alkyl chain = 11 carbons

Pool molecules with same scaffold (differing only in chain length):
- m15: `COc1ccc(cc1)C(=O)NCC` — 2C chain
- m24: `COc1ccc(cc1)C(=O)NCCC` — 3C chain
- m28: `COc1ccc(cc1)C(=O)NCCCC` — 4C chain
- m17: `COc1ccc(cc1)C(=O)NCCCCC` — 5C chain
- m22: `COc1ccc(cc1)C(=O)NCCCCCC` — 6C chain

congener_ids = ["m15", "m24", "m28", "m17", "m22"]

### Measured QED values for the series
| mol_id | Chain length | QED     |
|--------|-------------|---------|
| m15    | 2           | 0.7621  |
| m24    | 3           | 0.7919  |
| m28    | 4           | 0.7518  |
| m17    | 5           | 0.7498  |
| m22    | 6           | 0.7377  |

### Observed trend
QED peaks at chain length 3 (0.7919), then declines:
- 3→4: ΔQED = -0.0401
- 4→5: ΔQED = -0.0020
- 5→6: ΔQED = -0.0121

The decline is non-linear and irregular. The relationship is clearly not a straight line.

### Model fitting — Exponential decay on chains 3–6
Since the peak is at chain 3 and the subsequent decline should asymptotically approach a lower bound (QED can't go below 0), I fit an exponential decay model to the declining portion (chains 3–6):

Linear regression of ln(QED) on chain length:
- ln(0.7919) = -0.2334
- ln(0.7518) = -0.2853
- ln(0.7498) = -0.2880
- ln(0.7377) = -0.3042

slope k = -0.02151, intercept = -0.18093
→ **QED(x) = 0.8345 × exp(-0.02151 × x)**

Verification at data points:
- x=3: predicted 0.7826, actual 0.7919 (error -0.009)
- x=4: predicted 0.7659, actual 0.7518 (error +0.014)
- x=5: predicted 0.7495, actual 0.7498 (error -0.000)
- x=6: predicted 0.7336, actual 0.7377 (error -0.004)

### Extrapolation to m05 (chain 11)
QED(11) = 0.8345 × exp(-0.02151 × 11) = 0.8345 × exp(-0.2366) = 0.8345 × 0.7894 ≈ **0.66**

I chose the exponential model over a 5-point quadratic (which gives 0.54) because:
1. The quadratic over-extrapolates (predicts negative QED at chain ~18, which is unphysical)
2. Cross-validation: quadratic fitted to chains 2–5 over-predicts the decline at chain 6 (predicts 0.705 vs actual 0.738)
3. Exponential decay is physically motivated (QED desirability functions are sigmoidal/exponential)
4. Multiple non-linear models on chains 3–6 (exponential: 0.66, power-law: 0.69, linear: 0.65) cluster around ~0.65–0.69

### Predicted QED for m05: **0.66**

---

## Job 2: Lead compound selection

### Constraints
1. Measured QED ≥ 0.6
2. PSA ≥ 40 (free descriptor)
3. Not in m05's series (free, from SMILES)

### Pre-filtering (constraints 2 & 3, no cost)
- Series D (biphenyl): all have PSA = 29.1 < 40 → excluded
- Series A (m05's series): excluded by constraint 3
- Remaining candidates: Series B (PSA=110) and Series C (PSA=72)

### Measurements and results
| mol_id | Series | Chain | PSA    | QED (measured) | QED ≥ 0.6? |
|--------|--------|-------|--------|----------------|------------|
| m12    | C      | 4     | 72.24  | 0.4709         | No         |
| m32    | C      | 5     | 72.24  | 0.4683         | No         |
| m33    | C      | 6     | 72.24  | 0.4591         | No         |
| m02    | C      | 3     | 72.24  | 0.6046         | **Yes**    |
| m09    | C      | 2     | 72.24  | 0.5826         | No         |
| m14    | B      | 2     | 110.02 | 0.3237         | No         |
| m01    | B      | 4     | 110.02 | 0.3187         | No         |

Series B molecules have very low QED (~0.32) due to high PSA, negative logP, and many HBD/HBA from the sugar alcohol moiety. Series C molecules show QED peaking at chain 3 (m02, QED=0.6046), with shorter and longer chains having lower QED. The nitro group is a structural alert that reduces QED compared to Series A.

### Selected lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: 0.6046 ≥ 0.6 ✓
- PSA: 72.24 ≥ 40 ✓
- Not in m05's series (Series C, nitrophenyl scaffold) ✓

---

## Budget usage
- 5 measurements for m05's series (m15, m24, m28, m17, m22)
- 7 measurements for Job 2 candidates (m12, m32, m33, m02, m09, m14, m01)
- Total: 12/12 measurements used
