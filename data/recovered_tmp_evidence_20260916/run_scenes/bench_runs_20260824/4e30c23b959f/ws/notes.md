# Task Notes

## Identifying the m05 Series

m05 SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11-carbon chain)

Scaffold: COc1ccc(cc1)C(=O)N- (methoxyphenyl amide)

**Congeners found in pool:**
- m15: COc1ccc(cc1)C(=O)NCC (2 carbons)
- m24: COc1ccc(cc1)C(=O)NCCC (3 carbons)
- m28: COc1ccc(cc1)C(=O)NCCCC (4 carbons)
- m17: COc1ccc(cc1)C(=O)NCCCCC (5 carbons)
- m22: COc1ccc(cc1)C(=O)NCCCCCC (6 carbons)

## Strategy

1. Measure all 5 congeners to establish QED trend
2. Extrapolate to 11-carbon chain (m05)
3. Find lead compound: QED ≥ 0.6, psa ≥ 40, not in m05 series

## Candidate Series for Lead (psa ≥ 40, not m05 series)

**Sugar-like series (psa = 110.02):**
- m01, m03, m07, m14, m18, m19, m21, m25, m27

**Nitrophenyl series (psa = 72.24):**
- m02, m06, m09, m10, m12, m29, m31, m32, m33

Biphenyl series excluded (psa = 29.1 < 40)
Methoxyphenyl series excluded (psa = 38.33 < 40, and it's m05's series)

## Measurements

Budget used so far: 14 (9 paging + 5 measurements)
Remaining: 3 measurements

**m05 series QED measurements:**
- m15 (2C): 0.7621
- m24 (3C): 0.7919
- m28 (4C): 0.7518
- m17 (5C): 0.7498
- m22 (6C): 0.7377

## Extrapolation for m05 (11C)

**Observed trend:**
- Peak at 3C (0.7919)
- Steady decline from 3C to 6C

**Linear regression from 3C to 6C:**
- Slope: -0.01646 per carbon
- Intercept: 0.83187
- Formula: qed = -0.01646 * chain_length + 0.83187
- Prediction for 11C: 0.65

**Prediction: m05 QED ≈ 0.65**

## Job 2: Finding Lead Compound

Need: QED ≥ 0.6, psa ≥ 40, not in m05 series

Candidates with psa ≥ 40:
- Sugar-like (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrophenyl (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

**Lead compound measurements:**
- m02: QED = 0.6046 ✓ (meets all criteria)
- m12: QED = 0.4709 ✗ (below 0.6)
- m32: QED = 0.4683 ✗ (below 0.6)

**Selected lead: m02**
- QED: 0.6046 (≥ 0.6) ✓
- psa: 72.24 (≥ 40) ✓
- Series: nitrophenyl (not m05 series) ✓

## Final Answer

- **target_qed (m05 prediction):** 0.65
- **lead_mol_id:** m02
- **lead_qed_measured:** 0.6046
- **congener_ids:** m15, m24, m28, m17, m22
