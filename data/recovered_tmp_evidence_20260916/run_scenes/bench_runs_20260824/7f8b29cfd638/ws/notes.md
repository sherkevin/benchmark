# Compound Screening Notes

## Task Overview
We have 20 compounds to screen for:
1. Aqueous solubility (logS) between -3 and -2 inclusive
2. Blood-brain-barrier penetration (bbb = 1)

We have a budget of 8 assays total (8 measurement calls).

## Initial Analysis
Looking at the compounds' properties from the catalogue:
- Molecular weight (mw)
- Calculated logP (clogp)
- Topological polar surface area (tpsa)
- Hydrogen bond donors (hbd)
- Hydrogen bond acceptors (hba)
- Rotatable bonds (rotb)
- Number of rings (rings)

## Strategy
Since we only have 8 assays, we need to be selective. For BBB penetration, compounds typically need:
- Lower molecular weight (<500)
- Moderate logP (typically 1-3)
- Lower polar surface area (<90 Å²)
- Fewer hydrogen bond donors (<3)

For solubility (logS between -3 and -2), we need compounds with moderate solubility.

## Compound Analysis

Let me analyze compounds based on typical BBB penetration rules:

**BBB-favorable compounds** (based on typical rules):
- z000364: mw=399.4, clogp=2.87, tpsa=83.1, hbd=1 - Reasonable candidate
- z001510: mw=180.2, clogp=-1.04, tpsa=72.7, hbd=1 - Good size but low logP
- z001999: mw=299.4, clogp=1.5, tpsa=41.9, hbd=1 - Good candidate
- z002003: mw=308.8, clogp=3.58, tpsa=43.1, hbd=0 - Good candidate
- z002011: mw=325.8, clogp=4.32, tpsa=30.2, hbd=0 - High logP but good tpsa
- z002040: mw=217.3, clogp=1.77, tpsa=46.2, hbd=1 - Good candidate
- z002170: mw=250.3, clogp=3.0, tpsa=34.9, hbd=0 - Good candidate
- z002746: mw=231.3, clogp=1.55, tpsa=30.2, hbd=0 - Good candidate
- z003001: mw=237.7, clogp=2.9, tpsa=29.1, hbd=1 - Good candidate

**BBB-unfavorable compounds** (based on typical rules):
- z000889: mw=430.7, clogp=8.84, tpsa=29.5 - Too high MW and logP
- z001543: mw=286.3, clogp=-1.64, tpsa=119.6, hbd=5 - Too many HBD, high tpsa
- z002010: mw=339.5, clogp=4.28, tpsa=29.5, hbd=0 - High logP but okay otherwise
- z002015: mw=336.5, clogp=4.14, tpsa=23.6, hbd=0 - High logP
- z002052: mw=369.4, clogp=1.99, tpsa=65.1, hbd=0 - Reasonable but higher MW
- z002216: mw=250.3, clogp=0.86, tpsa=98.0, hbd=2 - High tpsa
- z002683: mw=273.2, clogp=2.55, tpsa=100.7, hbd=2 - High tpsa
- z002730: mw=258.2, clogp=0.09, tpsa=83.6, hbd=1 - Low logP, high tpsa
- z002764: mw=318.3, clogp=3.56, tpsa=66.8, hbd=2 - Moderate
- z002891: mw=314.5, clogp=4.72, tpsa=34.1, hbd=0 - High logP
- z015334: mw=306.3, clogp=0.74, tpsa=81.6, hbd=1 - Moderate

Given the budget of 8 assays, I'll start testing the most promising candidates.

## Testing Results

### Test 1: z001999
- Properties: mw=299.4, clogp=1.5, tpsa=41.9, hbd=1
- Measured: logS = -1.522, bbb = 1
- Analysis: bbb = 1 is good, but logS = -1.522 is greater than -2, so NOT in required range (-3 to -2 inclusive)
- Status: **Does NOT qualify**

Remaining budget: 7 assays

Next, I'll test compounds that might have lower solubility (more hydrophobic) to get logS in the -3 to -2 range.

### Test 2: z002003
- Properties: mw=308.8, clogp=3.58, tpsa=43.1, hbd=0
- Measured: logS = -3.6, bbb = 1
- Analysis: bbb = 1 is good, but logS = -3.6 is less than -3, so NOT in required range (-3 to -2 inclusive)
- Status: **Does NOT qualify**

Remaining budget: 6 assays

I need compounds with moderate hydrophobicity - not too soluble (logS > -2) and not too insoluble (logS < -3). Maybe clogp around 2-3 might work.

### Test 3: z002040
- Properties: mw=217.3, clogp=1.77, tpsa=46.2, hbd=1
- Measured: logS = -2.3374, bbb = 1
- Analysis: logS = -2.3374 is between -3 and -2, and bbb = 1
- Status: **QUALIFIES!**

Remaining budget: 5 assays

First qualifying compound found! Let me update answer.json and continue testing.

### Test 4: z003001
- Properties: mw=237.7, clogp=2.9, tpsa=29.1, hbd=1
- Measured: logS = -0.137, bbb = 1
- Analysis: bbb = 1 is good, but logS = -0.137 is greater than -2
- Status: **Does NOT qualify**

Remaining budget: 4 assays

Let me try z000364 which has clogp=2.87, similar to z002040 which qualified.

### Test 5: z000364
- Properties: mw=399.4, clogp=2.87, tpsa=83.1, hbd=1
- Measured: logS = -0.9482, bbb = 0
- Analysis: logS = -0.9482 is greater than -2, and bbb = 0
- Status: **Does NOT qualify** (fails both criteria)

Remaining budget: 3 assays

Let me try z002170 which has clogp=3.0, tpsa=34.9, hbd=0 - good BBB properties.

### Test 6: z002170
- Properties: mw=250.3, clogp=3.0, tpsa=34.9, hbd=0
- Measured: logS = -2.92, bbb = 1
- Analysis: logS = -2.92 is between -3 and -2, and bbb = 1
- Status: **QUALIFIES!**

Remaining budget: 2 assays

Second qualifying compound found! Let me try z002746 which has clogp=1.55.

### Test 7: z002746
- Properties: mw=231.3, clogp=1.55, tpsa=30.2, hbd=0
- Measured: logS = -0.6286, bbb = 1
- Analysis: bbb = 1 is good, but logS = -0.6286 is greater than -2
- Status: **Does NOT qualify**

Remaining budget: 1 assay

Last assay available. Let me try z002011 which has higher clogp=4.32 - might have lower solubility.

### Test 8: z002011
- Properties: mw=325.8, clogp=4.32, tpsa=30.2, hbd=0
- Measured: logS = -4.13, bbb = 1
- Analysis: bbb = 1 is good, but logS = -4.13 is less than -3
- Status: **Does NOT qualify**

Remaining budget: 0 assays

## Summary
Used all 8 assays. Found 2 qualifying compounds:
1. z002040 - logS = -2.3374, bbb = 1
2. z002170 - logS = -2.92, bbb = 1

These have been added to answer.json.