# ADMET Screening Notes

## Task Overview
- 20 compounds to screen
- 8 assay budget total (each `measure_admet` call uses 1 budget unit)
- Target profile: logS between -3 and -2 inclusive AND bbb =安排
- Need to identify all qualifying compounds

## Initial Analysis of Chemical Descriptors

Key descriptors that influence solubility and BBB penetration:
- **clogP**: LogP (lipophilicity) - affects both solubility and BBB penetration
  - Lower clogP generally means better solubility
  - Moderate clogP (~1-3) often optimal for BBB penetration
- **tpsa**: Topological polar surface area - affects BBB penetration
  - Lower tpsa (<90 Å²) generally better for BBB
- **mw**: Molecular weight - affects both
  - Lower MW generally better for both solubility and BBB
- **hbd**: Hydrogen bond donors - affects BBB
  - Fewer HBD (<5) better for BBB
- **hba**: Hydrogen bond acceptors
- **rotb**: Rotatable bonds - affects flexibility
- **rings**: Number of rings

## Target Profile Interpretation:
1. logS between -3 and -2: This is moderately soluble (10⁻³ to 10⁻² M)
2. bbb = 1: Can cross blood-brain barrier

## Strategy:
Given limited budget (8/20 = 40% of compounds), need to prioritize compounds most likely to meet both criteria:
- Moderate clogP (not too high, not too low)
- Reasonable tpsa (<90 Å² for BBB)
- Not too high molecular weight
- Limited hydrogen bond donors

## Summary of Results

### Qualifying Compounds (3):
1. **z002052**: logS = -2.7894, bbb = 1 (clogP 1.99, tpsa 65.1)
2. **z002170**: logS = -2.92, bbb = 1 (clogP 3.0, tpsa 34.9)
3. **z002040**: logS = -2.3374, bbb = 1 (clogP 1.77, tpsa 46.2)

### Non-qualifying Compounds Tested (5):
1. z002746: logS = -0.6286, bbb = 1 (too soluble, clogP 1.55)
2. z002003: logS = -3.6, bbb = 1 (too insoluble, clogP 3.58)
3. z003001: logS = -0.137, bbb = 1 (too soluble, clogP 2.9)
4. z001999: logS = -1.522, bbb = 1 (too soluble, clogP 1.5)
5. z000364: logS = -0.9482, bbb = 0 (too soluble, no BBB, clogP 2.87)

### Pattern Observed:
- Optimal clogP range for target logS (-3 to -2) appears to be approximately 1.8-3.0
- Compounds with clogP < 1.8 tend to be too soluble (logS > -2)
- Compounds with clogP > 3.5 tend to be too insoluble (logS < -3)
- BBB penetration (bbb=1) was achieved by most compounds with reasonable tpsa (<90) and few HBD

### Budget Usage:
- Used all 8 assay budget units
- Tested 8 compounds (40% of total)
- Found 3 qualifying compounds (37.5% hit rate among tested compounds)

## Compound Analysis:

| mol_id | mw | clogP | tpsa | hbd | hba | BBB potential | Solubility potential | Test Results |
|--------|----|-------|------|-----|-----|---------------|----------------------|--------------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | 6 | Moderate (tpsa 83.1, hbd 1) | Low-moderate (clogP 2.87) | **logS: -0.9482, bbb: 0** (too soluble, no BBB) |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | 2 | Poor (clogP too high) | Very poor (clogP 8.84) | |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | 3 | Good (low MW, moderate tpsa) | Good (low clogP) | |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | 7 | Poor (high tpsa, many HBD) | Good (low clogP) | |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | 4 | Good (clogP ideal, low tpsa) | Moderate | **logS: -1.522, bbb: 1** (too soluble) |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | 3 | Good (no HBD, low tpsa) | Moderate-low | **logS: -3.6, bbb: 1** (too insoluble) |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | 3 | Moderate (clogP high but no HBD) | Low | |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | 2 | Moderate (clogP high) | Low | |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | 2 | Moderate (clogP high) | Low | |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | 2 | Good (clogP ideal, low tpsa) | Moderate | **logS: -2.3374, bbb: 1** ✓ QUALIFIES |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | 6 | Good (clogP good, no HBD) | Moderate | **logS: -2.7894, bbb: 1** ✓ QUALIFIES |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | 2 | Good (clogP borderline, no HBD) | Moderate-low | **logS: -2.92, bbb: 1** ✓ QUALIFIES |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | 5 | Poor (high tpsa) | Good (low clogP) | |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | 5 | Poor (high tpsa) | Moderate | |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | 4 | Moderate (tpsa borderline) | Good (very low clogP) | |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | 2 | Good (clogP ideal, low tpsa) | Moderate | **logS: -0.6286, bbb: 1** (too soluble) |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | 4 | Moderate (tpsa ok, clogP high) | Low-moderate | |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | 2 | Moderate (clogP high) | Low | |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | 2 | Good (clogP borderline, low tpsa) | Moderate | **logS: -0.137, bbb: 1** (too soluble) |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | 5 | Moderate (tpsa borderline) | Good (low clogP) |