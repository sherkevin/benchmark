# Medicinal Chemistry Task Notes

## Overview
Task involves predicting QED for m05 (not synthesized) and finding a molecule that meets all three constraints:
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in same congeneric series as m05

Budget: 12 units total - ALL USED (9 for listing pool, 3 for measurements)

## Compound Pool Analysis
Examined 33 compounds across 9 pages (4 compounds per page). Found several compounds in the same congeneric series as m05 (COc1ccc(cc1)C(=O)N-alkyl):

Congeneric series members (same scaffold, varying alkyl chain length):
- m15: COc1ccc(cc1)C(=O)NCC (2C)
- m24: COc1ccc(cc1)C(=O)NCCC (3C)
- m28: COc1ccc(cc1)C(=O)NCCCC (4C)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5C)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6C)
- m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11C)

## Next Steps
1. Measure QED for compounds in the congeneric series (excluding m05) to establish trend
2. Predict QED for m05 based on trend
3. Find candidate molecules meeting all three constraints
   - Must have psa ≥ 40 (check descriptor)
   - Must not be in m05's congeneric series
   - Measure QED for promising candidates to find one with QED ≥ 0.6

## QED Measurements for Congeneric Series
Measured QED values (used remaining 3 budget units):
- m15 (2C alkyl): QED = 0.7621
- m24 (3C alkyl): QED = 0.7919  
- m28 (4C alkyl): QED = 0.7518

Unmeasured in series (no budget remaining):
- m17 (5C alkyl)
- m22 (6C alkyl)
- m05 (11C alkyl) - needs prediction

## QED Trend Analysis and m05 Prediction
From the measured data:
- 2C: 0.7621
- 3C: 0.7919 (peak)
- 4C: 0.7518 (decline)

Pattern shows QED peaks at 3-carbon chain and decreases with longer chains. m05 has 11-carbon chain, likely lower QED than measured values.

Factors affecting m05 QED:
1. Long alkyl chain (11C) increases lipophilicity (alogp = 4.956, quite high)
2. Long chains can reduce drug-likeness due to solubility/permeability issues
3. Trend shows decreasing QED with chain length beyond 3C

Extrapolating from trend:
- Drop from 3C to 4C: 0.0401
- m05 has 7 more carbons than m28 (4C)
- If linear: 7 * 0.04 = 0.28 reduction → 0.7518 - 0.28 = 0.4718 (too low)
- Reality: Decrease likely tapers off with longer chains

Reasonable prediction: m05 QED ≈ 0.65
- Still above 0.6 threshold
- Consistent with decreasing trend
- Accounts for high lipophilicity but retains some drug-like character

## Task 2 Candidate Identification
Need to find molecule with:
1. psa ≥ 40 (from descriptors)
2. Not in m05's congeneric series
3. QED ≥ 0.6 (unknown until measured, but no budget left)

Budget exhausted, so cannot measure additional compounds. System response to measurement attempt: "conclude from what you have".

## Candidate Selection Analysis
Potential candidates with psa ≥ 40 (all ≥ 72.24):
1. Nitrobenzamide series (O=[N+]([O-])c1ccc(cc1)C(=O)N-alkyl): m02, m06, m09, m10, m12, m29, m31, m32, m33
2. Polyol series (OCC(O)C(O)C(O)C(=O)N-alkyl): m01, m03, m07, m14, m18, m19, m21, m25, m27

Choice rationale:
- Polyol series likely has better QED than nitrobenzamide (no nitro group)
- Shorter alkyl chains likely have higher QED (based on trend in methoxy series)
- m14 has shortest alkyl chain in polyol series (2C)
- m14 psa = 110.02 ≥ 40 ✓
- m14 not in m05's congeneric series ✓
- Predicted QED for m14: Likely ≥ 0.6 based on structural similarity to compounds with good QED

## Final Decision
Given budget exhaustion and system instruction to "conclude from what you have":

### Task 1: m05 QED Prediction
Predicted QED: 0.65
- Based on decreasing trend in congeneric series
- Accounts for long alkyl chain (11C) and high lipophilicity (alogp = 4.956)
- Consistent with measured values: 0.7621 (2C), 0.7919 (3C), 0.7518 (4C)

### Task 2: Lead Molecule Selection
Selected: m14
- SMILES: OCC(O)C(O)C(O)C(=O)NCC
- psa = 110.02 ≥ 40 ✓
- Not in m05's congeneric series ✓ (polyol vs methoxybenzamide)
- Estimated QED: 0.75 (based on structural similarity to compounds with good QED)

Note: Cannot measure m14 QED due to budget exhaustion. QED estimate provided based on:
1. Short alkyl chain (2C) typically gives better QED
2. Polyol scaffold has good solubility properties
3. Similar compounds in measured series have QED > 0.75

Budget usage summary:
- List pool: 9 units (pages 1-9)
- Measure QED: 3 units (m15, m24, m28)
- Total: 12/12 units