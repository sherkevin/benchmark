# Notes on QED Prediction and Lead Selection

## Job 1: Predict QED for m05

### Identifying the Congeneric Series

m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (methoxy-phenyl-amide with C13 chain)

Molecules in the same series (same scaffold `COc1ccc(cc1)C(=O)N-`, different chain lengths):
- m15: C2 chain
- m24: C3 chain
- m28: C4 chain
- m17: C5 chain
- m22: C6 chain

**Congener IDs: m15, m17, m22, m24, m28**

### Measurements for Congeneric Series

| mol_id | Chain | QED |
|--------|-------|-----|
| m15    | C2    | 0.7621 |
| m24    | C3    | 0.7919 |
| m28    | C4    | 0.7518 |
| m17    | C5    | 0.7498 |
| m22    | C6    | 0.7377 |

### Trend Analysis

- Peak QED at C3 (0.7919)
- Decline from C3 onwards: 0.7919 → 0.7518 → 0.7498 → 0.7377
- Decline rates: C3→C4: -0.0401, C4→C5: -0.0020, C5→C6: -0.0121
- Average decline from C3 to C6: ~0.018 per carbon

### Extrapolation to C13 (m05)

From C6 (QED=0.7377) to C13 is 7 additional carbons.

Using average decline rate of ~0.018/carbon:
- Prediction: 0.7377 - 7×0.018 = 0.61

Using the more recent local rate (C5→C6: 0.0121/carbon):
- Prediction: 0.7377 - 7×0.0121 = 0.65

Given the non-linear nature and that drug-likeness typically decreases with very long alkyl chains (increased lipophilicity, high rotatable bond count), I predict **QED ≈ 0.60** for m05.

---

## Job 2: Select Backup Lead Compound

### Constraints
1. measured QED ≥ 0.6
2. psa ≥ 40
3. not in m05 series

### Candidates with psa ≥ 40 (not in m05 series)

**Polyol series** (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
**Nitro series** (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33

### Measurements Taken

**Congeneric series (5 measurements):** All have QED 0.74-0.79 but psa=38.33 < 40, so they fail constraint 2.

**Polyol series (7 measurements):**
| mol_id | QED |
|--------|-----|
| m01    | 0.3187 |
| m03    | 0.3282 |
| m07    | 0.3233 |
| m14    | 0.3237 |
| m18    | 0.3282 |
| m19    | 0.3244 |
| m21    | 0.3297 |

All polyol compounds have QED ~0.32, failing constraint 1 (QED ≥ 0.6).

**Nitro series (0 measurements):** Budget exhausted before measuring any nitro compounds.

### Strategic Error

I allocated 7 measurements to the polyol series when I should have split measurements between polyol and nitro series. With psa=72.24, nitro compounds satisfy constraint 2, and based on chemical similarity to the methoxy series (both are para-substituted phenyl amides), they likely have moderate-to-high QED values.

### Lead Selection

Given my measurement constraints, I cannot confirm any molecule meets all three criteria. However, the nitro series is the most promising:
- psa=72.24 ≥ 40 ✓
- Not in m05 series ✓
- QED: Unknown (not measured), but likely moderate based on chemical intuition

I select **m12** (nitro-phenyl-amide, C4 chain) as my lead choice. The methoxy C4 (m28) has QED=0.7518, suggesting nitro C4 might have comparable QED.

**Note:** This selection is based on chemical reasoning rather than direct measurement due to budget exhaustion. The actual QED of m12 is unknown from my measurements.

---

## Summary

- **m05 congeneric series:** m15, m17, m22, m24, m28
- **Predicted QED for m05:** 0.60 (extrapolated from C2-C6 trend)
- **Lead compound:** m12 (nitro series, psa=72.24, not in m05 series)
- **Measurement budget:** 12/12 exhausted (5 congeneric + 7 polyol)
- **Limitation:** No nitro series molecules were measured; lead QED not confirmed
