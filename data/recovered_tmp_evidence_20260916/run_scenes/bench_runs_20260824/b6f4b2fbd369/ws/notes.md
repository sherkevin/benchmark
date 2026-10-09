# Notes - Compound Triage

## Target Profile
- logS between -3 and -2 (inclusive)
- bbb = 1

## Analysis of 20 compounds
Using cheap descriptors (mw, clogp, tpsa, hbd, hba, rotb, rings) to prioritize assays.

### Key filters:
- **BBB penetration**: favors low TPSA (<60), low HBD (0-1), moderate clogp (1-4), low MW (<400)
- **Moderate solubility (logS -3 to -2)**: favors moderate clogp (~1-3), moderate MW

### Compounds eliminated (12):
- z000889: clogp=8.84 → solubility way too poor
- z001510: clogp=-1.04, TPSA=72.7 → solubility too high, BBB unlikely
- z001543: clogp=-1.64, TPSA=119.6 → solubility too high, BBB impossible
- z000364: TPSA=83.1 → BBB very unlikely
- z002216: TPSA=98.0 → BBB very unlikely
- z002683: TPSA=100.7 → BBB very unlikely
- z002730: TPSA=83.6, clogp=0.09 → BBB unlikely, solubility too high
- z015334: TPSA=81.6, clogp=0.74 → BBB unlikely, solubility too high
- z002891: clogp=4.72 → solubility likely too poor
- z002015: clogp=4.14 → solubility likely too poor
- z002010: clogp=4.28, rotb=8 → solubility too poor, BBB hindered by flexibility
- z002764: TPSA=66.8, HBD=2 → BBB unlikely

### Top 8 selected for assays:
1. z002746 (MW=231, clogp=1.55, TPSA=30.2) - excellent BBB, moderate solubility
2. z003001 (MW=238, clogp=2.9, TPSA=29.1) - good BBB, moderate solubility
3. z001999 (MW=299, clogp=1.5, TPSA=41.9) - good BBB, moderate solubility
4. z002040 (MW=217, clogp=1.77, TPSA=46.2) - good BBB, moderate solubility
5. z002170 (MW=250, clogp=3.0, TPSA=34.9) - good BBB, moderate solubility
6. z002003 (MW=309, clogp=3.58, TPSA=43.1) - good BBB, borderline solubility
7. z002052 (MW=369, clogp=1.99, TPSA=65.1) - borderline BBB, good solubility
8. z002011 (MW=326, clogp=4.32, TPSA=30.2) - good BBB, solubility uncertain

## Assay Results (8/8 used)

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002746 | -0.6286 | 1 | ❌ logS > -2 |
| z003001 | -0.137 | 1 | ❌ logS > -2 |
| z001999 | -1.522 | 1 | ❌ logS > -2 |
| z002040 | -2.3374 | 1 | ✅ logS in [-3,-2], bbb=1 |
| z002170 | -2.92 | 1 | ✅ logS in [-3,-2], bbb=1 |
| z002003 | -3.6 | 1 | ❌ logS < -3 |
| z002052 | -2.7894 | 1 | ✅ logS in [-3,-2], bbb=1 |
| z002011 | -4.13 | 1 | ❌ logS < -3 |

## Conclusions
- All 8 tested compounds had bbb=1 — BBB filtering via TPSA/HBD was effective.
- Solubility was the key differentiator: compounds with clogp ~1-1.5 had logS too high (too soluble), while clogp >3.5 had logS too low (insufficient solubility).
- The sweet spot was clogp ~2-3 with low TPSA.

### Qualifying compounds (confirmed by assay):
1. **z002040** (logS=-2.3374, bbb=1)
2. **z002170** (logS=-2.92, bbb=1)
3. **z002052** (logS=-2.7894, bbb=1)

### Untested compounds unlikely to qualify:
- High TPSA compounds (z000364, z002216, z002683, z002730, z015334, z001510, z001543): BBB unlikely
- High clogp compounds (z000889, z002891, z002010, z002015): solubility too poor
- z002764: TPSA=66.8 + HBD=2 makes BBB unlikely despite moderate clogp
