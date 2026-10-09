# Notes

## Task Overview
- Pool: 33 molecules, 4 per page (9 pages)
- Budget: 12 units total (shared between list_pool and measure_qed)
- Task 1: Predict QED of m05 (never synthesized) from congeneric series trend
- Task 2: Find molecule with QED ≥ 0.6, psa ≥ 40, not in m05's congeneric series

## m05 Info
- SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (methoxyphenyl amide, 11-carbon N-alkyl chain)
- Scaffold: COc1ccc(cc1)C(=O)N- (4-methoxybenzamide)
- Descriptors: MW=305.46, ALOGP=4.956, PSA=38.33, ROTB=12, AROM=1

## Budget Used: 12/12
- 9 pages read (9 units)
- 3 QED measurements (3 units)

## Congeneric Series of m05: COc1ccc(cc1)C(=O)N-
m15: 2C, MW=179, alogp=1.445, rotb=3, arom=1
m24: 3C, MW=193, alogp=1.835, rotb=4, arom=1
m28: 4C, MW=207, alogp=2.225, rotb=5, arom=1 → MEASURED QED=0.7518
m17: 5C, MW=221, alogp=2.615, rotb=6, arom=1
m22: 6C, MW=235, alogp=3.005, rotb=7, arom=1 → MEASURED QED=0.7377
m05: 11C, MW=305, alogp=4.956, rotb=12, arom=1 (NEVER SYNTHESIZED)

## Task 1: Predicting m05 QED
Two measured members:
- m28 (4C): QED = 0.7518
- m22 (6C): QED = 0.7377

Linear extrapolation: -0.00705 per carbon × 5 = -0.035 → QED ≈ 0.703
Non-linear correction for high ALOGP (4.956) and high ROTB (12): additional ~0.03 drop
**Final estimate: QED ≈ 0.67**

## Task 2: Lead molecule
Only measured non-m05-series molecule with PSA≥40: m12 (nitrophenyl, PSA=72.24)
m12 QED = 0.4709 (low due to nitro group structural alert)
Budget exhausted - best available option.

## Other Scaffolds Identified
- Sugar acids (OCC(O)C(O)C(O)C(=O)N-): PSA=110.02, very negative ALOGP
- Nitrophenyls (O=[N+]([O-])c1ccc(cc1)C(=O)N-): PSA=72.24, nitro alert lowers QED
- Biphenyls (c1ccc(cc1)-c1ccc(cc1)C(=O)N-): PSA=29.10 (below 40, excluded)
