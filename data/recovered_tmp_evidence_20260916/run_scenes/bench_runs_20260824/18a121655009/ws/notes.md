# Task notes

## Setup
- Pool: 33 molecules, 4 scaffolds (from `group_by_scaffold`, free).
- Shared budget: 17 (list_pool pages + measure_qed). qed_components & fit_trend & group_by_scaffold are free.

## Scaffolds (structural grouping, free)
1. `COc1ccc(cc1)C(=O)N` — methoxy-benzoyl amide (m05's series)
   - members by chain_len: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11, UNSYNTHESIZED)
2. `O=[N+]([O-])c1ccc(cc1)C(=O)N` — nitro-benzoyl amide
   - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. `OCC(O)C(O)C(O)C(=O)N` — sugar amide
   - m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — biphenyl amide
   - m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Task 1 — predict m05 QED (chain 11, unsynthesized)
Measured the entire synthesizable series (chains 2–6):
- chain 2: m15 → 0.7621
- chain 3: m24 → 0.7919  (peak)
- chain 4: m28 → 0.7518
- chain 5: m17 → 0.7498
- chain 6: m22 → 0.7377
Predict at chain 11 via trend extrapolation + mechanistic sanity check (m05 descriptors).

## Task 2 — pick molecule: QED≥0.6, psa≥40, different scaffold from m05
- m05 scaffold = methoxy-benzoyl amide. Different scaffolds OK.
- psa is scaffold-determined (N-alkyl chain adds only CH2, no polar atoms).
  - Nitro scaffold psa ≈ 72.24 (from m02, p1) → ≥40 ✓, different scaffold ✓.
  - Sugar scaffold psa ≈ 110 → ≥40 ✓ but very polar (alogp<0), likely low QED.
  - Biphenyl scaffold psa ≈ 29.1 (from m04, p1) → <40 ✗.
- Strongest candidate: m02 (nitro, chain3: mw208, alogp1.735, rotb4, arom1, psa72.24) — very drug-like.

## Budget log
- After 5 measures (m15,m24,m28,m17,m22) + page1: used 6, remaining 11.
- +page2 + measure m02: used 8, remaining 9.

## m05 actual descriptors (from page 2)
- mw 305.46, alogp 4.956, psa 38.33, rotb 12, arom 1, smiles COc1ccc(cc1)C(=O)NCCCCCCCCCCC
- rotb formula per chain_len n ≈ n+1 (chain11→rotb12 ✓). So series rotb: ch2=3,ch3=4,ch4=5,ch5=6,ch6=7,ch11=12.
- alogp per chain ≈ 4.956 - (11-n)*0.5 → ch6≈2.46, ch2≈0.46.
- mw per chain ≈ 305.46 - (11-n)*14 → ch6≈235.5, ch2≈179.5.

## QED component breakdown (free) — m05's series, chains 2-6
chain | mol | MW     | ALOGP  | ROTB   | ALERTS | QED
2     | m15 | 0.3319| 0.8736 | 0.9927 | 0.8425 | 0.7621
3     | m24 | 0.4062| 0.9363 | 0.9697 | 0.8425 | 0.7919  (peak)
4     | m28 | 0.4914| 0.9768 | 0.8476 | 0.6397 | 0.7518
5     | m17 | 0.5853| 0.9973 | 0.6885 | 0.6397 | 0.7498
6     | m22 | 0.6836| 0.9973 | 0.5332 | 0.6397 | 0.7377
Constant across series: HBA 0.9305, HBD 0.9866, PSA 0.933, AROM 0.8273.

KEY MECHANISTIC INSIGHT for m05 (chain 11):
- ROTB score declines ~linearly -0.157/chain (ch4-7); at rotb 12 (ch11) it extrapolates
  BELOW 0 → floors ≈ 0. ROTB→~0 is the dominant drag (QED is a weighted GEOMETRIC mean,
  very sensitive to a near-zero component).
- ALOGP: ch5-6 score ~1.0 (logp ~2.5, near optimal). But m05 logp=4.956 — well past optimal,
  so ALOGP score DROPS (est ~0.5-0.6). The flat ch4-6 trend does NOT show this drop (ch6 logp
  only ~2.5). So a pure QED-trend extrapolation OVERESTIMATES.
- MW: ch6 (mw~235) score 0.6836, still rising; m05 mw305 near QED MW optimum → MW score
  higher (est ~0.8-0.9). Helps, partially offsets.
- ALERTS: stays 0.6397 (long-alkyl alert from ch4 onward; persists at ch11).
=> Mechanistic estimate of m05 QED ≈ 0.45-0.55 (below m22's 0.7377). See reconstruction below.

## Task 2 — RESULT
m02 (nitro scaffold, different from m05): QED = 0.6046 (≥0.6 ✓), psa 72.24 (≥40 ✓),
nitro scaffold ≠ m05's methoxy scaffold ✓. All three constraints satisfied. LOCKED.

## Mechanistic QED reconstruction (weighted geometric mean, Bickerton weights)
weights: MW0.49 ALOGP0.62 HBA0.39 HBD0.28 PSA0.42 ROTB0.50 AROM0.19 ALERTS0.59 (sum=3.48)
Validation vs measured (pred/actual ratio):
- m15: pred 0.789 vs 0.7621 (ratio 0.966)
- m24: pred 0.819 vs 0.7919 (ratio 0.967)
- m22: pred 0.780 vs 0.7377 (ratio 0.946)
~3-5% systematic overshoot → apply ~0.96 correction to m05 prediction.
m05 components (est): MW0.85, ALOGP0.55, HBA0.9305, HBD0.9866, PSA0.933, ROTB0.05, AROM0.8273, ALERTS0.6397
→ raw pred ≈ 0.514 → corrected ≈ 0.49.

## Polynomial fits at chain 11 (fit_trend, free)
- degree-2: pred 0.5416 (rmse 0.01188 on the 5 pts). Coeffs [0.74932, 0.017024, -0.003264].
- degree-1: pred 0.6950 (rmse 0.01308). Coeffs [0.79502, -0.00909].
- Both fit the observed points (chains 2-6) well. They diverge far at chain 11 because
  chains 2-6 are in the gentle / flat part of the ROTB-decline curve; only ROTB has gone
  truly nonlinear by chain 11 (floored at ~0), which neither trend captures.

## FINAL synthesis for m05 target_qed
Three independent lines of evidence bracket the answer:
- Polynomial: degree-2 0.542 (curvature-aware) and degree-1 0.695 (straight-line).
- Mechanistic reconstruction: raw 0.514, ~0.96 corrected → 0.49.
- Component-level reasoning: ROTB ~0 and ALOGP~0.55 floor the weighted-geometric-mean;
  the two sub-1 components (ROTB, ALERTS) plus a now-sub-1 ALOGP prevent QED from staying
  near 0.74.
 => converged estimate ≈ 0.50 (range 0.49-0.54). REPORT 0.50.

## Task 2 final
- lead_mol_id = m02
- lead_qed_measured = 0.6046 (real measure_qed call; ≥0.6 ✓, psa 72.24≥40 ✓, nitro scaffold≠m05 ✓)
