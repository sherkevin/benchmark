# Notes

## Approach

### Task 1: Predict QED for m05
- m05 is in the scaffold group `COc1ccc(cc1)C(=O)N` with chain length 11
- Other members of this series: m15 (len=2), m24 (len=3), m28 (len=4), m17 (len=5), m22 (len=6)
- Measured m15, m24, m28 successfully:
  - m15: QED = 0.7621
  - m24: QED = 0.7919
  - m28: QED = 0.7518
- Used linear regression (degree=1) to predict QED at chain length 11
- Prediction: 0.7274

### Task 2: Find molecule with QED ≥ 0.6, psa ≥ 40, not in m05's series
- m05's series has psa = 38.33, so none of its members qualify (psa < 40)
- Candidates with psa ≥ 40:
  - Nitro series (psa=72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
  - Sugar series (psa=110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Budget exhausted before measuring any of these candidates
- Cannot provide a valid answer for Task 2

## Budget Usage
- list_pool pages 1-9: 9 units
- measure_qed m15: 1 unit
- measure_qed m24: 1 unit
- measure_qed m28: 1 unit
- Total: 12/12 units (exhausted)

## Lesson Learned
Should have been more strategic:
1. Use group_by_scaffold (free) to get mol_ids without listing all pages
2. Measure Task 2 candidates first (from scaffolds with psa ≥ 40)
3. Use remaining budget for m05's series trend

## Final Answer
- target_qed: 0.7274 (predicted for m05 using linear trend from m15, m24, m28)
- lead_mol_id: null (no valid measurement possible due to budget exhaustion)
- lead_qed_measured: null

The budget was exhausted before any Task 2 candidate could be measured. An optimal strategy would have reserved budget for measuring molecules from the nitro or sugar series (psa ≥ 40, different scaffold from m05).
