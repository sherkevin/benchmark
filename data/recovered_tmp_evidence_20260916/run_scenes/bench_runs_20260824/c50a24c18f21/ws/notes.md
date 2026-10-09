# Notes for QED Prediction Task

## Task Summary
- Predict QED for m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - cannot be measured directly
- Pick a backup lead: QED ≥ 0.6, psa ≥ 40, not in same series as m05
- Budget: 17 measurements total (shared between list_pool and measure_qed)

## Pool Exploration

Total: 33 molecules across 9 pages (9 budget units).

### Series Identification

**m05 series (methoxy series)**: Scaffold = COc1ccc(cc1)C(=O)N-
- m05: chain 11 (target, not measurable)
- m15: chain 2
- m24: chain 3
- m28: chain 4
- m17: chain 5
- m22: chain 6

**Nitro series**: Scaffold = O=[N+]([O-])c1ccc(cc1)C(=O)N-
- m02, m06, m09, m10, m12, m29, m31, m32, m33

**Sugar series**: Scaffold = OCC(O)C(O)C(O)C(=O)N-
- m01, m03, m07, m14, m18, m19, m21, m25, m27

**Biphenyl series**: Scaffold = c1ccc(cc1)-c1ccc(cc1)C(=O)N-
- m04, m08, m11, m13, m16, m20, m23, m26, m30

## Congener Measurements (5 molecules, 5 budget units)

| Molecule | Chain | QED |
|----------|-------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 (peak) |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

## QED Trend Analysis for m05 Series

The trend shows:
- QED peaks at chain length 3 (0.7919)
- Gradual decline from chain 3 to 6: 0.7919 → 0.7377 (drop of 0.0542 over 3 carbons)
- Average slope from chain 3-6: -0.0181 per carbon

For extrapolation to chain 11:
- Linear extrapolation from chain 3: 0.7919 + (11-3)*(-0.0181) = 0.647
- Linear extrapolation from chain 6: 0.7377 + (11-6)*(-0.007) = 0.702

However, the task warns against assuming linearity. m05 has:
- alogp = 4.956 (well above optimal ~2-3)
- rotb = 12 (high, negative for QED)
- mw = 305.46 (slightly above optimal)

These factors suggest QED will drop more significantly for very long chains.

**Prediction: m05 QED ≈ 0.58-0.60**

## Lead Selection Challenge

Constraints:
1. Measured QED ≥ 0.6
2. psa ≥ 40
3. Not in m05 series

Budget used: 9 (list_pool) + 8 (measure_qed) = 17/17

### Measurements for Lead Candidates

Nitro series (psa=72.24 ≥ 40):
- m12: QED = 0.4709
- m32: QED = 0.4683
- m33: QED = 0.4591

All measured nitro compounds have QED < 0.6.

Methoxy series (m05 series) has QED 0.73-0.79 but psa=38.33 < 40, so excluded.

### Analysis

Comparing methoxy and nitro series at same chain lengths:
- Chain 4: methoxy 0.7518 vs nitro 0.4709 (diff ~0.28)
- Chain 5: methoxy 0.7498 vs nitro 0.4683 (diff ~0.28)
- Chain 6: methoxy 0.7377 vs nitro 0.4591 (diff ~0.28)

The nitro series consistently has QED ~0.28 lower than methoxy series.

If this pattern holds for shorter chains:
- Nitro chain 2: ~0.76 - 0.28 = 0.48
- Nitro chain 3: ~0.79 - 0.28 = 0.51

Even at its peak, nitro series QED might only reach ~0.51, still < 0.6.

Sugar series has very negative alogp (-2.8 to 0.3), likely resulting in even lower QED.

**Conclusion**: Based on my measurements, no molecule outside the m05 series has both psa ≥ 40 and QED ≥ 0.6. The task constraints may not be satisfiable with the given budget.

### Final Lead Selection

Given the constraints cannot be fully met, I select m12 as the best available lead:
- psa = 72.24 ≥ 40 ✓
- Not in m05 series ✓
- QED = 0.4709 (highest among my measured non-m05-series candidates with psa ≥ 40)

Note: m12 does not meet the QED ≥ 0.6 constraint, but it's the best option given my measurements.

## Final Answer

- target_qed: 0.58 (extrapolated for m05)
- lead_mol_id: m12
- lead_qed_measured: 0.4709
- congener_ids: ["m15", "m17", "m22", "m24", "m28"]
