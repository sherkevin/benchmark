# Task notes

## Pool structure (4 scaffolds, each varying only in N-alkyl chain length c)

| Scaffold | psa | Members (chain → mol_id) |
|---|---|---|
| Methoxyphenyl `COc1ccc(cc1)C(=O)N` (m05's series) | 38.33 | c2 m15, c3 m24, c4 m28, c5 m17, c6 m22, **c11 m05 (target)** |
| Biphenyl `c1ccc(cc1)-c1ccc(cc1)C(=O)N` | 29.1 | c2 m13, c3 m11, c4 m04, c5 m30, c6 m23, c7 m20, c8 m26, c9 m16 |
| Nitrophenyl `O=[N+]([O-])c1ccc(cc1)C(=O)N` | 72.24 | c2 m09, c3 m02, c4 m12, c5 m32, c6 m33, c7 m29, c8 m31, c9 m06, c10 m10 |
| Glycamide `OCC(O)C(O)C(O)C(=O)N` | 110.02 | c2 m14, c3 m27, c4 m01, c5 m19, c6 m03, c7 m18, c8 m21, c9 m07, c10 m25 |

QED is driven by alogp (MW and TPSA differ within a series): the methoxyphenyl
series has the lowest PSA among the "valid" (psa≥40) series and is m05's own.

## Task 1 — QED of m05 (methoxyphenyl, c11, never synthesized)

Measured the other 5 methoxyphenyl members (chain → QED):

| chain | mol | alogp | QED |
|---|---|---|---|
| 2 | m15 | 1.445 | 0.7621 |
| 3 | m24 | 1.835 | **0.7919 (peak)** |
| 4 | m28 | 2.225 | 0.7518 |
| 5 | m17 | 2.615 | 0.7498 |
| 6 | m22 | 3.005 | 0.7377 |

Series peaks at c3 then declines monotonically (c3→c6). m05 is c11, well past
the peak, so interpolate the decline out to chain 11.

Degree-1 fit on the c3–c6 decline (the post-peak regime m05 lives in):
- slope ≈ -0.0165/chain, rmse 0.0088
- predicted QED at c11 = **0.6508**

(Degree-2 over all five points gives 0.5416, but that fit is pulled down by
the c2→c3 rise it must also accommodate; the post-peak linear regime is the
more appropriate extrapolation for a point at c11.)

**target_qed (m05) ≈ 0.65**

## Task 2 — pick one molecule satisfying all three constraints

Constraints: QED ≥ 0.6 AND psa ≥ 40 AND not m05's series.

- m05's series (methoxyphenyl) is excluded.
- Biphenyl: psa 29.1 < 40 → excluded.
- Valid-series candidates: nitrophenyl (psa 72.24) and glycamide (psa 110.02).

Strategy: find the chain length where each valid series peaks (alogp near the
drug-like optimum ~1.8, where methoxyphenyl peaked at c3).

Measurements (budget 12 total, shared with Task 1):

| series | mol | chain | alogp | QED | ≥0.6? |
|---|---|---|---|---|---|
| nitro | m09 | 2 | 1.345 | 0.5826 | no |
| nitro | m12 | 4 | 2.125 | 0.4709 | no |
| nitro | m32 | 5 | 2.515 | 0.4683 | no |
| nitro | m33 | 6 | 2.905 | 0.4591 | no |
| nitro | **m02** | **3** | **1.735** | **0.6046** | **YES** |
| glycamide | m03 | 6 | -1.242 | 0.3282 | no |
| glycamide | m25 | 10 | 0.318 | 0.3148 | no |

Glycamide series stays near 0.3 (PSA 110 is too polar). Nitrophenyl series
peaks at low chain length near alogp ~1.7. **m02** (nitrophenyl, c3) lands at
QED 0.6046 — just above the 0.6 bar, psa 72.24 ≥ 40, different scaffold from
m05. ✓ all three constraints.

## Budget accounting
12 measurements used: m15, m24, m28, m17, m22 (Task 1 trend) + m09, m12, m32,
m33, m02, m03, m25 (Task 2 search).

## Deliverables
- target_qed (m05) ≈ 0.65  (from post-peak linear fit of m05's own congeneric series)
- lead_mol_id = m02
- lead_qed_measured = 0.6046
