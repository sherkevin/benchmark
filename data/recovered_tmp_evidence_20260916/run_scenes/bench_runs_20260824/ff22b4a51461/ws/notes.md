# Notes

## Task overview
- Budget: 12 units total, shared by list_pool (per page) and measure_qed (per measurement), across both tasks.
- Task 1: report QED of m05 (scaffold `COc1ccc(cc1)C(=O)N`, chain_len 11). m05 unsynth → must predict from trend of its OWN congeneric series.
- Task 2: pick ONE molecule with measured QED ≥ 0.6, psa ≥ 40, NOT in m05's scaffold.

## group_by_scaffold (free) results
4 scaffolds:
- SC1 `COc1ccc(cc1)C(=O)N` (methoxy+amide) = m05's series: m15(2), m24(3), m28(4), m17(5), m22(6), m05(11). [m05 unsynth]
- SC2 `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro+amide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10). PSA ~ nitro+amide (high).
- SC3 `OCC(O)C(O)C(O)C(=O)N` (polyol/sugar+amide): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10). PSA very high, HBD high.
- SC4 `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl+amide): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10). PSA ~ just amide (~43, borderline).

## Task 1 measurements (SC1, m05's series)
chain → QED:
- m15 (2): 0.7621
- m24 (3): 0.7919  ← peak
- m28 (4): 0.7518
- m17 (5): 0.7498
- m22 (6): 0.7377
m05 = chain 11 (unsynth) → predict via polynomial trend.
Budget after Task 1: 5 used. 7 left.

## Task 1 trend fit (free, fit_trend)
Quadratic (deg 2, default): prediction at x=11 = **0.5416**, RMSE on points 0.01188.
  coeffs: 0.74932 + 0.017024·x − 0.003264·x²  (peak at x≈2.6, then concave-down)
Linear (deg 1, sensitivity): prediction at x=11 = 0.695, RMSE 0.01308.
→ Chose QUADRATIC: lower RMSE, and correctly captures rise-then-peak-then-fall shape
  (data rises 2→3 then falls 3→6). Linear is monotonic-decreasing, misses the peak.
  Also: chain 11 = far extrapolation; quadratic's negative curvature reflects real
  drug-likeness degradation at long chains (high alogp/rotb). Linear 0.695 looks optimistic.
  Chemically m05: long C11 N-alkyl → alogp ~5+, rotb ~12 → MW/ALOGP/ROTB scores all
  worse than short-chain members (0.74-0.79); 0.5416 sits in expected 0.5-0.65 band.

## Page 1 descriptors (budget +1 → 6 used, 6 left)
- m01 (SC3 ch4): mw 221.25, alogp -2.022, psa 110.02, rotb 7, arom 0
- m02 (SC2 ch3): mw 208.22, alogp 1.735, psa 72.24, rotb 4, arom 1
- m03 (SC3 ch6): mw 249.31, alogp -1.242, psa 110.02, rotb 9, arom 0
- m04 (SC4 ch4): mw 253.34, alogp 3.884, psa 29.10, rotb 5, arom 2

## PSA per scaffold (constant within scaffold — only N-alkyl chain changes, alkyl has no N/O)
- SC1: ~ (excluded; it's m05's own series)
- SC2: 72.24 (nitro+amide) ≥40 ✓  ← VIABLE
- SC3: 110.02 (polyol) ≥40 ✓ but negative alogp + high HBD/PSA → low QED
- SC4: 29.10 < 40 ✗ → ENTIRE SC4 FAILS Task 2 constraint 2
⇒ Only SC2 viable for Task 2 (SC3 scores too low, SC4 fails psa, SC1 excluded).

## Task 2 measurements (SC2, nitro+amide, psa 72.24, NOT SC1)
chain → QED (declines with chain length):
- m09 (2): 0.5826
- m02 (3): 0.6046  ← peak, ≥0.6 ✓
- m12 (4): 0.4709
- m32 (5): 0.4683
Budget used: 10/12. 2 left (unused — answer is solid).

## m02 component breakdown (qed_components, free)
MW 0.4975, ALOGP 0.9223, HBA 0.9848, HBD 0.9866, PSA 0.9302, ROTB 0.9697, AROM 0.8273,
ALERTS 0.2414. → Nitro triggers ALERTS (only drag); other 7 scores 0.83-0.99. QED=0.6046.

## DECISIONS
- Task 1 (target_qed): 0.5416  (quadratic fit of SC1 trend, evaluated at chain 11)
- Task 2 (lead): m02, measured QED 0.6046.
  Constraints check: (1) 0.6046 ≥ 0.6 ✓  (2) psa 72.24 ≥ 40 ✓  (3) SC2 ≠ SC1 (m05's) ✓
PSA is constant within a scaffold (only the N-alkyl chain changes, alkyl has no N/O). So reading ONE member's psa gives psa for the whole scaffold.

## Plan
- Task 1: measure all 5 synth members of SC1 (m15,m24,m28,m17,m22) → fit trend (chain vs QED), evaluate at chain 11. = 5 units.
- Task 2: read page(s) to get psa for non-SC1 candidates; measure most drug-like candidate(s); pick one with QED≥0.6 & psa≥0.6 & psa≥40 & not SC1.
- For Task 2 high-QED candidate: prefer low chain (low MW) in a scaffold with good descriptors. SC2 (nitro) m09/m02 low chain → drug-like but nitro may trigger ALERTS. SC4 (biphenyl) borderline psa + high AROM. SC3 high HBD/PSA may hurt.
