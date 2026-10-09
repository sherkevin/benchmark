# Compound Triage Notes

## Task Overview
- Need to find compounds with:
  - logS between -3 and -2 (inclusive)
  - bbb = 1
- Budget: 8 assays total
- Total compounds to evaluate: 20

## Initial Strategy
Based on the molecular properties in the TSV file, I'll prioritize compounds that might meet our criteria:
- Low to moderate molecular weight (likely better solubility)
- Lower clogP values (hydrophilic compounds tend to have better solubility)
- Higher TPSA (polar surface area often correlates with solubility)

## Assay Results

Actual testing results:

1. z001999: logS = -1.522, bbb = 1 → logS too high (above -2)
2. z002003: logS = -3.6, bbb = 1 → logS too low (below -3)
3. z002015: logS = -3.82, bbb = 1 → logS too low (below -3)
4. z002170: logS = -2.92, bbb = 1 → QUALIFIES! (logS between -3 and -2, bbb = 1)
5. z002011: logS = -4.13, bbb = 1 → logS too low (below -3)
6. z002891: logS = -4.5526, bbb = 0 → bbb = 0 (does not qualify)
7. z002010: logS = -5.0097, bbb = 1 → logS too low (below -3)
8. z002764: logS = -2.9008, bbb = 0 → bbb = 0 (does not qualify)

## Summary
Out of 8 assays performed with the budget:
- Only 1 compound qualified: z002170 with logS = -2.92 and bbb = 1
- All other compounds either had logS values outside the -3 to -2 range or bbb = 0