# Notes

## Data Analysis

Read `data/pool.tsv` — 33 molecules with mol_id, smiles, mw, alogp, psa, rotb, arom.

### m05 Congeneric Series (Task 1)

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide with 11-carbon N-alkyl chain.

Scaffold: `COc1ccc(cc1)C(=O)N-` — molecules in the same series differ only in N-alkyl chain length.

Congeners identified:
| mol_id | Chain C | MW     | ALogP | PSA   | ROTB | AROM |
|--------|---------|--------|-------|-------|------|------|
| m15    | 2       | 179.22 | 1.445 | 38.33 | 3    | 1    |
| m24    | 3       | 193.25 | 1.835 | 38.33 | 4    | 1    |
| m28    | 4       | 207.27 | 2.225 | 38.33 | 5    | 1    |
| m17    | 5       | 221.30 | 2.615 | 38.33 | 6    | 1    |
| m22    | 6       | 235.33 | 3.005 | 38.33 | 7    | 1    |
| **m05**| **11**  | 305.46 | 4.956 | 38.33 | 12   | 1    |

### QED Measurements — m05 Congeners (5 measurements)

| mol_id | Chain C | QED    |
|--------|---------|--------|
| m15    | 2       | 0.7621 |
| m24    | 3       | 0.7919 |
| m28    | 4       | 0.7518 |
| m17    | 5       | 0.7498 |
| m22    | 6       | 0.7377 |

**Trend:** QED peaks at 3C (alogp ~1.8, near optimal), then declines as chain length / alogp / rotb increase.

### Extrapolation to m05 (11C)

Multiple methods:
1. **Linear (all 5 pts):** slope = -0.00909/C → QED(11) ≈ 0.695
2. **Linear (excl. 3C outlier):** slope = -0.00564/C → QED(11) ≈ 0.712
3. **Quadratic ln(QED) vs alogp (all 5 pts):** accounts for nonlinear QED penalties → QED(11) ≈ 0.57
4. **Quadratic ln(QED) vs chain (excl. 3C):** mild curvature → QED(11) ≈ 0.67

The QED formula penalizes high ALogP and high ROTB nonlinearly. m05 has ALogP=4.956 and ROTB=12. The quadratic model (method 4) provides the best balance.

**Predicted m05 QED: ~0.67**

### QED Measurements — Task 2 Candidates (7 measurements)

Nitrobenzamide series (PSA=72.24):
| mol_id | Chain C | QED    | PSA    | QED≥0.6? |
|--------|---------|--------|--------|----------|
| m09    | 2       | 0.5826 | 72.24  | No       |
| m02    | 3       | 0.6046 | 72.24  | **Yes**  |
| m12    | 4       | 0.4709 | 72.24  | No       |
| m32    | 5       | 0.4683 | 72.24  | No       |
| m33    | 6       | 0.4591 | 72.24  | No       |
| m29    | 7       | 0.4446 | 72.24  | No       |
| m31    | 8       | 0.4252 | 72.24  | No       |

### Task 2 Selection: m02

Only **m02** satisfies all three constraints:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. PSA = 72.24 ≥ 40 ✓
3. Different scaffold from m05 (nitrobenzamide vs methoxybenzamide) ✓

Other series:
- Biphenyl amides (PSA=29.1): PSA < 40, excluded
- Sugar acids (PSA=110.02): very negative ALogP and 0 aromatics, likely low QED

## Budget: 12/12 measurements used
