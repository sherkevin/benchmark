# Notes

## Job 1: Predict QED of m05

### Identifying the m05 series (congeners)
m05 has SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` - a methoxy-phenyl amide with an 11-carbon alkyl chain.

The same series (same scaffold COc1ccc(cc1)C(=O)N-, differing only in chain length) includes:
- m15: NCC (C2 chain)
- m24: NCCC (C3 chain)
- m28: NCCCC (C4 chain)
- m17: NCCCCC (C5 chain)
- m22: NCCCCCC (C6 chain)
- m05: NCCCCCCCCCCC (C11 chain) - TARGET

### Measurements for m05 series
| mol_id | chain length | alogp | QED |
|--------|-------------|-------|-----|
| m15    | C2          | 1.445 | 0.7621 |
| m24    | C3          | 1.835 | 0.7919 |
| m28    | C4          | 2.225 | 0.7518 |
| m17    | C5          | 2.615 | 0.7498 |
| m22    | C6          | 3.005 | 0.7377 |

### Trend analysis
QED peaks at C3 (m24) with alogp ~1.8, then decreases as chain length (and alogp) increases.
- From C3 to C6, QED drops from 0.7919 to 0.7377 (Δ = -0.0542 over 3 carbons)
- Average decrease: ~0.018 per carbon beyond C3

For extrapolation to C11 (m05):
- m05 is 5 carbons longer than m22 (C6)
- m05 alogp = 4.956, significantly higher than the measured range (1.445-3.005)
- QED penalizes high alogp non-linearly; the penalty accelerates above alogp ~3-4

Linear extrapolation from C3-C6 trend:
- Slope ≈ -0.0181 per carbon
- Prediction: 0.7919 + (-0.0181 × 8) = 0.647

However, considering:
1. m05 has alogp = 4.956 (well beyond optimal ~2-3)
2. m05 has rotb = 12 (beyond optimal <10)
3. QED formula applies stronger penalties for extreme values

The actual QED is likely lower than linear extrapolation suggests. Accounting for non-linear alogp penalty and high rotb count, I estimate **m05 QED ≈ 0.58**.

## Job 2: Pick a backup lead compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40 (from pool.tsv)
3. NOT in same series as m05

### Filtering by psa ≥ 40
- Series 1 (m05 series, COc1ccc...): psa = 38.33 - EXCLUDED (also constraint 3)
- Series 2 (nitro-phenyl, O=[N+]...): psa = 72.24 ✓
- Series 3 (sugar, OCC(O)...): psa = 110.02 ✓
- Series 4 (biphenyl, c1ccc...): psa = 29.1 ✗

### Potential leads (Series 2 and 3 only)
Measurements taken:
| mol_id | series | QED | meets QED≥0.6? |
|--------|--------|-----|----------------|
| m02    | S2     | 0.6046 | YES ✓ |
| m12    | S2     | 0.4709 | NO |
| m32    | S2     | 0.4683 | NO |
| m33    | S2     | 0.4591 | NO |
| m01    | S3     | 0.3187 | NO |
| m19    | S3     | 0.3244 | NO |
| m21    | S3     | 0.3297 | NO |

### Selected lead: m02
- QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- Not in m05 series (nitro-phenyl scaffold) ✓

All three constraints satisfied.

## Budget usage
- 5 measurements for m05 series congeners (m15, m24, m28, m17, m22)
- 7 measurements for potential leads (m02, m12, m32, m33, m01, m19, m21)
- Total: 12/12 measurements used
