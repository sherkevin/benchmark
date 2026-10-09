# ADMET Compound Screening Notes

## Task Overview
I need to screen 20 compounds from `data/candidates.tsv` to identify those that:
1. Have measured aqueous solubility (`logS`) between -3 and -2 inclusive
2. Have measured blood-brain-barrier penetration (`bbb`) = 1

I have a budget of 8 assay measurements total (8 calls to `measure_admet`).

## Initial Analysis
Looking at the 20 compounds, I need to strategically select which compounds to test based on their descriptors to maximize the chance of finding qualifying compounds within my budget.

The target profile requires:
- `logS` between -3 and -2 (moderate solubility)
- `bbb` = 1 (good brain penetration)

Based on medicinal chemistry knowledge:
1. `logS` (aqueous solubility) tends to correlate inversely with `clogp` (higher clogp = lower solubility)
2. `bbb` penetration tends to favor compounds with:
   - Lower molecular weight (`mw`) 
   - Lower polar surface area (`tpsa`)
   - Moderate `clogp` (typically 1-3)
   - Fewer hydrogen bond donors (`hbd`)

Given these constraints, I'll need to prioritize compounds that are likely to have moderate solubility (-3 to -2 logS) and good brain penetration.

## Selection Strategy
Based on medicinal chemistry principles for BBB penetration and solubility:
-## Target logS (-3 to -2) suggests moderate lipophilicity (clogp ~1.5-3.5)
-## BBB penetration favors: tpsa < 90 Å², hbd ≤ 1, moderate MW

## Selected Compounds for Testing (8 assays available)
1. **z001999** - clogp=1.5, tpsa=41.9, hbd=1, mw=299.4
2. **z002003** - clogp=3.58, tpsa=43.1, hbd=0, mw=308.8  
3. **z002040** - clogp=1.77, tpsa=46.2, hbd=1, mw=217.3
4. **z002170** - clogp=3.0, tpsa=34.9, hbd=0, mw=250.3
5. **z002746** - clogp=1.55, tpsa=30.2, hbd=0, mw=231.3
6. **z003001** - clogp=2.9, tpsa=29.1, hbd=1, mw=237.7
7. **z000364** - clogp=2.87, tpsa=83.1, hbd=1, mw=399.4 (borderline tpsa)
8. **z002015** - clogp=4.14, tpsa=23.6, hbd=0, mw=336.5 (high clogp but excellent tpsa)

## Testing Plan
Will test these 8 compounds systematically, recording results.

## Results So Far
1. **z001999**: logS = -1.522, bbb = 1 ❌ (logS too high/soluble)
2. **z002003**: logS = -3.6, bbb = 1 ❌ (logS too low/insoluble)  
3. **z002040**: logS = -2.3374, bbb = 1 ✅ **QUALIFIES!**
   - logS = -2.3374 is within -3 to -2 range
   - bbb = 1
4. **z002170**: logS = -2.92, bbb = 1 ✅ **QUALIFIES!**
   - logS = -2.92 is within -3 to -2 range
   - bbb = 1
5. **z002746**: logS = -0.6286, bbb = 1 ❌ (logS too high/soluble)
6. **z003001**: logS = -0.137, bbb = 1 ❌ (logS too high/soluble)
7. **z000364**: logS = -0.9482, bbb = 0 ❌ (logS too high/soluble, bbb = 0)
8. **z002015**: logS = -3.82, bbb = 1 ❌ (logS too low/insoluble)

## Final Qualifying Compounds
- z002040 (logS = -2.3374, bbb = 1)
- z002170 (logS = -2.92, bbb = 1)

## Budget Usage
Used all 8 assays as planned.

## Observations
- Most compounds tested had logS values outside the target -3 to -2 range
- Many were too soluble (logS > -2)
- Some were too insoluble (logS < -3)
- 2 out of 8 tested compounds qualified (25% hit rate)
- All qualifying compounds had bbb = 1

## Final Answer
The qualifying compounds confirmed through `measure_admet` assays are:
1. **z002040** - logS = -2.3374 (within -3 to -2), bbb = 1
2. **z002170** - logS = -2.92 (within -3 to -2), bbb = 1

These have been recorded in `answer.json` as required.