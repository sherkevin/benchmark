# Notes — QED prediction & lead pick

## Setup
- 33 molecules, shared budget of 17 units (list_pool pages + measure_qed).
- Page 1 fetched (cost 1). Budget used so far: 1.

## group_by_scaffold (free) — 4 scaffolds
1. **Methoxy-benzamide** `COc1ccc(cc1)C(=O)N` — m05's series (Task 1 target series)
   - m15 (c2), m24 (c3), m28 (c4), m17 (c5), m22 (c6), **m05 (c11, unsynth)**
2. **Nitro-benzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N`
   - m09 (c2), m02 (c3), m12 (c4), m32 (c5), m33 (c6), m29 (c7), m31 (c8), m06 (c9), m10 (c10)
3. **Sugar/alcohol** `OCC(O)C(O)C(O)C(=O)N`
   - m14 (c2), m27 (c3), m01 (c4), m19 (c5), m03 (c6), m21 (c7), m18 (c8), m07 (c9), m25 (c10)
4. **Biphenyl-benzamide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N`
   - m13 (c2), m11 (c3), m04 (c4), m30 (c5), m23 (c6), m20 (c7), m26 (c8), m08 (c9), m16 (c10)

## Page 1 descriptors (cost 1)
- m01: mw 221.25, alogp -2.022, psa 110.02, rotb 7, arom 0  (sugar, c4)
- m02: mw 208.22, alogp 1.735, psa 72.24, rotb 4, arom 1    (nitro, c3) ← Task-2 candidate
- m03: mw 249.31, alogp -1.242, psa 110.02, rotb 9, arom 0  (sugar, c6)
- m04: mw 253.34, alogp 3.884, psa 29.1, rotb 5, arom 2     (biphenyl, c4) — psa<40, no good

## Plan
- Task 1: measure QED of methoxy-series members (m15,m24,m28,m17,m22 at chains 2,3,4,5,6),
  fit trend vs chain_len, evaluate at chain 11 → m05 QED.
- Task 2: measure m02 (nitro, psa 72.24≥40, not m05 series). Expect QED≥0.6. Backup: m09.

## Measurements (budget: 17 total, used 8)

### Task 1 — methoxy-benzamide series (m05's series) QED vs chain_len
| chain | mol_id | QED    |
|-------|--------|--------|
| 2     | m15    | 0.7621 |
| 3     | m24    | 0.7919 |  ← peak
| 4     | m28    | 0.7518 |
| 5     | m17    | 0.7498 |
| 6     | m22    | 0.7377 |
| 11    | m05    | **unsynth → predicted**

### Task 2 — candidate measurement
- m02 (nitro-benzamide, chain 3): QED = **0.6046** ✓

### Page 2 (cost 1) — confirms m05
- m05: mw 305.46, alogp 4.956, psa 38.33, rotb 12, arom 1, SMILES COc1ccc(cc1)C(=O)NCCCCCCCCCCC (11 C)
- m06: nitro c9, psa 72.24  (alt Task-2 backup if needed)
- m07: sugar c9
- m08: biphenyl c9, psa 29.1

## Trend fits for m05 (chain 11)
- degree-2, all 5 pts (2,3,4,5,6): **0.5416** ← CHOSEN (RMSE 0.0119, lowest; respects curvature)
- degree-1, all 5 pts: 0.6950 (RMSE 0.0131; ignores peak, over-predicts)
- degree-2, pts 3,4,5,6: 0.938 (nonsense — parabola curves back up)
- degree-2, pts 2,3,4,5: 0.269 (nonsense — dips hard)
- Far extrapolation is unstable (0.27–0.94 across subsets) → honest uncertainty, picked most principled fit.
- Chemistry check: m05 alogp 4.96 + rotb 12 are both past drug-likeness optima → QED well below series peak → 0.54 is more credible than 0.70.

## Final deliverable
- target_qed (m05) = 0.5416
- lead_mol_id = m02
- lead_qed_measured = 0.6046

### Task-2 constraint check for m02
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. nitro-benzamide scaffold ≠ m05's methoxy-benzamide scaffold ✓ (different scaffold, not same series)
