# Notes

## Budget tracking
- Total budget: 12 (shared by list_pool + measure_qed, both tasks).
- list_pool pages 1-9 read = 9 units spent. **3 units remaining.**
- 33 molecules total. m05 is the (unsynthesized) target; never made → measure_qed returns probe_unavailable (costs 0).

## m05 congeneric series (methoxybenzamide scaffold: `COc1ccc(cc1)C(=O)N(C)n`)
Members differ only in N-alkyl chain length (carbons after N):
| mol | chain C | mw    | alogp  | psa   | rotb | arom |
|-----|---------|-------|--------|-------|------|------|
| m15 | 2       | 179.22| 1.445  | 38.33 | 3    | 1    |
| m24 | 3       | 193.25| 1.835  | 38.33 | 4    | 1    |
| m28 | 4       | 207.27| 2.225  | 38.33 | 5    | 1    |
| m17 | 5       | 221.30| 2.615  | 38.33 | 6    | 1    |
| m22 | 6       | 235.33| 3.005  | 38.33 | 7    | 1    |
| m05 | 11      | 305.46| 4.956  | 38.33 | 12   | 1    |  ← unsynthesized

Within this series only mw / alogp / rotb change; psa=38.33, arom=1, HBD=1, HBA=2, alerts=0 are constant.
As chain grows → rotb↑, alogp↑ → QED should DECREASE. m05 (rotb 12, alogp 4.956) is at the high/penalized end.

## Other series (psa ≥ 40 candidates for Task 2)
Constraint 2 needs psa ≥ 40. Only two non-m05 series qualify:
- **Nitrobenzamides** `O=[N+]([O-])c1ccc(cc1)C(=O)N(C)n`, psa=72.24:
  m09(2C,rotb3,mw194,alogp1.345), m02(3C,rotb4,mw208,alogp1.735), m12(4C,rotb5,mw222,alogp2.125),
  m32(5C), m33(6C), m29(7C), m31(8C), m06(9C), m10(10C).
- **Sugar-amides** `OCC(O)C(O)C(O)C(=O)N(C)n`, psa=110.02 (logP very negative → likely lower QED):
  m14,m01,m27,m19,m03,m21,m18,m07,m25.
- Biphenyl series (psa 29.1) and m05 series (psa 38.33) fail constraint 2.

None of the psa≥40 candidates are in the m05 series → constraint 3 auto-satisfied.

## Plan (only 3 units left)
- Task 2: measure a nitrobenzamide (best drug-like descriptors). Pick **m02** (mw 208 near peak, logP 1.735 ideal, rotb 4, psa 72, arom 1) — most likely QED ≥ 0.6. Uncertainty = whether aromatic nitro is a QED alert.
- Task 1: measure 2 m05-series anchors to get the chain-length trend, then extrapolate to 11C.

Adaptive order: measure m02 first (confirm Task 2). If ≥0.6, spend remaining 2 on m05-series anchors m15(2C) + m22(6C). If m02 fails (nitro disqualifier), pivot remaining 2 units to a sugar-amide + one Task-1 anchor.

## MEASUREMENTS (budget now 12/12 — exhausted)
- m02 (nitrobenzamide 3C): **QED = 0.6046** ✓ Task-2 candidate (psa 72.24≥40, not m05-series)
- m15 (m05-series, 2C):  QED = 0.7621
- m22 (m05-series, 6C):  QED = 0.7377

## Task 1 — m05 (11C) extrapolation from the series
Same scaffold, only chain length differs → only mw, alogp, rotb change; psa=38.33, arom=1, HBD=1, HBA=2, alerts=0 constant → those QED contributors identical across the series.

Trend observed (2C→6C): QED 0.7621 → 0.7377 (gentle decline as chain grows).
m05 (11C) continues the trend but no cliff because:
- rotb 12 ≥ 10 floor → same weight as m22's rotb 7 (ROTB fn floored for both).
- mw 305 just past 300 penalty onset → slightly below m22's weight, but MW fn barely penalized there.
- alogp 4.956 vs 3.005 (m22) → AlogP desirability lower (peak ~2-3), the *largest* declining contributor.
- All other contributors constant.

Geometric mean dominated by lowest weights; rotb already floored at m22 and unchanged at m05, so the decline from 0.7377 is driven mostly by the single alogp term moving ~3→5 (gradual). Estimate **m05 QED ≈ 0.6** — clearly below m22 (0.7377) from the logP/MW drift, clearly above m02's nitro baseline (0.6046).

## Task 2 — answer
Lead = **m02**, measured QED = **0.6046** (≥0.6 ✓, psa 72.24 ≥40 ✓, nitrobenzamide ≠ m05 methoxybenzamide series ✓).
