# Notes — predict m05 QED & pick a lead

## Catalogue (33 mols) — scaffold series
Target **m05**: SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, scaffold `COc1ccc(cc1)C(=O)N`
(4-methoxybenzamide), **chain_len = 11** (never synthesised → probe_unavailable).

Four congeneric series exist (same scaffold, only N-alkyl chain length differs):

### Series A — methoxyphenyl amide, scaffold `COc1ccc(cc1)C(=O)N` (psa 38.33) — TARGET's series
- m15 chain2  m24 chain3  m28 chain4  m17 chain5  m22 chain6  | m05 chain11 (target)
- psa 38.33 < 40 → NONE pass the psa screen (so no A-member can be the lead).

### Series B — nitrophenyl amide, scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` (psa 72.24)
- m09 ch2  m02 ch3  m12 ch4  m32 ch5  m33 ch6  m29 ch7  m31 ch8  m06 ch9  m10 ch10

### Series C — aldose/polyol amide, scaffold `OCC(O)C(O)C(O)C(=O)N` (psa 110.02)
- m14 ch2  m27 ch3  m01 ch4  m19 ch5  m03 ch6  m21 ch7  m18 ch8  m07 ch9  m25 ch10

### Series D — biphenyl amide, scaffold `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (psa 29.10)
- m13 ch2  m11 ch3  m04 ch4  m30 ch5  m23 ch6  m20 ch7  m26 ch8  m16 ch9(+?) ...
- psa 29.10 < 40 → fails screen, ineligible as lead.

## screen_candidates (free, psa≥40): 18 mols
All from series B (nitro) + series C (sugar). None from m05's series A → constraint 3
("not in m05's series") is automatically satisfied for every screened candidate.

## Budget tracking
- 9 used paging list_pool (9 pages × 4 = 33 mols).
- 5 used measuring m05's congeners (m15,m24,m28,m17,m22).
- = 14 used, 3 remaining for lead measurements.
- fit_trend / describe_structure / screen_candidates are FREE.

## Job 1 — m05 series measurements (chain_len → QED)
| chain | mol | QED    |
|------|-----|--------|
| 2    | m15 | 0.7621 |
| 3    | m24 | 0.7919 |  ← peak
| 4    | m28 | 0.7518 |
| 5    | m17 | 0.7498 |
| 6    | m22 | 0.7377 |
| 11   | m05 | ??? (extrapolate, beyond measured range 2–6) |

Trend: rises 2→3, peaks at 3, then declines 3→4→5→6. As chain grows, MW & alogp
& rotb all rise → QED falls. Extrapolating to 11 should give a lower value.

### Extrapolation fit (x_query = 11, the target's chain length)
- **degree 2**: QED(m05) = **0.5416**, rmse 0.01188. Coeffs (low→high):
  0.74932 + 0.017024·x − 0.003264·x². → *chosen prediction*.
- degree 3: QED = 2.1323 — physically impossible (>1), the cubic overfits the small
  wiggle (peak at 3, near-flat 4→5) and blows up far outside the measured range
  [2,6]. Rejected.

The data peak at chain 3 then a steady gentle decline through 6; a downward
parabola is the right shape and extrapolating it to 11 gives ~0.54. m05 is far
past the measured range so this is an extrapolation, as the task warns — but it
matches the monotone decline the longer chains are on.

**target_qed = 0.5416**

## Job 2 — lead selection
Need: measured QED ≥ 0.6 AND psa ≥ 40 AND not series A.
All 18 screen passes are in series B (nitro, psa 72) or C (sugar, psa 110) —
none are m05's series A, so constraint 3 holds automatically for all of them.
Only the measured-QED gate remains.

Measurements (3 left after Job 1):
- m09 (nitro ch2, MW194 alogp1.3 rotb3): QED = **0.5826** → below 0.6, FAILS.
- m02 (nitro ch3, MW208 alogp1.7 rotb4, psa 72.24): QED = **0.6046** → ≥0.6, PASSES.

m02 satisfies all three constraints:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. scaffold `O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's scaffold ✓

**lead_mol_id = m02, lead_qed_measured = 0.6046**

Budget used: 9 (paging) + 5 (Job 1) + 2 (m09, m02) = 16 of 17. One spare.

## congener_ids (m05's series A, all measured)
["m15", "m24", "m28", "m17", "m22"]  (chain lengths 2,3,4,5,6)
