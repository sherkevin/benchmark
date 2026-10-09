# Notes

## Task Overview
- Task 1: Report QED of m05 (never synthesized, must predict from congeneric series trend)
- Task 2: Find one molecule with QED ≥ 0.6, psa ≥ 40, not in m05's congeneric series
- Budget: 12 units total (shared between list_pool and measure_qed)

## Pool Analysis
Read all 33 molecules across 9 pages (9 budget units used).

### Congeneric Series Identified:
1. **m05 series** (scaffold: `COc1ccc(cc1)C(=O)N-`): m05, m15, m17, m22, m24, m28
   - All have psa = 38.33
2. **OCC series** (scaffold: `OCC(O)C(O)C(O)C(=O)N-`): m01, m03, m07, m14, m18, m19, m21, m25, m27
   - All have psa = 110.02
3. **Nitro series** (scaffold: `O=[N+]([O-])c1ccc(cc1)C(=O)N-`): m02, m06, m09, m10, m12, m29, m31, m32, m33
   - All have psa = 72.24
4. **Biphenyl series** (scaffold: `c1ccc(cc1)-c1ccc(cc1)C(=O)N-`): m04, m08, m11, m13, m16, m20, m23, m26, m30
   - All have psa = 29.1

## Task 2 Analysis
For Task 2, need:
- QED ≥ 0.6
- psa ≥ 40
- Not in m05 series

Candidates with psa ≥ 40:
- OCC series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

Biphenyl series excluded (psa=29.1 < 40).

## Measurements Performed
Used remaining 3 budget units:
- m01: QED = 0.3187 (too low)
- m02: QED = 0.6046 ✓ (meets all criteria!)
- m03: QED = 0.3282 (too low)

## Task 2 Solution
**m02** satisfies all three constraints:
1. QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. Not in m05 series (nitro series, not methoxy series) ✓

## Task 1 - m05 QED Prediction
m05 could not be measured (probe_unavailable). Must predict from congeneric series trend.

m05 properties:
- SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
- mw: 305.46
- alogp: 4.956 (high lipophilicity - reduces QED)
- psa: 38.33 (moderate)
- rotb: 12 (high flexibility - reduces QED)
- arom: 1

Unfortunately, budget was exhausted before measuring any molecules in m05's congeneric series (m15, m17, m22, m24, m28). 

Based on m05's physicochemical properties:
- High alogp (4.956) indicates poor solubility/permeability balance
- High rotb (12) indicates high flexibility, reducing oral bioavailability
- These factors typically result in moderate QED values

**Estimated QED for m05: ~0.45**

This estimate is based on the molecule's high lipophilicity and flexibility, which are known to reduce drug-likeness scores. The long C13 alkyl chain likely places it past the optimal chain length for this congeneric series.
