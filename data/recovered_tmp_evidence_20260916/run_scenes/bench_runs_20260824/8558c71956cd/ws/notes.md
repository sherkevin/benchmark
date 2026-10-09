# Notes

## Task 1: Predict QED of m05

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` — methoxybenzamide with 11-carbon N-alkyl chain.

### Congeneric series (methoxybenzamides — same scaffold `COc1ccc(cc1)C(=O)N-`, varying N-alkyl chain)

| mol_id | chain_C | mw     | alogp | psa   | rotb | QED (measured) |
|--------|---------|--------|-------|-------|------|----------------|
| m15    | 2       | 179.22 | 1.445 | 38.33 | 3    | 0.7621         |
| m24    | 3       | 193.25 | 1.835 | 38.33 | 4    | 0.7919         |
| m28    | 4       | 207.27 | 2.225 | 38.33 | 5    | 0.7518         |
| m17    | 5       | 221.30 | 2.615 | 38.33 | 6    | 0.7498         |
| m22    | 6       | 235.33 | 3.005 | 38.33 | 7    | 0.7377         |
| **m05**| **11**  | **305.46** | **4.956** | **38.33** | **12** | **predicted** |

### Linear regression: QED vs chain_length (all 5 measured members)

- n=5, Σx=20, Σy=3.7933, Σxy=15.0823, Σx²=90
- slope = (5×15.0823 − 20×3.7933) / (5×90 − 400) = −0.4545/50 = **−0.00909**
- intercept = (3.7933 + 0.00909×20) / 5 = **0.79502**
- **Predicted QED(m05, 11C) = 0.79502 − 0.00909 × 11 ≈ 0.695**

The trend shows QED gradually decreasing as the N-alkyl chain lengthens (due to increasing logP and rotatable bonds). Linear extrapolation to 11 carbons gives ~0.695.

## Task 2: Find a lead molecule

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40
3. Not in m05's congeneric series (must not be a methoxybenzamide)

### Molecules with psa ≥ 40, not methoxybenzamides

Focused on the nitrobenzamide series (`O=[N+]([O-])c1ccc(cc1)C(=O)N-`, psa=72.24):

| mol_id | chain_C | alogp | QED    | Pass QED≥0.6? |
|--------|---------|-------|--------|---------------|
| m09    | 2       | 1.345 | 0.5826 | ❌            |
| m02    | 3       | 1.735 | 0.6046 | ✅            |
| m12    | 4       | 2.125 | 0.4709 | ❌            |
| m32    | 5       | 2.515 | 0.4683 | ❌            |
| m33    | 6       | 2.905 | 0.4591 | ❌            |
| m29    | 7       | 3.295 | 0.4446 | ❌            |
| m31    | 8       | 3.685 | 0.4252 | ❌            |

### Selected: m02

- **QED = 0.6046** (≥ 0.6) ✅
- **PSA = 72.24** (≥ 40) ✅
- **Nitrobenzamide scaffold** — different from m05's methoxybenzamide ✅

m02 has the shortest alkyl chain (3C) among those tested in the nitrobenzamide series, giving it the most favorable logP (1.735) and fewest rotatable bonds (4), which explains why it has the highest QED.

## Budget: 12/12 measurements used
- 5 for m05 congeneric series (m15, m24, m28, m17, m22)
- 7 for Task 2 candidates (m09, m02, m12, m32, m33, m29, m31)
