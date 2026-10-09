# Notes — QED prediction & lead selection

## Catalogue (33 molecules) — 4 congeneric series
(scaffold = everything except N-alkyl chain; chain_len = #carbons in that chain; verified with free `describe_structure`)

| series | scaffold | psa | pool members (chain_len) |
|---|---|---|---|
| methoxy-benzamide (**m05's series**) | `COc1ccc(cc1)C(=O)N` | 38.33 | m15(2), m24(3), m28(4), m17(5), m22(6), m05(11, target/unsynth.) |
| nitro-benzamide | `O=[N+]([O-])c1ccc(cc1)C(=O)N` | 72.24 | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) |
| polyol-benzamide | `OCC(O)C(O)C(O)C(=O)N` | 110.02 | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) |
| biphenyl-benzamide | `c1ccc(cc1)-c1ccc(cc1)C(=O)N` | 29.10 | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m16(10) |

## Job 1 — predict m05 QED
- m05 = methoxy-benzamide, chain_len = 11, never synthesized (probe_unavailable). Must extrapolate from its OWN series.
- Pool holds only short-chain methoxy members (chains 2–6). Target chain 11 ⇒ **extrapolation beyond the measured range**.
- congeners (same scaffold, chain length DIFFERS from m05's 11) = m15, m24, m28, m17, m22. (m05's own chain 11 does not "differ" ⇒ excluded.)

Measured (the 5 congeners):
| chain_len | mol_id | QED |
|---|---|---|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Trend: rises to a peak at chain 3, then gently declines. Per the task ("do not assume a straight line… fit the trend you observe") I fit a **degree-2 polynomial** (the tool's default; RMSE 0.0119, slightly better than linear's 0.0131):
`QED(x) = 0.74932 + 0.017024·x − 0.003264·x²`  (peak at x≈2.6, matching the observed peak).

Extrapolating to x=11: `0.74932 + 0.017024·11 − 0.003264·121 = 0.5416`.
⇒ **target_qed = 0.5416**.
Physical sanity check: at chain 11 the heavy QED penalties are rotb=12 (far past optimal 2.25) and alogp=4.956 (past optimal 2.9); these weights fall off faster at large chains, so the decline should STEEPEN beyond chain 6 — the downward-curving parabola is physically motivated, not just arithmetic. (A naïve constant-decline extrapolation from the chain 3→6 slope would give ~0.65; the curve's steeper decline at long chain is the more defensible reading.)

## Job 2 — backup lead (QED≥0.6, psa≥40, NOT m05's series)
- psa≥40 free screen → 18 candidates. m05's methoxy series (psa 38.33) and the biphenyl series (psa 29.10) both fail psa, so the eligible families are **nitro** (psa 72.24) and **polyol** (psa 110.02).
- Measured lead candidates (nitro & polyol):
  - m33 nitro chain 6: **0.4591** (psa 72.24✓, not m05-series✓, but QED < 0.6 ✗)
  - m21 polyol chain 7: 0.3297 ✗
  - m07 polyol chain 9: 0.3233 ✗
- **Polyol is capped ≈0.33** (high HBD=4, very high PSA=110, very negative alogp) — confirmed by two measurements; no polyol reaches 0.6.
- **Nitro at chain 6 = 0.4591.** m33 has near-optimal MW(250)/alogp(2.9)/psa(72) yet sits at 0.46 ⇒ a heavy penalty beyond rotb=7 is in play (aromatic nitro is a QED/toxicophore alert substructure, plus aromatic-ring ratio=1.0). The nitro series likely peaks at a SHORT chain (low rotb); **m09 (nitro, chain 2: rotb 3, MW 194, alogp 1.345)** was the most likely molecule to clear 0.6 but was NOT measured.

### Budget / lead outcome (honest disclosure)
Budget = 17, shared by `list_pool` and `measure_qed`. I spent **9 on `list_pool` paging** (one per page to see all SMILES) + **8 on `measure_qed`** = 17/17. The 9 list_pool pages were avoidable — `screen_candidates` (free) + `describe_structure` (free) give the psa filter and scaffold/chain_len without paging, which would have left ~9 more measurements to probe the nitro short-chain region (m09/m02/m12) where the ≥0.6 lead almost certainly sits.

Because every `lead_qed_measured` value must come from a real `measure_qed` call (explicit task rule), and the only measured non-m05-series molecules (m33, m21, m07) are all below 0.6, I report the **best honest measured lead, m33 (0.4591)**, which satisfies constraints 2 and 3 but **fails constraint 1 (QED < 0.6)**. m09 is flagged here as the intended (unmeasured, budget-exhausted) lead.

## Deliverables
- target_qed = 0.5416 (deg-2 extrapolation of the methoxy series)
- congener_ids = [m15, m24, m28, m17, m22] (m05's series, chains 2–6)
- lead_mol_id = m33 ; lead_qed_measured = 0.4591 (best measured; fails the 0.6 gate — see above)
