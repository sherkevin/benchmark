# Notes — QED prediction & lead selection

## Catalogue (33 molecules, paged in 9 pages → 9 budget used on listing)

Four congeneric series. All are Ar–C(=O)–N(alkyl) amides (or a sugar amide), differing
only in the length of the N-alkyl chain. ALogP is exactly linear in chain length within
each series (+0.39 per CH2), confirming the "only chain length differs" structure.

### Series A — 4-methoxybenzamide  (COc1ccc(cc1)C(=O)N… )  psa=38.33  ← m05's series
| mol  | chain | MW     | ALogP  | PSA   | ROTB | AROM |
|------|-------|--------|--------|-------|------|------|
| m15  | 2C    | 179.22 | 1.445  | 38.33 | 3    | 1    |
| m24  | 3C    | 193.25 | 1.835  | 38.33 | 4    | 1    |
| m28  | 4C    | 207.27 | 2.225  | 38.33 | 5    | 1    |
| m17  | 5C    | 221.30 | 2.615  | 38.33 | 6    | 1    |
| m22  | 6C    | 235.33 | 3.005  | 38.33 | 7    | 1    |
| m05  | 11C   | 305.46 | 4.956  | 38.33 | 12   | 1    | ← TARGET, never made

ALogP(2C→6C) = 1.445 + (chain−2)·0.39 exactly; projects to 4.956 at 11C = m05's value. ✓
So m05 = extrapolation of this series from chain 2–6 out to chain 11 (beyond measured range).

### Series B — 4-nitrobenzamide  (O=[N+]([O-])c1ccc(cc1)C(=O)N…)  psa=72.24  (NOT m05 series)
m09(2C) m02(3C) m12(4C) m32(5C) m33(6C) m29(7C) m31(8C) m06(9C) m10(10C)

### Series C — biphenyl-4-carboxamide  (c1ccc(cc1)-c1ccc(cc1)C(=O)N…)  psa=29.1
m13 m11 m04 m30 m23 m20 m26 m08 m16  → PSA 29.1 < 40, fails lead constraint 2.

### Series D — sugar amide  (OCC(O)C(O)C(O)C(=O)N…)  psa=110.02
m14 m27 m01 m19 m03 m21 m18 m07 m25  → ALogP all ≤ 0.3, very polar → low QED expected.

---

## Job 1 — predict m05's QED

### Why the 3 "hidden" QED properties are constant within Series A
QED weighs 8 properties; 5 are in the table (MW, ALogP, PSA, ROTB, AROM) and 3 are not
(H-bond donors HBD, H-bond acceptors HBA, structural alerts PAINS). Within Series A only
the N-alkyl chain changes, so:
- HBD = 0 (tertiary amide, no N–H) — constant
- HBA = 2 (amide carbonyl O + methoxy O) — constant
- alerts = 0 (clean para-methoxybenzamide) — constant
- PSA = 38.33, AROM = 1 — constant
So the QED variation along the series is driven entirely by MW, ALogP, ROTB, all of which
move monotonically with chain length. Extrapolating the fitted QED-vs-chain curve to 11C
is therefore well-founded (no hidden variable shifts).

### Measured trend (chain → QED)
| chain | mol  | QED     |
|-------|------|---------|
| 2     | m15  | 0.7621  |
| 3     | m24  | 0.7919  |  ← peak
| 4     | m28  | 0.7518  |
| 5     | m17  | 0.7498  |
| 6     | m22  | 0.7377  |

The curve is **non-monotonic**: it rises 2C→3C, peaks at 3C (good MW/ALogP/ROTB balance),
then declines as ALogP drifts past ~2.5 and ROTB past 6. First differences after the peak:
3→4: −0.0401 (peak-exit threshold step), 4→5: −0.0020, 5→6: −0.0121. Not a straight line.

### Fitting the declining arm and extrapolating to chain 11
Linear regression on the monotonic declining arm (chains 3–6): slope ≈ −0.0165/CH2,
intercept ≈ 0.832  →  QED(11) ≈ 0.832 − 0.0165·11 ≈ 0.651.
Equivalent: average post-peak decline −0.0181/CH2; from the last measured point
0.7377 (6C) over 5 more CH2: 0.7377 − 5·0.0181 ≈ 0.647.
Both agree on ≈ 0.65.

A downward parabola through all 5 points peaks at ~2.6C and extrapolates to ≈ 0.54 at 11C,
but it underfits the sharp peak (predicts 0.771 at the 3C peak vs measured 0.792) and a
downward parabola is unreliable for far extrapolation (it would go negative by ~chain 15),
so I do not trust its 11C value.

The desirability functions flatten toward their 0 floor far out, so the decline should not
accelerate without bound; the linear rate is a defensible central estimate. Allowing for
extra threshold steps between 6C and 11C (ROTB crossing 10, ALogP crossing 4, MW ~300) that
the short measured arm can't resolve, the true value could fall modestly below 0.65.

### Predicted QED for m05
**target_qed ≈ 0.65** (linear extrapolation of the observed declining trend to chain 11;
honest uncertainty band roughly 0.58–0.68).

---

## Job 2 — backup lead compound

Constraints: measured QED ≥ 0.6  AND  psa ≥ 40  AND  not in m05's series (Series A).

Free pre-screen (no measurement cost):
- Series A (methoxy, psa 38.33): excluded (it IS m05's series; also psa < 40).
- Series C (biphenyl, psa 29.1): excluded (psa < 40, and not m05's series anyway).
- Series D (sugar, psa 110, ALogP ≤ 0.3): eligible on psa but extremely polar → low QED.
- Series B (nitro, psa 72.24): eligible — psa ≥ 40 and not m05's series; ALogP 1.3–4.5 and
  ROTB 3–11, mid-chain members look drug-like. Best bet.

Measured Series B mid-chain members:
| mol  | chain | ALogP | ROTB | QED     | ≥0.6? |
|------|-------|-------|------|---------|-------|
| m02  | 3C    | 1.735 | 4    | 0.6046  | ✓     |
| m12  | 4C    | 2.125 | 5    | 0.4709  | ✗     |
| m32  | 5C    | 2.515 | 6    | 0.4683  | ✗     |

m02 is the only measured molecule meeting all three constraints:
- measured QED 0.6046 ≥ 0.6 ✓
- psa 72.24 ≥ 40 ✓
- 4-nitrobenzamide ≠ m05's 4-methoxybenzamide series ✓

→ **lead = m02, measured QED = 0.6046**.

---

## Budget reconciliation
- list_pool: 9 pages → 9 units (pages 1–9; page 9 had the last molecule m33).
- measure_qed: m15, m24, m28, m17, m22 (Job 1 = 5) + m12, m32, m02 (Job 2 = 3) = 8.
- Total = 17/17. Exactly on budget. No probe_unavailable calls were made (m05 was never
  measured — its QED is the prediction, by design).

## Deliverable values
- target_qed: 0.65
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids: m15, m24, m28, m17, m22  (the made pool molecules in m05's Series A;
  m05 itself is the prediction target, chain 11, never synthesized)
