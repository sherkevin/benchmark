# Notes on Compound Screening

## Strategy
Target profile: logS between -3 and -2 (inclusive), bbb = 1

Prioritizing compounds with:
- TPSA < 70 (for BBB penetration)
- clogp between 2-5 (balanced lipophilicity for solubility and BBB)
- HBD <= 1 (for BBB penetration)

## Assays Run

### Assay 1: z002003
- Profile: clogp=3.58, tpsa=43.1, hbd=0
- Good candidate for BBB penetration

### Assay 2: z002010
- Profile: clogp=4.28, tpsa=29.5, hbd=0
- Good candidate for BBB penetration

### Assay 3: z002011
- Profile: clogp=4.32, tpsa=30.2, hbd=0
- Good candidate for BBB penetration

### Assay 4: z002015
- Profile: clogp=4.14, tpsa=23.6, hbd=0
- Good candidate for BBB penetration

### Assay 5: z002170
- Profile: clogp=3.0, tpsa=34.9, hbd=0
- Good candidate for BBB penetration

### Assay 6: z002891
- Profile: clogp=4.72, tpsa=34.1, hbd=0
- Good candidate for BBB penetration, though clogp is high

### Assay 7: z003001
- Profile: clogp=2.9, tpsa=29.1, hbd=1
- Good candidate for BBB penetration

### Assay 8: z001999
- Profile: clogp=1.5, tpsa=41.9, hbd=1
- Moderate candidate, clogp on lower side

## Results

| mol_id | logS | bbb | Qualifies? |
|--------|------|-----|------------|
| z002003 | -3.6 | 1 | NO (logS < -3) |
| z002010 | -5.0097 | 1 | NO (logS < -3) |
| z002011 | -4.13 | 1 | NO (logS < -3) |
| z002015 | -3.82 | 1 | NO (logS < -3) |
| z002170 | -2.92 | 1 | **YES** |
| z002891 | -4.5526 | 0 | NO (logS < -3, bbb=0) |
| z003001 | -0.137 | 1 | NO (logS > -2) |
| z001999 | -1.522 | 1 | NO (logS > -2) |

## Qualifying Compounds
- z002170 (logS=-2.92, bbb=1) - meets both criteria

Budget used: 8/8 assays
