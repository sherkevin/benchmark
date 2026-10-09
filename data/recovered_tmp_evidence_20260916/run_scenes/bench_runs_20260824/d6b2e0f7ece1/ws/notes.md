# Notes — QED prediction & lead selection

## Pool structure (33 molecules, 4 congeneric series)
Each series = same scaffold, only N-alkyl chain length varies (C2..C10, plus m05 at C11).

- **Methoxy-benzamide** `COc1ccc(cc1)C(=O)N` — PSA 38.33:
  m15(C2), m24(C3), m28(C4), m17(C5), m22(C6), **m05(C11, target, unsynth)**
- **Nitro-benzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N` — PSA 72.24:
  m09(C2), m02(C3), m12(C4), m32(C5), m33(C6), m29(C7), m31(C8), m06(C9), m10(C10)
- **Biphenyl-benzamide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — PSA 29.1:
  m13(C2), m11(C3), m04(C4), m30(C5), m23(C6), m20(C7), m26(C8), m08(C9), m16(C10)
- **Sugar/polyol amide** `OCC(O)C(O)C(O)C(=O)N` — PSA 110.02:
  m14(C2), m27(C3), m01(C4), m19(C5), m03(C6), m21(C7), m18(C8), m07(C9), m25(C10)

## Job 1 — predict m05 (methoxy series, C11)
m05's series only has C2–C6 in pool → must extrapolate to C11. Measure the 5
methoxy congeners to fit the trend (curvature, not a straight line).

## Job 2 — backup lead
Constraints: measured QED ≥ 0.6 AND psa ≥ 40 AND NOT in m05's (methoxy) series.
PSA filter rules out methoxy (38.33) and biphenyl (29.1). Eligible families:
nitro (PSA 72.24) and sugar (PSA 110.02). Budget 12 total; 5 used on methoxy
series leaves up to 7 for lead search.

## Budget log
1. m15 (methoxy C2)  → 0.7621
2. m24 (methoxy C3) → 0.7919   ← peak
3. m28 (methoxy C4) → 0.7518
4. m17 (methoxy C5) → 0.7498
5. m22 (methoxy C6) → 0.7377

### Trend in methoxy series (m05's series)
QED rises C2→C3 (peak 0.7919 at C3), then declines: C3→C4 −0.040, C4→C5 −0.002,
C5→C6 −0.012. Nonlinear (peak then decline), so NOT a straight line.

### Extrapolation to m05 (methoxy C11)
Only C2–C6 exist in pool → extrapolate 5 CH2 beyond measured range.
m05 properties: MW 305.46, ALogP 4.956 (≈5, lipophilicity cutoff), PSA 38.33,
rotb 12 (well above optimal ~6), HBD 1, HBA 2, AROM 1, alerts 0.
vs m22 (C6, QED 0.7377): ALogP 3.0→4.96 (crosses cutoff), rotb 7→12, MW 235→305.
=> accelerating decline beyond C6 as d_ALogP & d_rotb drop sharply.

Fits tried:
- Quadratic QED(n)=0.7493+0.0170n−0.003264n² (peak at n≈2.6, matches observed):
  QED(11) ≈ 0.54. (tends to over-extrapolate curvature far from data)
- Linear post-peak (C3→C6 slope −0.0181/CH2): QED(11) ≈ 0.65.
- Threshold reasoning (d_ALogP@4.96≈0.6, d_rotb@12≈0.5, d_MW@305≈0.95):
  factor vs m22 ≈0.85 → QED ≈ 0.63.

Quadratic lower bound 0.50, linear upper ~0.65, threshold ~0.58-0.61.

**Why below the linear 0.65:** the gentle C4→C6 decline (−0.014 over 2 CH2) only
holds because no property is in a penalty zone in that range (ALogP 1.8–3.0,
rotb 4–7, MW 193–235 — all favorable). m05 (C11) LEAVES that favorable region:
ALogP 4.956 (at the ~5 lipophilicity cutoff), rotb 12 (well above favorable ~6–7),
MW 305. New penalty zones → decline must accelerate vs the measured C4–C6 slope.
Quadratic (0.50) over-extrapolates curvature far from data (vertex outside range),
so treat as a soft floor, not the prediction.

**Final m05 prediction ≈ 0.61** — central, below linear (accelerating decline) but
above the over-extrapolated quadratic.

## Job 2 — lead search (budget 7 left after methoxy)
Eligible: nitro (PSA 72.24) or sugar (PSA 110.02) series, not methoxy.
Nitro short-chain = best property profiles (low MW, ALogP ~1–2, low rotb).
Measure m02 (nitro C3) and m12 (nitro C4) as primary lead candidates.

### Lead measurements (nitro series, PSA 72.24 ✓ not methoxy ✓)
6. m02 (nitro C3) → **0.6046** ✓ (≥0.6)  ← peak of nitro series
7. m12 (nitro C4) → 0.4709 ✗
8. m09 (nitro C2) → 0.5826 ✗ (just under 0.6)
9. m32 (nitro C5) → 0.4683 ✗

Nitro QED vs chain: C2=0.5826, C3=0.6046 (peak), C4=0.4709, C5=0.4683.
Peak at C3, drops sharply after C3 → no better nitro lead exists beyond m02.
Short-chain sugar (PSA 110.02, very high MW/PSA, ALogP<0): expected low QED;
m02 already a confirmed pass, so no need to spend there.

### Lead decision
**Lead = m02** (nitro-benzamide C3), measured QED 0.6046.
- measured QED 0.6046 ≥ 0.6 ✓
- psa 72.24 ≥ 40 ✓
- not in m05's (methoxy) series ✓
3 measurements held in reserve (not needed).

## Final results
- target_qed (m05, methoxy C11, extrapolated) ≈ 0.61
- lead_mol_id = m02, lead_qed_measured = 0.6046
- congener_ids (methoxy series incl. m05) = [m15, m24, m28, m17, m22, m05]
