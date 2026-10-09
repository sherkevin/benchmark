# ADMET Compound Screening Notes

## Task Overview
- 20 compounds in candidates.tsv
- Need to identify compounds with: 
  1. measured logS between -3 and -2 inclusive
  2. measured bbb = 1 (good brain penetration)
- Budget: 8 assays total (8 `measure_admet` calls)

## Initial Analysis of Descriptors

Key descriptors for prioritization:
- **logS prediction**: Higher solubility (less negative logS) correlates with:
  - Lower clogP (more hydrophilic)
  - Lower molecular weight
  - More polar surface area (TPSA)
- **BBB penetration prediction**: Good brain penetration (bbb=1) correlates with:
  - Moderate clogP (2-4)
  - Lower molecular weight (<400-450)
  - Fewer HBD (<3)
  - Moderate TPSA (<90 Å²)

## Prioritization Strategy

Given limited budget (8 assays), I'll prioritize compounds with:
1. MW < 400 (better chance for BBB penetration)
2. clogP between 2-4 (optimal range for BBB)
3. HBD ≤ -2 (fewer hydrogen bond donors better for BBB)
4. TPSA < 90 Å² (better BBB penetration)

Compounds outside these ranges are less likely to succeed in both criteria.

## Testing Plan
1. First pass: Test 4 most promising candidates based on descriptors
2. Second pass: Depending on results, adjust strategy and test remaining 4

## Compound Analysis and Prioritization

From the 20 compounds, I'll prioritize based on BBB penetration likelihood:

**Primary criteria for BBB penetration (bbb=1):**
1. MW < 450 (all pass except z000889 borderline at 430.7)
2. clogP 2-4 (optimal range)
3. HBD ≤ 2 (fewer hydrogen bond donors)
4. TPSA <   90 Å²

**Primary criteria for logS -3 to -2:**
1. Moderate lipophilicity (clogP not too high or too low)
2. Moderate MW
3. Some polarity but not excessive

**Top candidates based on descriptors:**
1. **z002003** (MW 308.8, clogP 3.58, TPSA 43.1, HBD 0) - Excellent BBB profile
2. **z000364** (MW 399.4, clogP 2.87, TPSA 83.1, HBD 1) - Good profile
3. **z002170** (MW 250.3, clogP 3.0, TPSA 34.9, HBD 0) - Excellent BBB profile
4. **z002011** (MW 325.8, clogP 4.32, TPSA 30.2, HBD 0) - Good but clogP slightly high
5. **z002015** (MW 336.5, clogP 4.14, TPSA 23.6, HBD 0) - Good but clogP slightly high
6. **z002010** (MW 339.5, clogP 4.28, TPSA 29.5, HBD 0) - Good but clogP slightly high
7. **z002001** (MW 237.7, clogP 2.9, TPSA 29.1, HBD 1) - Good profile
8. **z002891** (MW 314.5, clogP 4.72, TPSA 34.1, HBD 0) - clogP high but reasonable

**Lower priority:**
- Very low clogP (<1): z001510 (-1.04), z001543 (-1.64) - likely too soluble (logS > -2)
- Very high clogP (>5): z000889 (8.84) - likely poor solubility (logS < -3)
- High HBD (>2): z002216 (2), z002683 (2), z002764 (2), z015334 (1) - may have BBB issues
- High TPSA (>90): z001543 (119.6), z002683 (100.7), z002216 (98.0) - BBB concerns

## Testing Plan

I'll start with the top 8 candidates, but since I only have 8 assays total, I need to be strategic. I'll test 4 initially, analyze results, then decide on remaining 4.

## Assay Results

**Assay 1/8: z002003**
- logS: -3.6 (too low, outside -3 to -2 range)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** (fails logS criterion)
- Remaining budget:我們剩餘7次試驗

Analysis: This compound has good BBB penetration but is too insoluble. The clogP of 3.58 might be slightly higher than optimal for the target solubility range.

**Assay 2/8: z002170**
- logS: -2.92 (within -3 to -2 range ✓)
- bbb: 1 (good brain penetration ✓)
- **Result: QUALIFIES!**
- Remaining budget: 6 assays

Analysis: Excellent! This compound meets both criteria. MW 250.3, clogP 3.0 appears to be in the sweet spot.

**Assay 3/8: z000364**
- logS: -0.9482 (too high, > -2)
- bbb: 0 (poor brain penetration)
- **Result: Does NOT qualify** (fails both criteria)
- Remaining budget: 5 assays

Analysis: Despite moderate clogP (2.87), this compound is too soluble and lacks BBB penetration. The higher MW (399.4) and presence of multiple methoxy groups might be contributing factors.

**Assay 4/8: z002011**
- logS: -4.13 (too low, < -3)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** (fails logS criterion)
- Remaining budget: 4 assays

Analysis: This compound with clogP 4.32 is too lipophilic, resulting in poor solubility. BBB penetration is good but solubility is outside target range.

## Pattern Analysis after First 4 Assays

1. **z002170** (clogP 3.0): QUALIFIES! Sweet spot found.
2. Compounds with clogP > 3.5 (z002003: 3.58, z002011: 4.32): Too insoluble (logS < -3)
3. Compound with clogP ~2.9 but high MW (z000364): Too soluble (logS > -2) and fails BBB

Observations:
- Target clogP range for both criteria appears to be ~2.5-3.5
- Lower MW helps (<350)
- Need to balance lipophilicity for both solubility and BBB

Next candidates to test (clogP in 2.5-3.5 range):
1. **z003001** (MW 237.7, clogP 2.9, TPSA 29.1, HBD 1) - Promising!
2. **z002040** (MW 217.3, clogP 1.77, TPSA 46.2, HBD 1) - clogP low but worth testing
3. **z002052** (MW 369.4, clogP 1.99, TPSA 65.1, HBD 0) - clogP low, MW high
4. **z002015** (MW 336.5, clogP 4.14, TPSA 23.6, HBD 0) - clogP high but similar to failures

I'll test z003001 next as it looks most promising.

**Assay 5/8: z003001**
- logS: -0.137 (too high, > -2)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** (fails logS criterion)
- Remaining budget: 3 assays

Analysis: This compound with clogP 2.9 is too soluble despite similar clogP to z002170. The difference might be in other structural features. MW is similar (237.7 vs 250.3), but z003001 has a chlorine atom which might affect properties differently.

## Revised Strategy

We have 3 assays remaining. Need to find more compounds like z002170 (clogP ~3.0). Looking at remaining compounds:

Potential candidates:
1. **z002010** (MW 339.5, clogP 4.28) - High clogP, likely too insoluble (similar to z002011)
2. **z002015** (MW 336.5, clogP 4.14) - High clogP, likely too insoluble
3. **z002891** (MW 314.5, clogP 4.72) - Very high clogP, likely too insoluble
4. **z002040** (MW 217.3, clogP 1.77) - Low clogP, might be too soluble
5. **z002052** (MW 369.4, clogP 1.99) - Low clogP, high MW
6. **z002746** (MW 231.3, clogP 1.55) - Low clogP
7. **z002730** (MW 258.2, clogP 0.09) - Very low clogP
8. **z002216** (MW 250.3, clogP 0.86) - Low clogP, high TPSA (98.0)
9. **z002683** (MW 273.2, clogP 2.55) - Moderate clogP but high TPSA (100.7)
10. **z002764** (MW 318.3, clogP 3.56) - Moderate clogP, high TPSA (66.8)

**z002683** (clogP 2.55) looks interesting - moderate clogP but high TPSA might balance solubility.
**z002764** (clogP 3.56) similar to z002003 which failed.

Let me test z002683 next.

**Assay 6/8: z002683**
- logS: -4.17 (too low, < -3)
- bbb: 0 (poor brain penetration)
- **Result: Does NOT qualify** (fails both criteria)
- Remaining budget: 2 assays

Analysis: Despite moderate clogP (2.55), this compound is too insoluble and lacks BBB penetration. The nitro group and phenolic hydroxyls might be causing issues.

## Final 2 Assays

We have 2 assays remaining and only 1 confirmed qualifier (z002170). Need to test remaining promising candidates:

Remaining compounds with clogP in 2-4 range:
1. **z002010** (MW 339.5, clogP 4.28) - High end
2. **z002015** (MW 336.5, clogP 4.14) - High end  
3. **z002891** (MW 314.5, clogP 4.72) - Very high
4. **z002764** (MW 318.3, clogP 3.56) - Similar to failed z002003 (3.58)
5. **z002040** (MW 217.3, clogP 1.77) - Low end

Given that compounds with clogP > 3.5 have failed (z002003: 3.58, z002011: 4.32), compounds with clogP > 4 are unlikely to work.

**z002040** (clogP 1.77) might be worth testing - low clogP could give better solubility.
**z002764** (clogP 3.56) is borderline but similar to failed z002003.

Let me test z002040 first, then depending on result, decide on last assay.

**Assay 7/8: z002040**
- logS: -2.3374 (within -3 to -2 range ✓)
- bbb: 1 (good brain penetration ✓)
- **Result: QUALIFIES!**
- Remaining budget: 1 assay

Analysis: Excellent! This compound with lower clogP (1.77) but moderate MW (217.3) qualifies. This suggests the target logS range (-3 to -2) can be achieved with lower clogP than z002170 (3.0).

## Final Assay Decision

We have 1 assay remaining and 2 confirmed qualifiers (z002170, z002040). Looking at remaining untested compounds that might qualify:

Untested compounds (excluding very low/high clogP):
1. **z002010** (MW 339.5, clogP 4.28) - Likely too insoluble (similar to z002011: 4.32)
2. **z002015** (MW 336.5, clogP 4.14) - Likely too insoluble
3. **z002891** (MW 314.5, clogP 4.72) - Likely too insoluble
4. **z002764** (MW 318.3, clogP 3.56) - Similar to failed z002003 (3.58)
5. **z002052** (MW 369.4, clogP 1.99) - Low clogP but high MW
6. **z002746** (MW 231.3, clogP 1.55) - Low clogP, similar to z002040
7. **z002730** (MW 258.2, clogP 0.09) - Very low clogP
8. **z002216** (MW 250.3, clogP 0.86) - Low clogP, high TPSA

**z002746** (clogP 1.55) looks most promising - similar low clogP to z002040 (1.77) with even lower MW (231.3 vs 217.3). Worth testing as final assay.

**Assay 8/8: z002746**
- logS: -0.6286 (too high, > -2)
- bbb: 1 (good brain penetration)
- **Result: Does NOT qualify** (fails logS criterion)
- Remaining budget: 0 assays

Analysis: This compound is too soluble despite similar clogP to z002040. Structural differences (amide vs succinimide) likely affect solubility.

## Summary of Results

**Confirmed Qualifying Compounds (2):**
1. **z002170** - logS: -2.92, bbb: 1
2. **z002040** - logS: -2.3374, bbb: 1

**Tested but Failed (6):**
1. z002003 - logS: -3.6 (too low), bbb: 1
2. z000364 - logS: -0.9482 (too high), bbb: 0
3. z002011 - logS: -4.13 (too low), bbb: 1
4. z003001 - logS: -0.137 (too high), bbb: 1
5. z002683 - logS: -4.17 (too low), bbb: 0
6. z002746 - logS: -0.6286 (too high), bbb: 1

**Untested Compounds (12):** z000889, z001510, z001543, z001999, z002010, z002015, z002052, z002216, z002730, z002764, z002891, z015334

## Key Insights
1. Target logS range (-3 to -2) requires careful balance of lipophilicity
2. Successful compounds had clogP: 1.77 (z002040) and 3.0 (z002170)
3. BBB penetration (bbb=1) was achieved by many compounds with clogP 2-4
4. The narrow logS window was the main challenge
5. All 8 assays were used efficiently

## Final Answer
**Qualifying Compounds (confirmed via `measure_admet` assays):**
1. **z002170** - logS: -2.92, bbb: 1
2. **z002040** - logS: -2.3374, bbb: 1

Both compounds meet the dual criteria:
.### -3 ≤ logS ≤ -2 (inclusive)
.### bbb = 1 (good brain penetration)

The `answer.json` file has been updated with these qualifying IDs as required."/>
