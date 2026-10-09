# Notes — predict m05 QED & pick a backup lead

## Pool layout (via describe_structure / SMILES reading)

Four congeneric series, each = one scaffold + an N-alkyl chain of varying length:

| series | scaffold | psa | members (chain length) |
|---|---|---|---|
| methoxy-phenamide (TARGET series) | `COc1ccc(cc1)C(=O)N` | 38.33 | m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, unmade)** |
| nitro-phenamide | `O=[N+]([O-])c1ccc(cc1)C(=O)N` | 72.24 | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) |
| sugar-amide | `OCC(O)C(O)C(O)C(=O)N` | 110.02 | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) |
| biphenyl-phenamide | `c1ccc(cc1)-c1ccc(cc1)C(=O)N` | 29.1 | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) |

## Job 1 — predict m05 QED (chain 11)

m05 was never synthesized → `measure_qed` returns probe_unavailable; must extrapolate from its OWN series. The pool only holds short-chain members (chains 2–6), so this is extrapolation beyond the measured range, not interpolation.

Measured the 5 measurable congeners (budget 5):

| mol | chain | QED |
|---|---|---|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

Shape: rises to a peak at chain 3 (0.7919) then declines steadily — a curve, not a line. Per task instructions, fitted the observed trend with a degree-2 polynomial (do NOT assume a straight line, do NOT quote the nearest point).

`fit_trend(xs=[2,3,4,5,6], ys=[0.7621,0.7919,0.7518,0.7498,0.7377], x_query=11, degree=2)`:
- coefficients (low→high): 0.74932, 0.017024, -0.003264
- RMSE on given points: 0.01188
- **prediction at chain 11 = 0.5416**

The negative quadratic term dominates at large chain length, pulling QED well below the nearest measured point (m22=0.7377) — expected, since longer chains hurt drug-likeness (MW/rotb/alogp all rise, PSA fixed low). Prediction for m05 ≈ **0.542**.

congener_ids (same scaffold, chain length differs from m05): m15, m24, m28, m17, m22.

## Job 2 — backup lead (QED≥0.6 AND psa≥40 AND not m05's series)

Free pre-filters first:
- psa≥40 screen (`screen_candidates`): 18 pass. Drops the entire biphenyl series (psa 29.1) — those are out regardless of QED.
- "not m05's series": drops all methoxy members (m15,m17,m22,m24,m28 and m05). Remaining candidates are nitro (psa 72.24) and sugar (psa 110.02) series.
- Sugar series has very high PSA + strongly negative alogp (≤ -0.46 at usable chains) → expect low QED. Measured m27 (chain 3, the shortest sugar) = 0.333 → confirms sugar series is too low; abandoned.
- Nitro series: measured shortest members to find its peak.
  - m09 (ch2) = 0.5826
  - m02 (ch3) = 0.6046  ← peak, just clears 0.6
  - m12 (ch4) = 0.4709
  - Nitro series peaks at chain 3 and declines on both sides, so no other nitro member beats m02.

**Lead = m02**: measured QED 0.6046 (≥0.6 ✓), psa 72.24 (≥40 ✓), nitro scaffold ≠ methoxy scaffold of m05 (not in target's series ✓). All three constraints satisfied.

## Budget

Used 9/12: 5 (m05 series) + 4 lead probes (m09, m12, m27, m02). 3 left unspent — m02 is the best available lead (nitro peak, sugar too low, biphenyl fails psa), so no need to spend more.

## Answer
- target_qed = 0.5416
- lead_mol_id = m02
- lead_qed_measured = 0.6046
- congener_ids = [m15, m24, m28, m17, m22]
