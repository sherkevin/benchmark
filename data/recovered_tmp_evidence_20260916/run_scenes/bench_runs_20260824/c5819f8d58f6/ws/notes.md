# Compound Analysis Notes

## Task Overview
- Need to find compounds with:
  - logS between -3 and -2 (inclusive)
  - bbb = 1
- Budget: 8 assays total

## Candidate Analysis
Initial review of candidates.tsv to identify promising compounds based on descriptors.

Compounds with potentially favorable properties (moderate molecular weight and clogP):
- z001510: mw=180.2, clogp=-1.04 (might have good solubility)
- z001543: mw=286.3, clogp=-1.64 (low lipophilicity, might be too hydrophilic for BBB)
- z001999: mw=299.4, clogp=1.5 (good MW and moderate lipophilicity)
- z002003: mw=339.5, clogp=4.28 (high lipophilicity, might have low solubility)
- z002015: mw=336.5, clogp=4.14 (high lipophilicity)
- z002040: mw=217.3, clogp=1.77 (good MW, moderate lipophilicity)
- z002052: mw=369.4, clogp=1.99 (moderate properties)
- z002170: mw=250.3, clogp=3.0 (moderate properties)
- z002216: mw=250.3, clogp=0.86 (low lipophilicity)
- z002683: mw=273.2, clogp=2.55 (moderate properties)
- z002746: mw=231.3, clogp=1.55 (good MW and moderate lipophilicity)
- z002764: mw=318.3, clogp=3.56 (moderate-high lipophilicity)
- z003001: mw=237.7, clogp=2.9 (moderate properties)

Based on Rule of Five considerations and likely brain penetration (need moderate lipophilicity, reasonable MW), I'll prioritize testing:
1. z001999 - good MW and clogP
2. z002040 - good MW and clogP
3. z002170 - good MW and moderate clogP
4. z002746 - good MW and moderate clogP
5. z003001 - moderate MW and clogP
6. z002683 - moderate MW and clogP
7. z002216 - lower lipophilicity but might have good solubility
8. z001510 - polar compound but worth checking solubility

## Experimental Results
1. z001999: logS = -1.522, bbb = 1 → Does not qualify (logS too high)
2. z002040: logS = -2.3374, bbb = 1 → Qualifies! ✓
3. z002170: logS = -2.92, bbb = 1 → Qualifies! ✓
4. z002746: logS = -0.6286, bbb = 1 → Does not qualify (logS too high)
5. z003001: logS = -0.137, bbb = 1 → Does not qualify (logS too high)
6. z002683: logS = -4.17, bbb = 0 → Does not qualify (logS too low and no BBB penetration)
7. z002216: logS = -2.16, bbb = 1 → Qualifies! ✓
8. z001510: logS = -2.7372, bbb = 1 → Qualifies! ✓

## Summary
Four compounds met the qualification criteria:
- z002040: logS = -2.3374 (between -3 and -2), bbb = 1
- z002170: logS = -2.92 (between -3 and -2), bbb = 1
- z002216: logS = -2.16 (between -3 and -2), bbb = 1
- z001510: logS = -2.7372 (between -3 and -2), bbb = 1