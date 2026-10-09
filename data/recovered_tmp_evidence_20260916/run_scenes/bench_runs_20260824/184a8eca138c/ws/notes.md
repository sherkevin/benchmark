# Compound Triage Notes

## Task Overview
- Need to find compounds with:
  - logS between -3 and -2 (inclusive)
  - bbb = 1
- Budget: 8 assays
- Total compounds to screen: 20

## Initial Analysis
The dataset contains 20 compounds with various properties. Based on Rule of Five and general ADMET properties:
- Compounds with high molecular weight (>500), high clogP (>5), high TPSA (>140), many HBD (>5) or HBA (>10) are often problematic
- However, since we need specific solubility and BBB data, I'll prioritize compounds that seem most likely to meet our criteria

## Assay Results

### Round 1 Testing
Tested z002010: logS = -5.01, bbb = 1
- Does NOT qualify (logS too low, outside -3 to -2 range)

Tested z001999: logS = -1.522, bbb = 1
- Does NOT qualify (logS too high, outside -3 to -2 range)

Tested z002003: logS = -3.6, bbb = 1
- Does NOT qualify (logS too low, outside -3 to -2 range)

Tested z002011: logS = -4.13, bbb = 1
- Does NOT qualify (logS too low, outside -3 to -2 range)

Tested z002015: logS = -3.82, bbb = 1
- Does NOT qualify (logS too low, outside -3 to -2 range)

Tested z002040: logS = -2.3374, bbb = 1
- QUALIFIES! (logS between -3 and -2, bbb = 1)

Tested z002170: logS = -2.92, bbb = 1
- QUALIFIES! (logS between -3 and -2, bbb = 1)

Tested z002216: logS = -2.16, bbb = 1
- QUALIFIES! (logS between -3 and -2, bbb = 1)

Remaining budget: 0 (exhausted)

## Summary
Found 3 qualifying compounds that meet both criteria:
- logS between -3 and -2 (inclusive)
- bbb = 1