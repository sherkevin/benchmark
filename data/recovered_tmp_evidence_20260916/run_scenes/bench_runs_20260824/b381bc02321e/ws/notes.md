# Medicinal Chemistry Analysis Notes

## Job 1: Predict QED of m05

### Target Molecule
- **m05**: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: methoxyphenyl amide (p-anisamide)
- N-alkyl chain: 11 carbons
- MW: 305.46, alogp: 4.956, PSA: 38.33, rotb: 12, arom: 1

### Congeneric Series Discovery
Searched for scaffold substring `COc1ccc(cc1)C(=O)N` (cost: 2 search pages = 2 budget units).
Found 6 molecules total (5 congeners + m05 target):

| mol_id | SMILES | Chain C# | MW | alogp | PSA | rotb |
|--------|--------|----------|------|-------|------|------|
| m15 | COc1ccc(cc1)C(=O)NCC | 2 | 179.22 | 1.445 | 38.33 | 3 |
| m24 | COc1ccc(cc1)C(=O)NCCC | 3 | 193.25 | 1.835 | 38.33 | 4 |
| m28 | COc1ccc(cc1)C(=O)NCCCC | 4 | 207.27 | 2.225 | 38.33 | 5 |
| m17 | COc1ccc(cc1)C(=O)NCCCCC | 5 | 221.30 | 2.615 | 38.33 | 6 |
| m22 | COc1ccc(cc1)C(=O)NCCCCCC | 6 | 235.33 | 3.005 | 38.33 | 7 |
| **m05** | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | **11** | 305.46 | 4.956 | 38.33 | 12 |

### QED Measurements (5 budget units)
| mol_id | Chain C# | QED |
|--------|----------|-------|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

### Trend Observed
QED rises to a peak at n=3 (QED=0.7919), then declines:
- n=2→3: +0.030 (rising)
- n=3→4: -0.040 (sharp initial drop)
- n=4→5: -0.002 (plateau)
- n=5→6: -0.012 (resumed decline)

The overall shape is a peak near n=3 followed by a non-linear decline. As the alkyl chain lengthens, alogp increases linearly (~0.39/CH2) and MW increases by ~14/CH2, compounding QED penalties.

### Fitted Model: Quadratic in QED Space
Centered at n=4 (x = n - 4), least-squares fit to all 5 data points:

**QED(x) = -0.003264·x² - 0.00909·x + 0.765189**

Fit residuals:
| n | x | Predicted | Actual | Error |
|---|---|-----------|--------|-------|
| 2 | -2 | 0.770 | 0.762 | +0.008 |
| 3 | -1 | 0.771 | 0.792 | -0.021 |
| 4 | 0 | 0.765 | 0.752 | +0.013 |
| 5 | 1 | 0.753 | 0.750 | +0.003 |
| 6 | 2 | 0.734 | 0.738 | -0.004 |

### Extrapolation to m05 (n=11, x=7)
QED = -0.003264(49) - 0.00909(7) + 0.765189
    = -0.160 - 0.064 + 0.765
    = **0.542**

### Cross-Validation with Log-Space Quadratic
ln(QED) vs x = n-4, quadratic fit:
ln(QED)(x) = -0.004293·x² - 0.01199·x - 0.26793

For x=7: ln(QED) = -0.210 - 0.084 - 0.268 = -0.562 → QED = exp(-0.562) = **0.570**

Both models agree in the 0.54–0.57 range. The quadratic in QED-space is simpler and more conservative.

### Physical Rationale
At n=11: alogp=4.956 (near Lipinski limit of 5), MW=305, rotb=12. These represent significant property violations. The accelerating decline in QED reflects compounding lipophilicity penalties in the QED formula. A prediction of ~0.54 is consistent with a borderline drug-like molecule.

**Final predicted QED for m05: 0.54**

---

## Job 2: Lead Compound Selection

### Search Strategy (5 budget units spent)
Attempted to find other congeneric series by searching for:
1. `S(=O)(=O)N` (sulfonamides) → 0 matches (1 unit)
2. `c1ccncc1` (pyridines) → 0 matches (1 unit)
3. `C(=O)O` (carboxylic acids/esters) → 0 matches (1 unit)
4. `Oc1ccc` (phenols) → 6 matches, all m05 series (1 unit)
5. `CC(=O)N` (acetamides) → 0 matches (1 unit)

### Problem
- All measured molecules are in the m05 series
- All have PSA = 38.33 (< 40 requirement)
- Budget exhausted (12/12) before finding any other series

### Measured Candidates
All measured molecules fail constraints 2 (PSA ≥ 40) and 3 (not m05 series):
| mol_id | QED | PSA | In m05 series? |
|--------|-------|------|----------------|
| m24 | 0.7919 | 38.33 | Yes |
| m15 | 0.7621 | 38.33 | Yes |
| m28 | 0.7518 | 38.33 | Yes |
| m17 | 0.7498 | 38.33 | Yes |
| m22 | 0.7377 | 38.33 | Yes |

### Selected Lead (best available)
**m24** with measured QED = 0.7919 (highest of all measured molecules).

Note: This molecule fails constraint 2 (PSA = 38.33 < 40) and constraint 3 (same series as m05). Budget exhaustion prevented discovery of molecules from other congeneric series. With additional budget, broader searches would be needed to identify series with higher-PSA scaffolds.

### Strategic Retrospective
Should have:
- Measured only 3 congeners (e.g., m24, m17, m22) instead of all 5, saving 2 units
- Used broader search terms (e.g., single heteroatom patterns like `Nc1cc`, `OCc1`, `c1cnc`)
- Prioritized finding different scaffolds over complete series characterization
