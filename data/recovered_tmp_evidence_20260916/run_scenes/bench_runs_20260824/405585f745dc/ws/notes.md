# Medicinal Chemistry Compound Triage Notes

## Task Overview
I need to identify compounds that satisfy both:
1. Measured aqueous solubility (logS) between -3 and -2 inclusive
2. Measured blood-brain-barrier penetration (bbb) = 1

I have 20 compounds in `data/candidates.tsv` and a budget of 8 ADMET assays (calls to `measure_admet`).

## Analysis Plan
1. First, I'll analyze the available descriptors to identify promising candidates
2. I'll use the budget strategically to test compounds most likely to qualify
3. I'll document all measurements and reasoning

## Compound Descriptors Analysis
From the data, I can see compounds have:
- Molecular weight (mw)
- Calculated logP (clogp)
- Topological polar surface area (tpsa)
- Hydrogen bond donors (hbd)
- Hydrogen bond acceptors (hba)
- Rotatable bonds (rotb)
- Number of rings (rings)

## Selection Strategy
Since I only have 8 assays, I need to prioritize compounds that are most likely to:
1. Have moderate solubility (logS between -3 and -2)
2. Cross the blood-brain-barrier (bbb=1)

General guidelines for BBB penetration:
- Lower molecular weight (<450 Da)
- Lower polar surface area (<90 Å²)
- Moderate lipophilicity (clogp ~1-3)
- Limited hydrogen bond donors (<3)

General guidelines for solubility:
- Moderate clogp (not too high, not too low)
- Reasonable hydrogen bonding capacity

## Assay Results

### Assay 1: z002040
- **logS**: -2.3374 ✓ (within -3 to -2)
- **bbb**: 1 ✓
- **Result**: QUALIFIES
- **Budget used**: 1 of 8

This confirms that compounds with moderate properties (mw=217.3, clogp=1.77, tpsa=46.2, hbd=1) can meet criteria.

Next targets: I'll test compounds with similar profiles.

### Assay 2: z002746
- **logS**: -0.6286 (too high, outside -3 to -2 range)
- **bbb**: 1 ✓
- **Result**: DOES NOT QUALIFY
- **Budget used**: 2 of 8

### Assay264
- **logS**: -2.92 ✓ (within -3 to -2)
- **bbb**: 1 ✓
- **Result**: QUALIFIES
- **Budget used**: 3 of 8

### Assay 4: z003001
- **logS**: -0.137 (too high, outside -3 to -2 range)
- **bbb**: 1 ✓
- **Result**: DOES NOT QUALIFY
- **Budget used**: 4 of 8

## Current Status
- **Qualifying compounds found**: 2 (z002040, z002170)
- **Assays remaining**: 4
- **Compounds remaining to test**: 16

## Analysis of Results So Far
Looking at the pattern:
- z002040 (qualifies): mw=217.3, clogp=1.77, tpsa=46.2, hbd=1
- z002170 (qualifies): mw=250.3, clogp=3.0, tpsa=34.9, hbd=0
- z002746 (fails): mw=231.3, clogp=1.55, tpsa=30.2, hbd=0 - logS too high (-0.6286)
- z003001 (fails): mw=237.7, clogp=2.9, tpsa=29.1, hbd=1 - logS too high (-0.137)

The compounds that failed had logS values that were too high (better solubility than required). I need compounds with slightly poorer solubility (logS between -3 and -2).

Next strategy: Look for compounds with slightly higher clogp or other features that might reduce solubility slightly.

### Assay 5: z002003
- **logS**: -3.6 (too low, outside -3 to -2 range)
- **bbb**: 1 ✓
- **Result**: DOES NOT QUALIFY
- **Budget used**: 5 of 8

### Assay 6: z002052
- **logS**: -2.7894 ✓ (within -3 to -2)
- **bbb**: 1 ✓
- **Result**: QUALIFIES
- **Budget used**: 6 of 8

## Current Status
- **Qualifying compounds found**: 3 (z002040, z002170, z002052)
- **Assays remaining**: 2
- **Compounds remaining to test**: 14

## Analysis of Results So Far
Pattern emerging:
- Qualifying compounds: z002040 (clogp=1.77), z002170 (clogp=3.0), z002052 (clogp=1.99)
- Failed compounds: z002746 (clogp=1.55, logS=-0.6286), z003001 (clogp=2.9, logS=-0.137), z002003 (clogp=3.58, logS=-3.6)

The "sweet spot" for logS (-3 to -2) seems to correspond to clogp around 1.8-3.0, but there's variability.

With 2 assays remaining, I should test compounds with clogp in this range that I haven't tested yet.

### Assay 7: z000364
- **logS**: -0.9482 (outside -3 to -2 range)
- **bbb**: 0 ✗
- **Result**: DOES NOT QUALIFY
- **Budget used**: 7 of 8

### Assay 8: z002764
- **logS**: -2.9008 ✓ (within -3 to -2)
- **bbb**: 0 ✗
- **Result**: DOES NOT QUALIFY
- **Budget used**: 8 of 8

## Final Results
- **Total assays performed**: 8 (budget exhausted)
- **Qualifying compounds found**: 3
- **Qualifying compound IDs**: z002040, z002170, z002052

All qualifying compounds were confirmed through actual `measure_admet` assays.

## Summary
The task required finding compounds with:
1. logS between -3 and -2 inclusive
2. bbb = 1

Out of 20 compounds, I tested 8 and found 3 qualifying compounds:
1. **z002040**: logS=-2.3374, bbb=1
2. **z002170**: logS=-2.92, bbb=1  
3. **z002052**: logS=-2.7894, bbb=1

The qualifying compounds show that moderate clogp (1.77-3.0) combined with reasonable tpsa (34.9-65.1) and molecular weight (217.3-369.4) can yield the desired solubility and BBB penetration profile.