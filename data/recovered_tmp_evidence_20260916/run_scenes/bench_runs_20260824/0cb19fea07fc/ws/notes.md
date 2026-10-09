# Notes

## Catalog Analysis

Listed all 33 molecules across 9 pages (budget: 9/12 for listing).

### Congeneric Series Identified

**Series A — m05's series** (scaffold: `COc1ccc(cc1)C(=O)N-`, methoxybenzamide, psa=38.33):
| mol_id | N-alkyl | n_C | MW     | ALOGP | ROTB |
|--------|---------|-----|--------|-------|------|
| m15    | NCC     | 2   | 179.22 | 1.445 | 3    |
| m24    | NCCC    | 3   | 193.25 | 1.835 | 4    |
| m28    | NCCCC   | 4   | 207.27 | 2.225 | 5    |
| m17    | NCCCCC  | 5   | 221.30 | 2.615 | 6    |
| m22    | NCCCCCC | 6   | 235.33 | 3.005 | 7    |
| m05    | NC₁₁    | 11  | 305.46 | 4.956 | 12   |

**Series B** (scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N-`, nitrobenzamide, psa=72.24):
m09(2C), m02(3C), m12(4C), m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C)

**Series C** (scaffold: `OCC(O)C(O)C(O)C(=O)N-`, sugar amide, psa=110.02):
m14(2C), m27(3C), m01(4C), m19(5C), m03(6C), m21(7C), m18(8C), m07(9C), m25(10C)

**Series D** (scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`, biphenyl amide, psa=29.10):
m13(2C), m11(3C), m04(4C), m30(5C), m23(6C), m20(7C), m26(8C), m08(9C), m16(10C)

## Measurements (3 remaining budget units used)

| mol_id | Series | QED measured |
|--------|--------|-------------|
| m15    | A      | 0.7621      |
| m22    | A      | 0.7377      |
| m09    | B      | 0.5826      |

## Task 1: Predict m05 QED

From Series A measurements:
- m15 (2C): QED = 0.7621
- m22 (6C): QED = 0.7377
- Decrease over 4 CH₂ groups: 0.7621 − 0.7377 = 0.0244
- Rate: −0.0061 per CH₂

Linear extrapolation to m05 (11C):
- From m22 (6C) to m05 (11C): 5 more CH₂ groups
- Expected decrease: 5 × 0.0061 = 0.0305
- **Predicted QED(m05) = 0.7377 − 0.0305 = 0.7072 ≈ 0.71**

## Task 2: Lead Molecule Selection

Constraints: (1) measured QED ≥ 0.6, (2) psa ≥ 40, (3) not in Series A

Candidates with psa ≥ 40 not in Series A: Series B (psa=72.24) and Series C (psa=110.02).

- m09 (Series B, shortest chain → highest QED expected): measured QED = 0.5826 (< 0.6)
- Since m09 is the most drug-like Series B member (lowest MW, ALOGP, ROTB), other Series B members should have lower QED.
- Series C molecules have 0 aromatic rings and very negative ALOGP, likely giving lower QED.
- Budget exhausted before measuring additional candidates.

**Best available: m09** (QED=0.5826, psa=72.24, different series from m05).
Satisfies constraints 2 and 3 but narrowly misses constraint 1.

## Budget Summary
- Listing: 9 pages = 9 units
- Measurements: 3 = 3 units  
- Total: 12/12 used
