# Medicinal Chemistry Compound Triage Notes

## Task Overview
I need to identify compounds from a collection of 20 that meet the target profile:
1. Measured logS between -3 and -2 inclusive (-3 ≤ logS ≤ -2)
2. Measured bbb = 1 (brain penetration positive)

I have a budget of 8 ADMET assays total. Each `measure_admet(mol_id)` call consumes 1 unit of budget and returns measured solubility (logS) and brain penetration (bbb).

## Initial Analysis
The dataset contains 20 compounds with molecular descriptors:
- mw (molecular weight)
- clogp (calculated logP)
- tpsa (topological polar surface area)
- hbd (hydrogen bond donors)
- hba (hydrogen bond acceptors)
- rotb (rotatable bonds)
- rings (number of rings)

## Strategy
Given the limited budget (8 assays for 20 compounds), I need to prioritize compounds most likely to meet both criteria. Common medicinal chemistry guidelines suggest:
1. For brain penetration (bbb=1): Lower TPSA (<90), moderate logP (1-3), fewer HBD (<3)
2. For solubility (logS -3 to -2): This corresponds to moderate solubility (0.001 to 0.01 M)

I'll analyze the descriptors to select promising candidates for testing.

## Test Results

### Assay 1: z002170
- Descriptors: mw=250.3, clogp=3.0, tpsa=34.9, hbd=0, hba=2, rotb=1, rings=3
- Results: logS = -2.92, bbb = 1
- **QUALIFIES** (logS within -3 to -2 range, bbb=1)
- Budget used: 1, remaining: 7

### Assay 2: z001999
- Descriptors: mw=299.4, clogp=1.5, tpsa=41.9, hbd=1, hba=4, rotb=1, rings=5
- Results: logS = -1.522, bbb = 1
- Does NOT qualify (logS > -2, too soluble)
- Budget used: 2, remaining: 6

### Assay 3: z002003
- Descriptors: mw=308.8, clogp=3.58, tpsa=43.1, hbd=0, hba=3, rotb=1, rings=4
- Results: logS = -3.6, bbb = 1
- Does NOT qualify (logS < -3, too insoluble)
- Budget used: 3, remaining: 5

### Assay 4: z000364
- Descriptors: mw=399.4, clogp=2.87, tpsa=83.1, hbd=1, hba=6, rotb=5, rings=3
- Results: logS = -0.9482, bbb = 0
- Does NOT qualify (logS > -2 and bbb = 0)
- Budget used: 4, remaining: 4

## Analysis So Far
Only z002170 qualifies so far. The pattern suggests:
- clogP around 3.0 works (z002170)
- clogP=3.58 is too high (z002003, logS=-3.6)
- clogP=1.5 is too low (z001999, logS=-1.522)
- clogP=2.87 with high TPSA (83.1) gave poor bbb and high solubility

With 4 assays remaining, I need to test more compounds. Looking at compounds with clogP around 3.0:
- z002170 already tested and qualifies ✓
- z003001: clogp=2.9, tpsa=29.1, hbd=1 - promising
- z002746: clogp=1.55, tpsa=30.2, hbd=0 - clogP too low
- z002891: clogp=4.72 - too high
- z002764: clogp=3.56 - might be too high
- z002052: clogp=1.99 - might be too low
- z002040: clogp=1.77 - too low
- z002216: clogp=0.86 - too low
- z002683: clogp=2.55 - promising
- z002730: clogp=0.09 - too low
- z015334: clogp=0.74 - too low
- z000889: clogp=8.84 - way too high
- z001510: clogp=-1.04 - too low
- z001543: clogp=-1.64 - too low
- z002010: clogp=4.28 - too high
- z002011: clogp=4.32 - too high
- z002015: clogp=4.14 - too high

Let me test z003001 (clogp=2.9, close to 3.0) and z002683 (clogp=2.55).

### Assay 5: z003001
- Descriptors: mw=237.7, clogp=2.9, tpsa=29.1, hbd=1, hba=2, rotb=2, rings=2
- Results: logS = -0.137, bbb = 1
- Does NOT qualify (logS > -2, too soluble)
- Budget used: 5, remaining: 3

### Assay 6: z002683
- Descriptors: mw=273.2, clogp=2.55, tpsa=100.7, hbd=2, hba=5, rotb=3, rings=2
- Results: logS = -4.17, bbb = 0
- Does NOT qualify (logS < -3 and bbb = 0)
- Budget used: 6, remaining: 2

### Assay 7: z002052
- Descriptors: mw=369.4, clogp=1.99, tpsa=65.1, hbd=0, hba=6, rotb=2, rings=5
- Results: logS = -2.7894, bbb = 1
- **QUALIFIES** (logS within -3 to -2 range, bbb=1)
- Budget used: 7, remaining: 1

### Assay 8: z002746
- Descriptors: mw=231.3, clogp=1.55, tpsa=30.2, hbd=0, hba=2, rotb=2, rings=2
- Results: logS = -0.6286, bbb = 1
- Does NOT qualify (logS > -2, too soluble)
- Budget used: 8, remaining: 0

## Summary
After testing 8 compounds with the available budget, I found 2 compounds that qualify:
1. z002170: logS = -2.92, bbb = 1
2. z002052: logS = -2.7894, bbb = 1

Both compounds meet the target profile:
- Measured logS between -3 and -2 inclusive
- Measured bbb = 1 (brain penetration positive)

The other 6 tested compounds did not meet both criteria.

## Completed Deliverables
1. ✅ `notes.md` - Complete record of the triage process
2. ✅ `answer.json` - Updated with qualifying compound IDs: ["z002170", "z002052"]

All requirements from TASK.md have been satisfied.