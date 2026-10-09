# Task Notes

# Task Notes

## Task Overview
1. Predict QED for m05 (SMILES: COc1ccc(cc1)C(=O)NCCCCCCCCCCC) - not synthesized
2. Find a molecule from pool that satisfies:
   - measured QED ≥ 0.6
   - psa ≥ 40  
   - not in same congeneric series as m05 (same scaffold, differs only in N-alkyl chain length)

Budget: 12 units total (shared between list_pool and measure_qed)
- list_pool page: 1 unit per successful page
- measure_qed: 1 unit per successful measurement (probe_unavailable costs 0)

Budget used: 12/12 units (exhausted)

## Summary of Actions
1. Listed all 33 molecules (9 pages = 9 units)
2. Identified methoxybenzene congeneric series: m15, m24, m28, m17, m22, m05
3. Measured m15 QED = 0.7621 (1 unit)
4. Measured m22 QED = 0.7377 (1 unit)  
5. Attempted to measure m05: probe_unavailable (0 units)
6. Measured m02 QED = 0.6046 (1 unit)

Total: 9 + 1 + 1 + க + 1 = 12 units

## Measurements Taken
1. m15 (methoxybenzene C2): QED = 0.7621
2. m22 (methoxybenzene C6): QED = 0.7377  
3. m02 (nitrobenzene C3): QED = 0.6046
4. m05: probe_unavailable (not synthesized)

## QED Prediction for m05
Based on trend from m15 (C2, QED=0.7621) and m22 (C6, QED=0.7377):
- QED decreases by 0.0244 over 4 carbons = 0.0061 per carbon
- m05 has C13 chain (7 more carbons than m22's C6)
- Expected decrease: 7 × 0.0061 = 0.0427
- Predicted QED: 0.7377 - 0.0427 = 0.6950

Prediction: m05 QED ≈ 0.695

## Task 2 Candidate Selection
Selected m02 because:
1. Measured QED = 0.6046 (≥ 0.6) ✓
2. psa = 72.24 (≥ 40) ✓  
3. Not in same congeneric series as m05 (nitrobenzene scaffold vs methoxybenzene) ✓

## Final Answer Verification
Answer.json contains:
- target_qed: 0.695 (predicted QED for m05 based on congeneric series trend)
- lead_mol_id: "m02" (selected molecule meeting all constraints)
- lead_qed_measured: 0.6046 (measured QED for m02)

All requirements satisfied:
1. Predicted m05 QED based on trend from its congeneric series (m15, m22)
2. Found molecule m02 with QED ≥ 0.6, psa ≥ 40, and different scaffold from m05
3. All QED values reported are from actual measurements (except m05 prediction)
4. Budget fully utilized (12/12 units)

## Molecule Catalog Summary (33 total)

### Methoxybenzene Series (m05 congeneric series)
Same scaffold: COc1ccc(cc1)C(=O)N
1. m15: COc1ccc(cc1)C(=O)NCC (C2 chain) - psa: 38.33
2. m24: COc1ccc(cc1)C(=O)NCCC (C3 chain) - psa: 38.33  
3. m28: COc1ccc(cc1)C(=O)NCCCC (C4 chain) - psa: 38.33
4. m17: COc1ccc(cc1)C(=O)NCCCCC (C5 chain) - psa: 38.33
5. m22: COc1ccc(cc1)C(=O)NCCCCCC (C6 chain) - psa: 38.33
6. m05: COc1ccc(cc1)C(=O)NCCCCCCCCCCC (C13 chain) - psa: 38.33 - TARGET (not synthesized)

### Other Series Candidates (psa ≥ 40 requirement)
Based on psa values from catalog:
- Polyol series (psa = 110.02): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Nitrobenzene series (psa = 72.24): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Biphenyl series (psa = 29.1): m04, m08, m11, m13, m16, m20, m23, m26, m30 (doesn't meet psa ≥ 40)

## Current Plan
1. Measure QED for some methoxybenzene series compounds to establish trend
2. Predict QED for m05 based on trend
3. Search for candidate molecules with psa ≥ 40 and QED ≥ 0.6, not in methoxybenzene series
4. Measure QED for promising candidates

## Notes
- Remaining budget: 3 units
- Need to be strategic with measurements
- For QED prediction trend, could measure 2-3 methoxybenzene compounds
- For candidate search, need to find molecules with psa ≥ 40 (polyol or nitrobenzene series)