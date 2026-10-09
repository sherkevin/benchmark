# Task Notes

## Objective
Find compounds that qualify based on:
1. measured logS between -3 and -2 (inclusive)
2. measured bbb = 1 (blood-brain barrier penetration)

Budget: 8 assays total

## Initial Analysis
There are 20 compounds in the dataset. I need to strategically select which compounds to test based on their properties to maximize my chances of finding qualifying compounds within the budget.

Based on known medicinal chemistry rules (like Lipinski's Rule of Five), compounds with:
- clogP between 1-5 tend to have better drug-like properties
- TPSA values can indicate solubility potential
- BBB penetration is often favored by lower molecular weight and moderate lipophilicity

Let me prioritize compounds that might meet our criteria.

## Testing Results
Testing results so far:
- z001999: logS = -1.522, bbb = 1 → logS too high (should be between -3 and -2)
- z002003: logS = -3.6, bbb = 1 → logS too low (below -3)
- z002015: logS = -3.82, bbb = 1 → logS too low (below -3) 
- z002040: logS = -2.3374, bbb = 1 → QUALIFIES! (between -3 and -2)
- z002052: logS = -2.7894, bbb = 1 → QUALIFIES! (between -3 and -2)
- z002170: logS = -2.92, bbb = 1 → QUALIFIES! (between -3 and -2)
- z002216: logS = -2.16, bbb = 1 → QUALIFIES! (between -3 and -2)
- z002683: logS = -4.17, bbb = 0 → doesn't qualify (bbb=0 and logS too low)

Final results: z002040, z002052, z002170, and z002216 are the qualifying compounds.
All 8 measurements in budget have been used.