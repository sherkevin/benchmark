# Notes

## Task 1: Predict QED for m05

m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) has never been synthesized, so measure_qed returns probe_unavailable. I need to predict its QED from its congeneric series.

### Congeneric series measurements (same scaffold COc1ccc(cc1)C(=O)N-, different N-alkyl chain lengths):

| mol_id | chain length | QED measured |
|--------|-------------|--------------|
| m15    | 2           | 0.7621       |
| m24    | 3           | 0.7919       |
| m28    | 4           | 0.7518       |
| m17    | 5           | 0.7498       |
| m22    | 6           | 0.7377       |

### Trend analysis:
- QED peaks at chain length 3 (m24: 0.7919)
- Then decreases gradually: 0.7919 → 0.7518 → 0.7498 → 0.7377
- Decrease from chain 3 to 6: 0.0542 over 3 carbons ≈ 0.018 per carbon

### Extrapolation for m05 (chain length 13):
From chain 6 to 13 is 7 more carbons. Linear extrapolation: 0.7377 - 7×0.018 ≈ 0.61

However, m05 has properties far from optimal for drug-likeness:
- mw=305.46 (optimal ~232) - too high
- alogp=4.956 (optimal ~2.7) - too lipophilic
- psa=38.33 (optimal ~41) - slightly low
- rotb=12 (optimal ~5) - too flexible
- arom=1 (optimal ~2.7) - low

These unfavorable properties suggest QED would be lower than linear extrapolation. **Estimated QED for m05: ~0.45-0.50**

## Task 2: Find molecule with QED ≥ 0.6, psa ≥ 40, not in m05 series

### Molecules with psa ≥ 40 (not in m05 series):
- Nitro-phenyl series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Polyol series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27

### Measurements made (budget: 12 total):
Used 5 on m05 congeneric series, 7 on Task 2 candidates.

| mol_id | QED | psa | Valid for Task 2? |
|--------|-----|-----|-------------------|
| m01    | 0.3187 | 110.02 | No (QED < 0.6) |
| m09    | 0.5826 | 72.24  | No (QED < 0.6) |
| m14    | 0.3237 | 110.02 | No (QED < 0.6) |
| m12    | 0.4709 | 72.24  | No (QED < 0.6) |
| m32    | 0.4683 | 72.24  | No (QED < 0.6) |
| m27    | 0.333  | 110.02 | No (QED < 0.6) |
| m03    | 0.3282 | 110.02 | No (QED < 0.6) |

### Result:
No molecule measured satisfies all three conditions. The closest was m09 with QED=0.5826 (just below 0.6 threshold).

**Reflection**: My measurement strategy was suboptimal. I should have measured more nitro-phenyl compounds with intermediate chain lengths (e.g., m02 with alogp=1.735, closer to optimal 2.7) which might have QED ≥ 0.6. m02 (mw=208, alogp=1.735, rotb=4) has properties closer to optimal than m09 (mw=194, alogp=1.345, rotb=3), and might achieve QED ≥ 0.6.

## Budget usage: 12/12 measurements

## Final Answer

**Task 1 (m05 QED prediction):** 0.61

Based on the congeneric series trend (as specified in the task), linear extrapolation from chain 3 (QED=0.7919) to chain 13 gives: 0.7919 - 10*0.0181 = 0.611 ≈ 0.61. While m05's properties (high MW, high logP) might suggest lower QED, the task specifies predicting from the congeneric series trend.

**Task 2 (lead molecule):** No valid molecule found with my measurement strategy.

The molecule m09 came closest with QED=0.5826, psa=72.24, and not in m05 series, but QED < 0.6 threshold. My measurement strategy was suboptimal - I spent 5 measurements on the m05 congeneric series when 2-3 would have sufficed for trend analysis. The remaining 7 measurements on Task 2 candidates didn't yield any molecule with QED ≥ 0.6.

Unmeasured molecules like m02 (nitro series, alogp=1.735, mw=208) or m33 (nitro series, alogp=2.905, mw=250) might potentially have QED ≥ 0.6 based on their properties being closer to optimal, but I couldn't measure them due to budget exhaustion.
