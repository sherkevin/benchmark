# Notes

## Task 1: Predict QED of m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC)

### Scaffold identification
m05 belongs to scaffold 1: COc1ccc(cc1)C(=O)N (methoxybenzamide)
Series members: m15(chain 2), m24(chain 3), m28(chain 4), m17(chain 5), m22(chain 6), m05(chain 11)

### Measurements (5 used)
| mol_id | chain | QED    |
|--------|-------|--------|
| m15    | 2     | 0.7621 |
| m24    | 3     | 0.7919 |
| m28    | 4     | 0.7518 |
| m17    | 5     | 0.7498 |
| m22    | 6     | 0.7377 |

### Trend analysis
- QED peaks at chain 3 (0.7919), then steadily declines
- Main driver of decline: ROTB desirability drops sharply (0.99 to 0.53 from chain 2 to 6)
- MW desirability increases (0.33 to 0.68) - favorable
- ALOGP desirability increases then plateaus near 1.0
- HBA, HBD, PSA, AROM constant; ALERTS drops at chain 4

### Polynomial fits for m05 (chain 11)
- Degree 1 (all 5 pts): QED ~ 0.695 (RMSE=0.0131)
- Degree 2 (all 5 pts): QED ~ 0.5416 (RMSE=0.0119) -- best fit
- Degree 3 (all 5 pts): QED ~ 2.13 -- nonsense, overfitting

### Component-based cross-check
For m05: MW=305.46 (d~0.90), ALOGP=4.956 (d~0.68), ROTB=12 (d~0.02-0.10)
Using QED weighted formula gives QED ~ 0.48-0.61, midpoint ~0.54
Consistent with degree-2 prediction.

### Prediction: QED ~ 0.5416

---

## Task 2: Pick lead molecule

### Constraints
1. QED >= 0.6
2. PSA >= 40
3. Not in scaffold 1 (m05's series)

### Candidates by scaffold PSA
- Scaffold 2 (nitrobenzamide): PSA=72.24 YES
- Scaffold 3 (sugar-like): PSA=110.02 YES
- Scaffold 4 (biphenyl): PSA=29.1 NO (below 40)

### Measurements
| mol_id | scaffold | PSA     | QED    | Pass? |
|--------|----------|---------|--------|-------|
| m02    | 2        | 72.24   | 0.6046 | YES   |
| m12    | 2        | 72.24   | 0.4709 | NO    |
| m01    | 3        | 110.02  | 0.3187 | NO    |
| m03    | 3        | 110.02  | 0.3282 | NO    |

### Lead: m02 (QED=0.6046, PSA=72.24, scaffold 2 != scaffold 1)

---

## Budget used: 9/12 measurements
