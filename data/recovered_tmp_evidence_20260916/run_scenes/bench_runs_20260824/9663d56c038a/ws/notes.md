# Task notes

## Pool structure (33 molecules, 4 congeneric series)

Identified via SMILES scaffolds (confirmed with `describe_structure`):

| series | scaffold | members (chain_len) | psa |
|---|---|---|---|
| OMe-benzamide (m05's) | `COc1ccc(cc1)C(=O)N` | m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target)** | 38.33 |
| NO2-benzamide | `O=[N+]([O-])c1ccc(cc1)C(=O)N` | m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10) | 72.24 |
| sugar | `OCC(O)C(O)C(O)C(=O)N` | m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10) | 110.02 |
| biphenyl | `c1ccc(cc1)-c1ccc(cc1)C(=O)N` | m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10) | 29.1 |

## Job 1 — predict m05 QED

- m05 scaffold = `COc1ccc(cc1)C(=O)N`, chain_len 11 (target, never synthesized → probe_unavailable).
- Same-series pool members (short chain): m15(2), m24(3), m28(4), m17(5), m22(6).
- Pool only holds short-chain members (chain ≤6); m05 at chain 11 is an **extrapolation** beyond the measured range.
- Plan: measure all 5 short-chain members, fit a degree-2 polynomial in chain_len, evaluate at x=11.

## Job 2 — lead (QED≥0.6 AND psa≥40 AND not m05's series)

- psa≥40 free screen (screen_candidates) passes 18 molecules: all NO2 + sugar series. Both the OMe (m05) and biphenyl series fail psa (38.33 and 29.1 < 40) → automatically excluded.
- Constraint 3 (not m05's series): all 18 passing candidates are NO2 or sugar ≠ OMe scaffold → automatically satisfied.
- So the lead must be a NO2- or sugar-series member with measured QED ≥ 0.6.
- Drug-likeness reasoning: sugar series has alogp -2.8…+0.3 and psa 110 (both poor for QED); NO2 series has alogp 1.3…4.5, psa 72, arom 1 — far more drug-like. The lead is almost certainly in the NO2 series. (Plain aromatic nitro is not in RDKit QED's default alert list, so NO2 members are not penalty-flagged.)
- NO2 series best QED expected near alogp ~2.5-3 / rotb ≤6 → chains 4-6 (m12, m32, m33). m32 (alogp 2.515, rotb 6, mw 236) is the single best-looking lead.

## Budget
- 17 total, 9 used paging the catalogue → 8 left.
- Allocation: 5 trend measurements + up to 3 lead measurements.

## Measurement plan
- Trend (batch): m15, m24, m28, m17, m22 (chains 2,3,4,5,6).
- Lead first: m32 (best NO2 profile) + m09 (low-chain hedge). Reserve: m33/m12.

## Results so far

Trend (m05 / OMe series, chain → QED):
- chain2 m15 = 0.7621
- chain3 m24 = 0.7919  ← peak
- chain4 m28 = 0.7518
- chain5 m17 = 0.7498
- chain6 m22 = 0.7377
Shape: rises to a peak at chain3 then slowly falls. Extrapolating to chain11 (m05) → downward arm, QED well below 0.7.

Lead (NO2 series, the only non-excluded eligible series besides sugar):
- chain5 m32 = 0.4683 ✗ (<0.6)
- chain2 m09 = 0.5826 ✗ (<0.6)
NO2 is structurally limited vs OMe (PSA 72 vs 38; nitro adds 2 HBA). OMe–NO2 gap at chain2 ≈ 0.18, at chain5 ≈ 0.28 (gap grows with chain). So NO2 peak ≈ 0.6-ish at best, near chain 2-3.

## Last measurement (budget 16/17 → 1 left)
Best remaining shot at crossing 0.6: m02 (NO2 chain3) — the NO2 analogue of m24 (the OMe peak). m09 chain2=0.5826 and OMe pattern peaks at chain3, so m02 (alogp 1.735 near QED-optimal, HBD1, HBA3, rotb4, MW208) is the single most likely NO2 member to reach ≥0.6. Sugar series rejected (alogp −2.8, 5 HBD, PSA110 → QED certainly <0.5). Measure m02.

## Final measurements

- m02 (NO2, chain3) = **0.6046** ✓ ≥0.6. Budget 17/17 exhausted.

**Lead = m02**: measured QED 0.6046 ≥ 0.6 ✓; psa 72.24 ≥ 40 ✓; scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's `COc1ccc(cc1)C(=O)N` ✓ (different series). All three constraints satisfied.

## Job 1 — extrapolation of m05 QED (chain 11)

Fit the 5 measured OMe-series points (chains 2–6) and evaluate at x=11. The data are clearly nonlinear: 0.7621 → 0.7919 (peak at chain3) → 0.7518 → 0.7498 → 0.7377, i.e. rise-then-decay. Per the task, do NOT assume a straight line.

Degree-2 fit (quadratic): prediction at x=11 = **0.5416**, RMSE on the 5 points = 0.01188.
Coefficients (low→high): 0.74932 + 0.017024·x − 0.003264·x².

Compare degree-1 (linear): 0.695, RMSE = 0.01308 (worse fit).

Choosing degree-2 because:
1. Lower RMSE → better captures the observed points.
2. The data genuinely curve (peak then decline); the linear fit ignores the peak and the task explicitly warns against assuming a straight line.
3. Chemically consistent: m05's alogp = 4.956 (far above the QED-desirable ~2–3 range) and rotb = 12 (well past desirable). Both penalties grow superlinearly with chain length, so the decline should accelerate, not stay linear. The quadratic's accelerating decline (down to ~0.54) is chemically reasonable; the linear's shallow 0.695 is too optimistic given alogp/rotb excursions.

**target_qed (m05, predicted) = 0.5416** (extrapolation of its own series' degree-2 trend to chain 11).

## congener_ids (pool molecules in m05's OMe series, chain differs from target)
m15 (chain2), m24 (chain3), m28 (chain4), m17 (chain5), m22 (chain6). (m05 itself is the unsynthesized target, excluded.)
