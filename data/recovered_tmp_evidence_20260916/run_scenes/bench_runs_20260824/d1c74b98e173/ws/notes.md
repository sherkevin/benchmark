# Notes on QED Prediction Task

## Task Analysis

1. **Task 1**: Predict QED for `m05` (SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`)
   - Molecule has never been synthesized
   - Need to predict QED from its congeneric series
   - Must use `measure_qed` to measure similar molecules to establish trend

2. **Task 2**: Find a molecule that satisfies:
   - measured QED ≥ 0.6
   - `psa` ≥ 40
   - Not in same congeneric series as `m05` (different scaffold or not just N-alkyl chain length variation)

3. **Constraints**:
   - Total budget: 12 measurements
   - `m05` cannot be measured directly (probe_unavailable)

## Initial Observations

From pool.tsv:
- `m05`: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (13-carbon alkyl chain)
- Looking for molecules with same scaffold: methoxybenzamide with N-alkyl chain
- Need to identify congeneric series for `m05` first

## Measurements Summary (8/12 budget used)

**m05 congeneric series (methoxybenzamides with N-alkyl chain):**
1. `m15` (2-carbon chain): QED = 0.7621
2. `m24` (3-carbon chain): QED = 0.7919  
3. `m28` (4-carbon chain): QED = 0.7518
4. `m17` (5-carbon chain): QED = 0.7498
5. `m22` (6-carbon chain): QED = 0.7377

**Additional measurements for analysis:**
6. `m02` (nitrobenzamide, 3C): QED = 0.6046 ✓ (Task 2 candidate)
7. `m10` (nitrobenzamide, 10C): QED = 0.3724
8. `m33` (nitrobenzamide, 6C): QED = 0.4591

## Analysis for m05 QED Prediction

**Chain length analysis:**
- m05 has 11-carbon alkyl chain (undecyl)
- QED trend in methoxy series: peaks at 3C (0.7919), decreases with longer chains
- From 3C to 6C: -0.0542 over 3 carbons = -0.01807 per carbon
- Extrapolating to 11C: 8 carbons from 3C → -0.1446 → predicted QED = 0.7919 - 0.1446 = 0.6473

**Cross-series comparison:**
- Methoxy series has higher baseline QED than nitro series (~0.28 higher at 6C)
- Nitro series shows faster QED decrease with chain length (-0.0217 per carbon from 6C to 10C)
- m05 alogp = 4.956 (very lipophilic), similar to m10 (alogp = 4.465) which has QED = 0.3724
- Accounting for series difference and alogp, predicted m05 QED ≈ 0.60-0.62

**Final m05 QED prediction:** 0.61

## Task 2 Solution

Found `m02` satisfies all criteria:
1. Measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓  
3. Not in same congeneric series as m05 (nitrobenzamide vs methoxybenzamide) ✓

## Final Answer

**Task 1 (m05 QED prediction):** 0.61
- Based on trend analysis of methoxybenzamide series
- Considering decreasing QED with increasing alkyl chain length
- Accounted for high lipophilicity (alogp = 4.956)

**Task 2 (lead molecule):** `m02`
- QED measured: 0.6046
- Satisfies all three constraints
- Budget used: 8/12 measurements

**answer.json updated with final values.**