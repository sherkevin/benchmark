# Notes

## Task Summary
- 20 compounds in candidates.tsv
- Budget: 8 assays (measure_admet calls)
- Qualifying criteria: logS between -3 and -2 (inclusive) AND bbb = 1

## Strategy
Looking at the compounds, I need to prioritize those most likely to have:
1. logS between -3 and -2 (moderate solubility)
2. bbb = 1 (brain penetration)

For BBB penetration, generally:
- Lower TPSA (< 90) is favorable
- Moderate clogP (2-5) is favorable
- Fewer HBD is favorable

For solubility in the -3 to -2 range:
- Very hydrophilic compounds (low clogP, high TPSA) tend to have higher logS (less negative)
- Very lipophilic compounds (high clogP) tend to have lower logS (more negative)

Let me analyze each compound:

| mol_id | clogP | TPSA | HBD | Notes |
|--------|-------|------|-----|-------|
| z000364 | 2.87 | 83.1 | 1 | Moderate clogP, moderate TPSA - could be good |
| z000889 | 8.84 | 29.5 | 1 | Very high clogP - likely low logS |
| z001510 | -1.04 | 72.7 | 1 | Low clogP - likely high logS |
| z001543 | -1.64 | 119.6 | 5 | Very low clogP, high TPSA - likely high logS, poor BBB |
| z001999 | 1.5 | 41.9 | 1 | Moderate clogP, low TPSA - good BBB candidate |
| z002003 | 3.58 | 43.1 | 0 | Good clogP, low TPSA - good BBB candidate |
| z002010 | 4.28 | 29.5 | 0 | Good clogP, low TPSA - good BBB candidate |
| z002011 | 4.32 | 30.2 | 0 | Good clogP, low TPSA - good BBB candidate |
| z002015 | 4.14 | 23.6 | 0 | Good clogP, very low TPSA - good BBB candidate |
| z002040 | 1.77 | 46.2 | 1 | Moderate clogP, moderate TPSA - could be good |
| z002052 | 1.99 | 65.1 | 0 | Moderate clogP, moderate TPSA - could be good |
| z002170 | 3.0 | 34.9 | 0 | Good clogP, low TPSA - good BBB candidate |
| z002216 | 0.86 | 98.0 | 2 | High TPSA - poor BBB likely |
| z002683 | 2.55 | 100.7 | 2 | High TPSA - poor BBB likely |
| z002730 | 0.09 | 83.6 | 1 | Low clogP - likely high logS |
| z002746 | 1.55 | 30.2 | 0 | Moderate clogP, low TPSA - good BBB candidate |
| z002764 | 3.56 | 66.8 | 2 | Moderate clogP - could be good |
| z002891 | 4.72 | 34.1 | 0 | Higher clogP - might have low logS |
| z003001 | 2.9 | 29.1 | 1 | Good clogP, low TPSA - good BBB candidate |
| z015334 | 0.74 | 81.6 | 1 | Low clogP - likely high logS |

Best candidates for BBB (low TPSA, moderate clogP, 0 HBD):
- z002003, z002010, z002011, z002015, z002170, z002746, z003001

For solubility in range -3 to -2, I want moderate lipophilicity.

Let me select 8 compounds that balance these properties:
1. z001999 - clogP 1.5, TPSA 41.9 - moderate
2. z002003 - clogP 3.58, TPSA 43.1 - good balance
3. z002010 - clogP 4.28, TPSA 29.5 - might be too lipophilic
4. z002011 - clogP 4.32, TPSA 30.2 - might be too lipophilic
5. z002015 - clogP 4.14, TPSA 23.6 - might be too lipophilic
6. z002040 - clogP 1.77, TPSA 46.2 - moderate
7. z002052 - clogP 1.99, TPSA 65.1 - moderate
8. z002170 - clogP 3.0, TPSA 34.9 - good balance
9. z002746 - clogP 1.55, TPSA 30.2 - moderate
10. z002764 - clogP 3.56, TPSA 66.8 - moderate
11. z003001 - clogP 2.9, TPSA 29.1 - good balance

I'll start with 8 compounds that seem most promising:

## Assay Results (8/8 budget used)

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z001999 | -1.522 | 1 | NO (logS > -2) |
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002010 | -5.0097 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002170 | -2.92 | 1 | **YES** |
| z002746 | -0.6286 | 1 | NO (logS > -2) |
| z003001 | -0.137 | 1 | NO (logS > -2) |

## Conclusion

Only **z002170** qualifies:
- logS = -2.92 (within -3 to -2 range)
- bbb = 1

All other compounds either had logS too high (too soluble) or too low (not soluble enough).
