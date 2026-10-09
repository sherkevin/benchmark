# Notes

## Task Analysis
- Need to find a benzamide with QED ≥ 0.85, no structural alerts, in the answer library
- Lead family: N-alkyl-4-substituted benzamides (C2-C8 chains, various para substituents)
- 12 measurement budget, 10 check attempts

## Measurement Results (all 12 used)

| SMILES | MW | LogP | PSA | ROTB | QED |
|--------|------|-------|-------|------|-------|
| CCNC(=O)c1ccccc1 | 149.19 | 1.436 | 29.1 | 2 | 0.6769 |
| CCCNC(=O)c1ccccc1 | 163.22 | 1.826 | 29.1 | 3 | 0.7232 |
| CCCCNC(=O)c1ccccc1 | 177.25 | 2.216 | 29.1 | 4 | 0.7018 |
| CCCCCNC(=O)c1ccccc1 | 191.27 | 2.607 | 29.1 | 5 | 0.7121 |
| CCNC(=O)c1ccc(F)cc1 | 167.18 | 1.575 | 29.1 | 2 | 0.7114 |
| CCCCNC(=O)c1ccc(F)cc1 | 195.24 | 2.356 | 29.1 | 4 | 0.7340 |
| CCCCCNC(=O)c1ccc(F)cc1 | 209.26 | 2.746 | 29.1 | 5 | 0.7421 |
| CCCCCCNC(=O)c1ccc(F)cc1 | 223.29 | 3.136 | 29.1 | 6 | 0.7374 |
| CCCCNC(=O)c1ccc(C#N)cc1 | 202.26 | 2.088 | 52.89 | 4 | 0.7599 |
| CCCCCNC(=O)c1ccc(C#N)cc1 | 216.28 | 2.478 | 52.89 | 5 | 0.7686 |
| CCNC(=O)c1ccc(C)cc1 | 163.22 | 1.745 | 29.1 | 2 | 0.7066 |
| CCCCCNC(=O)c1ccc(OC)cc1 | 221.30 | 2.615 | 38.33 | 6 | 0.7498 |

## Key Patterns
1. **Higher PSA → higher QED**: CN (PSA 52.89) > OMe (38.33) > F/H (29.1)
2. **CN leads give highest QED**: 0.76-0.77 (vs ~0.68-0.74 for others)
3. **Moderate chain length (C4-C5) optimal**: balances MW, LogP, and ROTB
4. **Best lead**: CCCCCNC(=O)c1ccc(C#N)cc1 at QED 0.769

## Design Rationale
To push QED from ~0.77 to ≥0.85, need major improvements:
- **Replace flexible pentyl chain with morpholine ring**: Reduces ROTB from 5→1, adds HBA (morpholine O), increases PSA
- **Keep CN para-substituent**: Maintains higher PSA contribution
- **No structural alerts**: CN and morpholine are clean functional groups

## Proposed Molecule
**O=C(N1CCOCC1)c1ccc(C#N)cc1** — (4-cyanophenyl)(morpholin-4-yl)methanone

Estimated properties:
- MW: ~216 (same as C5-CN lead)
- LogP: ~1.2 (morpholine reduces lipophilicity)
- PSA: ~57 (amide + morpholine O + CN)
- HBA: 4, HBD: 0
- ROTB: 1 (vs 5 in the lead)
- AROM: 1
- ALERTS: 0

Expected QED ≥ 0.85 due to dramatically lower ROTB and improved polarity balance.

## Existence Checks (all 10 used, none found)
Tried: morpholine amides, piperazine amides, cyclohexyl amides, diphenyl amides, acetanilides, moclobemide variants. The library may use different SMILES representations or contain specific compounds I couldn't guess.

## Final Proposal
- **Proposed**: O=C(N1CCOCC1)c1ccc(C#N)cc1
- **Anchor**: CCCCCNC(=O)c1ccc(C#N)cc1 (QED 0.769, measured)
- **Edit**: Replace N-pentyl chain with morpholine ring
