# Notes

## Pool structure (free group_by_scaffold)
4 scaffolds, each a benzamide with varying N-alkyl chain length:

| Scaffold | core | members (chain_len) | psa estimate* |
|---|---|---|---|
| 1 (m05's) | `COc1ccc(cc1)C(=O)N` methoxy | m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, unsynth)** | ~38 (<40) |
| 2 | `O=[N+]([O-])c1ccc(cc1)C(=O)N` nitro | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) | ~74 (>=40) |
| 3 | `OCC(O)C(O)C(O)C(=O)N` sugar/gluconamide | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) | ~110 (>=40) |
| 4 | `c1ccc(cc1)-c1ccc(cc1)C(=O)N` biphenyl | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) | ~29 (<40) |

*TPSA fragment additivity: secondary amide (-CONH-) = 29.10; methoxy O = 9.23; nitro = ~45.17; each OH = 20.23.
Within a series only CH2 changes -> psa, arom, alerts, hba, hbd CONSTANT; only mw, alogp, rotb change with chain.

## Budget (17 total, shared list_pool + measure_qed)
Used 0 list_pool pages (group_by_scaffold + structural psa reasoning made listing unnecessary).
Used 17 measure_qed calls. Budget exhausted at 17/17.

## Measurements (all real measure_qed calls)
### Task 1 — methoxy scaffold (m05's series), chain 2-6
| mol_id | chain | QED |
|---|---|---|
| m05 | 11 | probe_unavailable (unsynth, free) — must predict |
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |
Shape: peaks at chain 3, gently declines to chain 6.

### Task 1 cross-check — biphenyl analog (closest clean benzamide; has real long-chain data)
| mol_id | chain | QED |
|---|---|---|
| m13 | 2 | 0.8540 |
| m23 | 6 | 0.7345 |
| m20 | 7 | 0.6780 |
| m26 | 8 | 0.6136 |
| m08 | 9 | 0.5472 |
| m16 | 10 | 0.4842 |
Long-chain (6-10) nearly perfectly linear (RMSE 0.0024): decline ~-0.063/chain. Real decline confirmed.

### Task 2 candidates (nitro scaffold, psa~74>=40, not scaffold 1)
| mol_id | chain | QED | meets >=0.6? |
|---|---|---|---|
| m09 | 2 | 0.5826 | no |
| **m02** | **3** | **0.6046** | **YES** |
| m12 | 4 | 0.4709 | no |
| m33 | 6 | 0.4591 | no |
Sugar checks (psa~110, but low logP/rotb issues expected):
| m19 | 5 | 0.3244 | no |
| m03 | 6 | 0.3282 | no |

## qed_components (free)
- m15 (methoxy ch2): MW 0.332, ALOGP 0.874, HBA 0.931, HBD 0.987, PSA 0.933, ROTB 0.993, AROM 0.827, ALERTS 0.843
- m22 (methoxy ch6): MW 0.684, ALOGP 0.997, HBA 0.931, HBD 0.987, PSA 0.933, ROTB 0.533, AROM 0.827, ALERTS 0.640
  -> from ch2->ch6: MW rises (0.33->0.68, approaching MW peak ~300), ALOGP peaks (~1.0), ROTB falls (0.99->0.53)
- m09 (nitro ch2): ALERTS 0.241 (nitro = structural alert, permanent cap on the nitro series); MW 0.412
- m02 (nitro ch3): MW 0.498, ALOGP 0.922, HBA 0.985, HBD 0.987, PSA 0.930, ROTB 0.970, AROM 0.827, ALERTS 0.241
  -> ALERTS 0.241 is the only weak score; the other 7 are all >=0.83. QED 0.6046 just clears 0.6.

## Task 1 — m05 prediction (chain 11)
Extrapolate 5 units past last measured methoxy point (chain 6). Fits:
| Fit | points | pred @11 |
|---|---|---|
| methoxy linear, ch2-6 | 5 | 0.695 |
| methoxy quadratic, ch2-6 | 5 | **0.542** |
| methoxy quadratic, ch2-5 | 4 | 0.269 (artifact: overcurves past peak) |
| biphenyl linear, ch6-10 | 5 | 0.422 |
| biphenyl quadratic, ch6-10 | 5 | 0.415 |
| biphenyl quadratic, ch2,6-10 | 6 | 0.397 |

Component-based reasoning for m05 (chain 11) vs m22 (chain 6):
- ROTB: 4 -> 9 (declines, ~0.53 -> lower)
- ALOGP: at peak (~1.0) -> logP ~5.0 well past peak (declines)
- MW: 0.684 (MW~235, below peak) -> MW~305 (near peak ~300, IMPROVES toward ~1.0)
The MW improvement partially offsets ROTB+ALOGP decline.

Why the spread:
- methoxy linear (0.695) IGNORES the observed peak curvature -> overestimate.
- biphenyl analog (0.42) UNDERESTIMATES: biphenyl MW@11~351 (past peak, MW score~0.75) vs methoxy MW@11~305 (at peak, MW score~1.0); biphenyl logP also higher (further past logP peak). So methoxy should sit ABOVE the biphenyl analog value.
- methoxy quadratic (0.542) uses m05's OWN congeneric series (task-endorsed), captures peak+decline shape (parabola peaks at chain ~2.6, matching observed peak at 3, RMSE 0.012). Best direct estimate.
- Analog + MW/ALOGP correction (~0.50-0.57) converges on the same range as the own-series quadratic.

**Decision: target_qed = 0.54** (own-series quadratic extrapolation; convergent with MW-corrected analog; faithful to "predict from its OWN congeneric series").

## Task 2 — lead
m02 = 0.6046 (measured). Constraints check:
1. QED 0.6046 >= 0.6  PASS
2. nitro scaffold psa ~74 >= 40  PASS (psa constant within series; nitro = amide 29.1 + nitro 45.2 ~ 74)
3. m02 is scaffold 2 (nitro); m05 is scaffold 1 (methoxy) -> different series  PASS
qed_components(m02) confirms no hidden zero: only ALERTS=0.241 (the nitro alert) is weak; other 7 scores all >=0.83. Valid lead.

## Final answer.json
- target_qed: 0.54
- lead_mol_id: m02
- lead_qed_measured: 0.6046
