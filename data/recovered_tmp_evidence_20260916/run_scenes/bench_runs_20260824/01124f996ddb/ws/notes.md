# Notes: QED Prediction and Lead Selection

## Series Identification

**Target m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- Scaffold: `COc1ccc(cc1)C(=O)N-` (4-methoxybenzamide)
- N-alkyl chain length: 11 carbons (C11)

**Congeneric series members in the pool** (same scaffold, varying chain length):
| mol_id | Chain | SMILES | alogp | rotb |
|--------|-------|--------|-------|------|
| m15 | C2 | COc1ccc(cc1)C(=O)NCC | 1.445 | 3 |
| m24 | C3 | COc1ccc(cc1)C(=O)NCCC | 1.835 | 4 |
| m28 | C4 | COc1ccc(cc1)C(=O)NCCCC | 2.225 | 5 |
| m17 | C5 | COc1ccc(cc1)C(=O)NCCCCC | 2.615 | 6 |
| m22 | C6 | COc1ccc(cc1)C(=O)NCCCCCC | 3.005 | 7 |
| m05 | C11 | COc1ccc(cc1)C(=O)NCCCCCCCCCCC | 4.956 | 12 |

Other series in pool (different scaffolds):
- Sugar alcohol: `OCC(O)C(O)C(O)C(=O)N-` (9 members, PSA=110.02)
- Nitrobenzene: `O=[N+]([O-])c1ccc(cc1)C(=O)N-` (9 members, PSA=72.24)
- Biphenyl: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-` (9 members, PSA=29.1)

## Job 1 — QED Prediction for m05

### Measured QED values (5 measurements used):
| Chain | mol_id | QED |
|-------|--------|-----|
| C2 | m15 | 0.7621 |
| C3 | m24 | 0.7919 |
| C4 | m28 | 0.7518 |
| C5 | m17 | 0.7498 |
| C6 | m22 | 0.7377 |

### Observed trend:
QED peaks near C3 (0.7919) then steadily declines:
- C2→C3: +0.0298 (rising)
- C3→C4: -0.0401 (sharp drop)
- C4→C5: -0.0020 (plateau)
- C5→C6: -0.0121 (renewed decline)

Overall shape: inverted-U with peak near C3, then accelerating decline as chain elongates.

### Fitted model: Quadratic (least-squares, all 5 points)
**y = -0.003264·x² + 0.01702·x + 0.74933**

where x = N-alkyl chain carbon count, y = QED.

Peak at x = 2.61 (≈C3), matching the observed maximum. The negative quadratic coefficient reflects accelerating QED penalty as ALOGP→5 and RotB→12 at longer chains.

Verification at measured points:
- C2: pred 0.770, actual 0.762 (err +0.008)
- C3: pred 0.771, actual 0.792 (err -0.021)
- C4: pred 0.765, actual 0.752 (err +0.013)
- C5: pred 0.753, actual 0.750 (err +0.003)
- C6: pred 0.734, actual 0.738 (err -0.004)

### Extrapolation to C11 (m05):
y(11) = -0.003264×121 + 0.01702×11 + 0.74933
      = -0.3949 + 0.1872 + 0.7493
      = **0.54**

Physical justification: m05 has ALOGP=4.956 (near Lipinski limit of 5) and RotB=12 (well above ideal ~3-5), both heavily penalized by QED's desirability functions. The accelerating decline in the quadratic captures the compounding penalties.

## Job 2 — Lead Compound Selection

### Pre-filtering (no measurements needed):
- **PSA ≥ 40**: Biphenyl series (PSA=29.1) eliminated entirely. Sugar alcohols (110.02) and nitrobenzenes (72.24) pass.
- **Not methoxybenzamide**: Eliminates m15, m24, m28, m17, m22.

### Candidates measured (7 measurements used):
| mol_id | Series | Chain | PSA | QED | QED ≥ 0.6? |
|--------|--------|-------|-----|-----|------------|
| m09 | nitro | C2 | 72.24 | 0.5826 | No |
| m02 | nitro | C3 | 72.24 | 0.6046 | **Yes** ✓ |
| m12 | nitro | C4 | 72.24 | 0.4709 | No |
| m32 | nitro | C5 | 72.24 | 0.4683 | No |
| m33 | nitro | C6 | 72.24 | 0.4591 | No |
| m14 | sugar | C2 | 110.02 | 0.3237 | No |
| m27 | sugar | C3 | 110.02 | 0.3330 | No |

### Chosen lead: **m02**
- SMILES: `O=[N+]([O-])c1ccc(cc1)C(=O)NCCC`
- Measured QED: **0.6046** ≥ 0.6 ✓
- PSA: **72.24** ≥ 40 ✓
- Nitrobenzene series — not methoxybenzamide ✓

Why m02 works: It hits the sweet spot in the nitrobenzene series — short C3 chain gives moderate ALOGP (1.735), low RotB (4), and reasonable MW (208). The sugar alcohols all score poorly due to very high PSA (110) and negative logP. Longer nitrobenzene chains see QED decline as logP and RotB increase.

## Budget Summary
Total measurements used: 12/12
- Job 1 (methoxybenzamide series): 5 (m15, m24, m28, m17, m22)
- Job 2 (lead search): 7 (m09, m02, m12, m32, m33, m14, m27)
