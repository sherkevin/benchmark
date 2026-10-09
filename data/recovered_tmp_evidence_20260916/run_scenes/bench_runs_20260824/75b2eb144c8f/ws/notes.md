# ADMET Compound Triage Notes

## Task Overview
- 20 compounds in data/candidates.tsv
- Need to identify compounds with:
  1. Measured logS between -3 and -2 inclusive
  2. Measured bbb = 1
- Only 8 assay measurements available (budget constraint)
- Must report confirmed qualifying compounds in answer.json

## Initial Analysis
The compounds have various descriptors:
- mw (molecular weight)
- clogp (calculated logP - lipophilicity)
- tpsa (topological polar surface area)
- hbd/hba (hydrogen bond donors/acceptors)
- rotb (rotatable bonds)
- rings (number of rings)

For brain penetration (bbb = 1), compounds typically need:
- Lower molecular weight (<500)
- Moderate lipophilicity (clogp ~1-3)
- Lower polar surface area (tpsa < ~90 Å²)
- Limited hydrogen bond donors (<3-5)

For solubility (logS between -3 and -2):
- Higher solubility corresponds to more negative logS values
- logS -3 = 0.001 M solubility
- logS -2 = 0.01 M solubility
- Compounds with moderate hydrophilicity might fall in this range

## Strategy
Since I only have 8 assays, I need to prioritize compounds most likely to meet both criteria:
1. Likely brain penetration candidates (bbb=1)
2. Likely moderate solubility (logS -3 to -2)

## Candidate Prioritization
Based on typical ADMET properties:

For BBB penetration (bbb=1):
- Prefer clogp ~ 1-3 (moderate lipophilicity)
- Prefer tpsa < 90 Å² (lower polar surface area)
- Prefer hbd ≤ 2 (fewer hydrogen bond donors)

For solubility (logS -3 to -2):
- Need moderate solubility - neither too hydrophobic nor too hydrophilic
- clogp extremes likely give logS outside range
- Very low clogp → very soluble (logS < -3)
- Very high clogp → poorly soluble (logS > -2)

Promising candidates based on descriptors:
1. z000364: clogp=2.87, tpsa=83.1, hbd=1 (good BBB potential, moderate solubility?)
2. z001999: clogp=1.5, tpsa=41.9, hbd=1 (excellent BBB potential, solubility?)
3. z002003: clogp=3.58, tpsa=43.1, hbd=0 (good BBB, maybe too hydrophobic?)
4. z002010: clogp=4.28, tpsa=29.5, hbd=0 (good BBB, maybe too hydrophobic?)
5. z002040: clogp=1.77, tpsa=46.2, hbd=1 (good BBB potential)
6. z002052: clogp=1.99, tpsa=65.1, hbd=0 (good BBB potential)
7. z002170: clogp=3.0, tpsa=34.9, hbd=0 (good BBB potential)
8. z002730: clogp=0.09, tpsa=83.6, hbd=1 (maybe too hydrophilic for target solubility)
9. z002746: clogp=1.55, tpsa=30.2, hbd=0 (good BBB potential)
10. z002764: clogp=3.56, tpsa=66.8, hbd=2 (borderline)
11. z003001: clogp=2.9, tpsa=29.1, hbd=1 (good BBB potential)

I'll start testing the most promising candidates.

## Assay Results

### Testショ1: z001999
- logS: -1.522 (too soluble, outside target range of -3 to -2)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** - logS outside range

### Test 2: z000364
- logS: -0.9482 (even more soluble)
- bbb: 0 (no brain penetration)
- **Result: Does NOT qualify** - fails both criteria

### Test 3: z002170
- logS: -2.92 (within target range -3 to -2!)
- bbb: 1 (good brain penetration)
- **Result: QUALIFIES!** Meets both criteria

### Test 4: z002003
- logS: -3.6 (too low solubility, outside target range)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** - logS too low

### Test 5: z003001
- logS: -0.137 (very soluble)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** - logS too high

### Test 6: z002746
- logS: -0.6286 (soluble)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** - logS too high

### Test 7: z002010
- logS: -5.0097 (very insoluble)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** - logS too low

### Test 8: z002040
- logS: -2.3374 (within target range -3 to -2!)
- bbb: 1 (good brain penetration)
- **Result: QUALIFIES!** Meets both criteria

## Summary
Tested 8 compounds (budget exhausted):
1. z001999: logS=-1.522, bbb=1 ❌ (too soluble)
2. z000364: logS=-0.9482, bbb=0 ❌ (fails both)
3. z002170: logS=-2.92, bbb=1 ✅ QUALIFIES
4. z002003: logS=-3.6, bbb=1 ❌ (too insoluble)
5. z003001: logS=-0.137, bbb=1 ❌ (too soluble)
6. z002746: logS=-0.6286, bbb=1 ❌ (too soluble)
7. z002010: logS=-5.0097, bbb=1 ❌ (too insoluble)
8. z002040: logS=-2.3374, bbb=1 ✅ QUALIFIES

## Qualifying Compounds (2):
1. z002170: clogp=3.0, tpsa=34.9, hbd=0
2. z002040: clogp=1.77, tpsa=46.2, hbd=1

Both compounds meet the strict criteria: logS between -3 and -2 inclusive, and bbb=1.

## Final Answer
Found 2 qualifying compounds:
1. **z002170** - logS: -2.92, bbb: 1
2. **z002040** - logS: -2.3374, bbb: 1

Both have been confirmed through `measure_admet` assays and are reported in `answer.json`.

**Budget**: Used all 8 assays as required.