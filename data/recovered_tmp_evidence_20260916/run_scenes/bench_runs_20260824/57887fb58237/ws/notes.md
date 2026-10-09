# Notes — QED prediction & lead selection task

## Data analysis: identify congeneric series

The pool is 33 molecules. Each series = same scaffold, only N-alkyl chain length differs.
Reading SMILES, four scaffolds:

- **Series A — methoxybenzamide** `COc1ccc(cc1)C(=O)N(Cn)` (psa=38.33, arom=1):
  - C2=m15, C3=m24, C4=m28, C5=m17, C6=m22
  - **m05 = C11 (NCCCCCCCCCCC) — the TARGET, never synthesized**
  - The pool only holds C2–C6 of this series, so m05 (C11) is an EXTRAPOLATION beyond the measured range.

- **Series B — nitrobenzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N(Cn)` (psa=72.24, arom=1):
  - C2=m09, C3=m02, C4=m12, C5=m32, C6=m33, C7=m29, C8=m31, C10=m06, C11=m10

- **Series C — biphenylbenzamide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N(Cn)` (psa=29.1, arom=2):
  - C2=m13, C3=m11, C4=m04, C5=m30, C6=m23, C7=m20, C8=m26, C10=m08, C11=m16

- **Series D — glycero-amide** `OCC(O)C(O)C(O)C(=O)N(Cn)` (psa=110.02, arom=0):
  - C2=m14, C3=m27, C4=m01, C5=m19, C6=m03, C7=m21, C8=m18, C10=m07, C11=m25

## Job 2 — constraint pre-filter (free, before spending)

Lead must satisfy: (1) measured QED ≥ 0.6, (2) psa ≥ 40, (3) not series A.
- Series A: excluded by (3); also psa=38.33<40.
- Series C: psa=29.1 < 40 → EXCLUDED.
- Series B: psa=72.24 ≥ 40 ✓ eligible.
- Series D: psa=110.02 ≥ 40 ✓ eligible (but logP<0, psa=110, 4 HBD, 0 arom — heavy liabilities).
→ Lead must come from series B or D.

## Budget: 12 shared. Used 5 (Job 1) + 3 (Job 2) = 8. Stopped with 4 unused (m02 already qualifies; no measurement can improve Job 1 since all 5 pool methoxy congenerics are already measured and m05 is unsynthesizable).

## Job 1 — measured methoxy series (m05's congeneric series), C2–C6

| mol_id | chain | MW | alogp | rotb | QED (measured) |
|--------|-------|-----|-------|------|----------------|
| m15 | C2 | 179 | 1.45 | 3 | 0.7621 |
| m24 | C3 | 193 | 1.84 | 4 | **0.7919 (peak)** |
| m28 | C4 | 207 | 2.23 | 5 | 0.7518 |
| m17 | C5 | 221 | 2.62 | 6 | 0.7498 |
| m22 | C6 | 235 | 3.01 | 7 | 0.7377 |

**Trend observed:** QED rises C2→C3 to a peak at C3 (0.7919), then declines steadily through C6 (0.7377). Net movement C2→C6 is downward. This is NOT a straight line — it is an inverted-U (rise then fall), so a linear model is inappropriate. The post-peak decline is the regime relevant to extrapolating to C11 (well past the peak).

**Fits (n = carbon count):**
- Quadratic (nonlinear, all 5 points): QED ≈ 0.7652 − 0.00909·(n−4) − 0.003264·(n−4)². → at n=11: **0.542**. Residuals small (RMS ~0.011); the curve slightly underfits the sharp C3 peak, so its accelerating decline is a reasonable, slightly aggressive, lower-bound extrapolation.
- Linear on the post-peak decay phase (n=3..6): slope ≈ −0.0165/step → at n=11: 0.651. This is the upper bound (assumes constant, non-accelerating decline).
- Linear excluding the C3 spike (n=2,4,5,6): slope ≈ −0.0055/step → ~0.71 (too gentle; ignores that C11 enters steep penalty zones, so rejected as an upper bound).

**Why accelerate the decline toward C11?** At C11 (m05: MW=305, ALOGP=4.96, rotb=12) the descriptors cross into steeper QED-penalty zones (MW>300, logP≈5, rotb>10) that the measured range (C2–C6: MW≤235, logP≤3, rotb≤7) does not sample. So the decline should steepen relative to the measured rate — favoring the lower (quadratic) side rather than the linear upper bound. Also, the large single drop C3→C4 (0.040) shows this QED is sensitive to small descriptor changes, supporting a real decline at C11.

**Extrapolation decision:** Bracket is ~0.54 (quadratic) to ~0.65 (linear-on-decay). The task explicitly warns against assuming a straight line and against quoting the nearest measured value (0.738 at C6), so I lean toward the nonlinear/curved side. Central estimate, weighting the quadratic more heavily than the linear upper bound:

### target_qed (m05, C11) ≈ 0.58

This is below every measured value in the series (correctly reflecting downward extrapolation past the peak), between the quadratic lower bound and the linear upper bound, tilted toward the nonlinear model per the task's guidance.

## Job 2 — measured nitro (series B) candidates

| mol_id | chain | psa | QED (measured) | ≥0.6? |
|--------|-------|-----|----------------|-------|
| m09 | C2 | 72.24 | 0.5826 | ✗ |
| m02 | C3 | 72.24 | **0.6046** | ✓ |
| m12 | C4 | 72.24 | 0.4709 | ✗ |

The nitro series peaks at C3 (m02). Longer chains decline (m12 C4 already 0.47); shorter (m09 C2) is 0.583. So m02 is the nitro peak and the best lead from series B. Series D (glycero) was not measured: its extreme polarity (logP<0, psa=110, HBD=4, 0 arom) makes QED≥0.6 very unlikely, and m02 already satisfies all constraints.

## Lead selection

**lead = m02**:
- measured QED = 0.6046 ≥ 0.6 ✓
- psa = 72.24 ≥ 40 ✓
- not in series A (it is nitro, series B) ✓

All three constraints satisfied.

## Deliverables

- target_qed = 0.58 (predicted m05 QED, extrapolated from methoxy series C2–C6)
- lead_mol_id = "m02"
- lead_qed_measured = 0.6046 (real measure_qed call)
- congener_ids = ["m15","m24","m28","m17","m22"] (the 5 pool methoxy-series members, C2–C6, that differ from m05 only in N-alkyl chain length)
