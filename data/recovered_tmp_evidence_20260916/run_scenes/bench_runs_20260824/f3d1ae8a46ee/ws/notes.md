# ADMET Screening Notes

## Task Overview
- Need to screen 20 compounds from `data/candidates.tsv`
- Each compound has descriptors: mw, clogp, tpsa, hbd, hba, rotb, rings
- Need to measure logS (between -3 and -2 inclusive) and bbb (=1) using `measure_admet(mol_id)`
- Budget: 8 assays total (8 calls to `measure_admet`)
- Must confirm each qualifying compound with actual measurement

## Strategy
1. Analyze existing descriptors to prioritize likely candidates
2. Use limited budget (8 assays) efficiently
3. Focus on compounds likely to have logS -3 to -2 AND bbb=1

## Descriptor Analysis
Key factors for solubility (logS):
- clogp (logP): Lower values generally correlate with better solubility
- tpsa (Topological Polar Surface Area): Higher values often correlate with better aqueous solubility
- hbd/hba (hydrogen bond donors/acceptors): More H-bonding groups improve solubility

Key factors for blood-brain barrier penetration (bbb=1):
- Lower molecular weight (<450 Da)
- Lower tpsa (<90 Å²)
- Moderate clogp (1-3)
- Fewer HBD (<3)

Target profile: logS between -3 and -2 (moderately soluble), bbb=1

## Measurements
1. z002003: logS = -3.6, bbb = 1
   - logS too low (< -3), doesn't qualify
   - Budget used: 1, remaining: 7

2. z002040: logS = -2.3374, bbb = 1
   - QUALIFIES! logS within -3 to -2 range
   - Budget used: 2, remaining: 6

3. z001999: logS = -1.522, bbb = 1
   - logS too high (> -2), doesn't qualify
   - Budget used: 3, remaining: 5

4. z002052: logS = -2.7894, bbb = 1
   - QUALIFIES! logS within -3 to -2 range
   - Budget used: 4, remaining: 4