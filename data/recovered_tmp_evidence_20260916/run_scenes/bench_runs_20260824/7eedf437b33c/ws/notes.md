# Notes

## Task understanding
- 33 molecules in `data/pool.tsv` (`mol_id, smiles, mw, alogp, psa, rotb, arom`). No QED in catalogue.
- Budget: 12 `measure_qed` calls shared by both tasks. Uns synthesized → `probe_unavailable`, costs nothing.
- Task 1: QED of **m05** (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, chain L=11). Never synthesized → must PREDICT from its own congeneric series.
- Task 2: pick ONE molecule with (1) measured QED ≥ 0.6, (2) psa ≥ 40, (3) NOT in m05's congeneric series. Same series = same scaffold, differ only in N-alkyl chain length.

## Congeneric series of m05 (methoxy-benzamide scaffold, psa=38.33, arom=1)
Vary N-alkyl chain length L:
| mol | L | rotb | alogp | mw |
|----|---|------|-------|-----|
| m15 | 2 | 3 | 1.445 | 179 |
| m24 | 3 | 4 | 1.835 | 193 |
| m28 | 4 | 5 | 2.225 | 207 |
| m17 | 5 | 6 | 2.615 | 221 |
| m22 | 6 | 7 | 3.005 | 235 |
| m05 | 11 | 12 | 4.956 | 305 |

## Measurements (budget 12, all spent)

m05 congeneric series:
- m15 (L2): 0.7621
- m24 (L3): 0.7919  ← peak
- m28 (L4): 0.7518
- m17 (L5): 0.7498
- m22 (L6): 0.7377
- m05 (L11): probe_unavailable (confirming unsynthesized; predict from trend)

Task-2 candidates (PSA≥40 — nitro series psa=72.24, sugar series psa=110.02; exclude methoxy=38.33 & biphenyl=29.1):
- m09 (nitro L2): 0.5826  (< 0.6, fails)
- m02 (nitro L3): 0.6046  (≥ 0.6 ✓)
- m10 (nitro L10): 0.3724 (fails)
- m14, m27 (sugar): budget exhausted before measurement

## Task 2 decision: m02
`O=[N+]([O-])c1ccc(cc1)C(=O)NCCC` — nitro-benzamide, L=3.
1. measured QED = 0.6046 ≥ 0.6 ✓
2. psa = 72.24 ≥ 40 ✓
3. scaffold = nitro-benzamide ≠ m05's methoxy-benzamide → NOT same congeneric series ✓
m02 is the nitro series peak (L2=0.5826, L3=0.6046, L10=0.3724) → best available PSA≥40 candidate.

## Task 1: predicting m05 (L=11) QED
Trend: methoxy series peaks at L=3 (0.7919), declines to 0.7377 at L=6. m05 is L=11 (rotb=12, alogp=4.956 — both deep in QED penalty zones).

Naive linear extrapolation of L=3..6 gives ~0.65, but this OVERestimates: the nitro series (the only series with a high-chain data point) shows decline *accelerates* at high chain. Nitro linear-from-peak would predict ~0.76 at L=10, but actual m10 (L=10) = 0.3724 (retains only 61.6% of peak over +7 chains).

Anchoring on the nitro high-chain behavior:
- Nitro retains ~61.6% of peak at L=10. Methoxy peak 0.7919 → ~0.488 at L=10.
- m05 is L=11 (one more chain; rotb=12 vs 11; alogp=4.956 vs nitro-L10's 4.465 — worse on both), so slightly below the L=10 estimate.
- Estimate range 0.43-0.48; **predict m05 QED ≈ 0.45**.

## Deliverables
- target_qed (m05, predicted): 0.45
- lead_mol_id: m02
- lead_qed_measured: 0.6046
