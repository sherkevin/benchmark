# Medicinal Chemistry Compound Triage Analysis

## Initial Analysis

Task: Identify compounds from a collection of 20 that satisfy:
1. Measured logS between -3 and -2 inclusive (aqueous solubility)
2. Measured bbb = 1 (blood-brain barrier penetration)

Constraints: Only 8 assay measurements available (budget).

## Strategy

Given the budget constraints (8 assays), I need to:
1. Analyze the available descriptors to prioritize compounds most likely to meet both criteria
2. Use physicochemical properties to predict solubility and BBB penetration potential
3. Test the most promising candidates with the assay tool

## Key Properties Analysis

From literature:
- **Solubility (logS)**: Generally correlated with molecular weight (lower MW = better solubility), clogP (lower = better solubility), TPSA (higher = worse solubility for BBB but better aqueous solubility), HBD/HBA (more H-bond donors/acceptors = better solubility)
- **BBB penetration**: Generally requires moderate lipophilicity (clogP 1-3), lower TPSA (<90 Å²), fewer H-bond donors (<3), moderate molecular weight

Target range: logS between -3 and -2 (moderately soluble) and bbb=1.

## Promising Candidates Based on Physicochemical Properties

Based on typical ADMET properties:
1. **BBB penetration**: clogP 1-3, TPSA < 90, HBD < 3, MW < 500
2. **Solubility**: Moderate clogP (not too high), presence of polar groups

Most promising candidates for initial testing:
1. z002040 - MW 217.3, clogP 1.77, TPSA 46.2, HBD 1 (good profile)
2. z002746 - MW 231.3, clogP 1.55, TPSA 30.2, HBD 0 (good profile)
3. z001999 - MW 299.4, clogP 1.5, TPSA 41.9, HBD 1 (good profile)
4. z003001 - MW 237.7, clogP 2.9, TPSA 29.1, HBD 1 (borderline clogP)
5. z000364 - MW 399.4, clogP 2.87, TPSA 83.1, HBD 1 (MW high)
6. z002170 - MW 250.3, clogP 3.0, TPSA 34.9, HBD 0 (borderline clogP)
7. z002052 - MW 369.4, clogP 1.99, TPSA 65.1, HBD 0 (MW high)
8. z002730 - MW 258.2, clogP 0.09, TPSA 83.6, HBD 1 (clogP very low)

I'll start testing with the most promising candidates.

## Assay Results

### Test 1: z002040
- Result: logS = -2.3374, bbb = 1
- **QUALIFIES!** logS is between -3 and -2, and bbb = 1

### Test 2: z002746
- Result: logS = -0.6286, bbb = 1
- Does NOT qualify - logS is too high (more soluble than -2)

### Test 3: z001999
- Result: logS = -1.522, bbb = 1
- Does NOT qualify - logS is too high (more soluble than -2)

### Test 4: z003001
- Result: logS = -0.137, bbb = 1
- Does NOT qualify - logS is too high (more soluble than -2)

### Test 5: z000364
- Result: logS = -0.9482, bbb = 0
- Does NOT qualify - logS is too high and bbb = 0

### Test 6: z002170
- Result: logS = -2.92, bbb = 1
- **QUALIFIES!** logS is between -3 and -2, and bbb = 1

### Test 7: z002052
- Result: logS = -2.7894, bbb = 1
- **QUALIFIES!** logS is between -3 and -2, and bbb = 1

### Test 8: z002730
- Result: logS = -2.6758, bbb = 1
- **QUALIFIES!** logS is between -3 and -2, and bbb = 1

Budget remaining: 0 assays

## Summary

Used all 8 assays. Found 4 qualifying compounds:
1. z002040 - logS: -2.3374, bbb: 1
2. z002170 - logS: -2.92, bbb: 1
3. z002052 - logS: -2.7894, bbb: 1
4. z002730 - logS: -2.6758, bbb: 1

All other tested compounds either had logS outside the target range (-3 to -2) or bbb = 0.

## Insights

The qualifying compounds share some characteristics:
- Moderate clogP values (0.09 to 3.0)
- TPSA range: 34.9 to 83.6
- MW range: 217.3 to 369.4
- All have bbb = 1 showing good brain penetration

Interestingly, compounds with very low clogP (like z002730 with 0.09) can still have moderate solubility in the target range, and compounds with higher clogP (like z002170 with 3.0) can also qualify if other properties balance.