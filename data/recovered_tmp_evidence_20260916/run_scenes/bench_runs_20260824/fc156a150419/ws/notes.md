# Task notes

## Setup
- Budget: 12 `measure_qed` calls total, shared by Job 1 (predict m05 QED) and Job 2 (pick lead).
- m05 = `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, chain length 11, never synthesized → probe_unavailable.

## Identifying congeneric series (by scaffold, N-alkyl chain varies)

Counting carbons after `N` in each SMILES:

- **Series D — methoxybenzamide** `COc1ccc(cc1)C(=O)N{alkyl}` (m05's series):
  - m15 chain2, m24 chain3, m28 chain4, m17 chain5, m22 chain6
  - m05 chain11 (TARGET, not made)
  - Only short chains 2–6 measured → must extrapolate to 11.
- **Series A — pentitol amide** `OCC(O)C(O)C(O)C(=O)N{alkyl}`, psa 110.02, arom 0:
  - m14 c2, m27 c3, m01 c4, m19 c5, m03 c6, m21 c7, m18 c8, m07 c9, m25 c10
- **Series B — nitrobenzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N{alkyl}`, psa 72.24, arom 1:
  - m09 c1, m02 c2, m12 c4, m32 c5, m33 c6, m29 c7, m31 c8, m06 c9, m10 c10
- **Series C — biphenyl benzamide** `c1ccc(cc1)-c1ccc(cc1)C(=O)N{alkyl}`, psa 29.1, arom 2:
  - m13 c2, m11 c3, m04 c4, m30 c5, m23 c6, m20 c7, m26 c8, m08 c9, m16 c10

## Job 2 candidate filtering (free constraints first)
- Constraint 2 (psa ≥ 40): keeps pentitol (110.02) and nitro (72.24) series ONLY.
  - Biphenyl (29.1) and methoxy (38.33) both fail psa ≥ 40.
- Constraint 3 (not m05's series): excludes methoxy series anyway.
- So lead must come from pentitol or nitro series.
- Pentitol: very hydrophilic (alogp<0), high HBD/HBA/PSA → expect LOW QED.
- Nitro short chains (m09 c1, m02 c2, m12 c4, m32 c5, m33 c6): drug-like MW/logP → expect QED ≥ 0.6.

## Budget plan
- Job 1: measure 5 methoxy members (m15,m24,m28,m17,m22) to fit the chain-2..6 trend.
- Job 2: measure nitro short-chain candidates; start with most drug-like (m09).

## Measurements

### Methoxy series `COc1ccc(cc1)C(=O)N{alkyl}` (m05's series, Job 1)
chain n = #carbons after N:
| n | mol | MW | alogP | rotb | QED |
|---|-----|----|-------|------|-----|
| 2 | m15 | 179 | 1.445 | 3 | 0.7621 |
| 3 | m24 | 193 | 1.835 | 4 | 0.7919 |  <- peak
| 4 | m28 | 207 | 2.225 | 5 | 0.7518 |
| 5 | m17 | 221 | 2.615 | 6 | 0.7498 |
| 6 | m22 | 235 | 3.005 | 7 | 0.7377 |
| 11 | m05 (target) | 305 | 4.956 | 12 | ? (predict) |

Key structure facts (from pool, exact):
- alogp(n) = 0.663 + 0.390*n  (perfectly linear; at n=11 → 4.953 ✓ matches m05)
- rotb(n) = n + 1  (at n=11 → 12 ✓)
- mw(n) linear, ~+14/CH2 (at n=11 → 305 ✓)
- psa=38.33, arom=1, HBD=1(NH), HBA=2(OMe+amide O) — constant.

So only MW / AlogP / RotB change with n. Peak at n=3 coincides with AlogP≈1.835 ≈
the AlogP desirability optimum (~1.8) — consistent: QED peaks when logP is optimal.

### m09 (nitro series, n=1) — first Job-2 candidate
m09: nitro n1, MW194, alogp1.345, rotb3, psa72.24, arom1. QED = 0.5826.  -> just BELOW 0.6, fails Job 2.
Need another nitro (short chain) candidate. m02 (n2, alogp1.735 ~ logP optimum) most promising.

## Pending: measure m02 (Job 2 lead), and nitro high-n (m06 n9 / m10 n10) to read
the high-logP decline rate — the d_AlogP function is universal, so nitro at logP~4-4.5
constrains the methoxy n6→n11 (logP 3.0→4.96) extrapolation.

### Results round 2
- m02 (nitro n2, alogp1.735, rotb4): QED = 0.6046  -> ≥0.6 ✓ (just). Lead candidate.
- m06 (nitro n9, alogp4.075):  QED = 0.4011
- m10 (nitro n10, alogp4.465): QED = 0.3724

### Universal logP-desirability evidence (cross-scaffold, both jobs)
QED vs AlogP for two DIFFERENT scaffolds (methoxy & nitro), aligned by logP:
| AlogP | methoxy QED | nitro QED |
|-------|-------------|-----------|
| 1.345 |             | 0.5826 (n1) |
| 1.445 | 0.7621 (n2) |             |
| 1.735 |             | 0.6046 (n2) |  <- nitro near logP optimum, peaks here
| 1.835 | 0.7919 (n3) |             |  <- methoxy near logP optimum, peaks here
| 2.225 | 0.7518 (n4) |             |
| 2.515 |             | 0.7490? (n5 m32) not measured |
| 2.615 | 0.7498 (n5) |             |
| 3.005 | 0.7377 (n6) |             |
| 4.075 |             | 0.4011 (n9) |
| 4.465 |             | 0.3724 (n10)|

Two independent scaffolds peak at AlogP≈1.7–1.8 and decline as logP climbs past ~2.5,
and the methoxy decline (n6→n10 analog) is tracked by the nitro curve at the same logP.
This empirically confirms logP-desirability dominates the QED-vs-chain trend and is
scaffold-LEVEL-invariant in *shape* (as TASK says: "shape" of curve common, "level"
differs). Methoxy sits ~0.05 above nitro at matched logP (PSA 38 vs 72 — nitro's higher
PSA / nitro group lowers absolute level, but NOT the logP-driven shape).

## Job 1 — extrapolation model
For methoxy series, parametrize QED(n) by AlogP (exact linear in n):
  AlogP = 0.663 + 0.390*n  → at n=11, AlogP = 4.953 (matches m05 4.956).

We have methoxy data at n=2..6 (AlogP 1.445..3.005) and must extrapolate to AlogP≈4.95.
The methoxy curve is already declining past n=3. The nitro series gives the shape at
high logP (AlogP 4.075→4.465 → QED 0.401→0.372): near-linear, slope ≈
(0.3724-0.4011)/(4.465-4.075) = -0.074 QED/logP-unit.

Methoxy decline n4→n6: (0.7377-0.7518)/(3.005-2.225) = -0.018/logP-unit (gentle,
near optimum). Past n=6 the curve enters the steeper high-logP regime the nitro series
samples. From methoxy n6 (AlogP3.005, QED0.7377) to target n11 (AlogP4.953):
extend with the high-logP slope observed in the nitro series (~-0.074/logP-unit),
adjusted for the ~0.05 methoxy>nitro level offset.

Linear extension of the late methoxy/nitro-consistent decline:
  QED(m05) ≈ 0.7377 + (-0.074)*(4.953-3.005) ≈ 0.7377 - 0.144 ≈ 0.594.
Floor/rounding through the MW desirability (m05 MW305 within 1-5 / 0.5; rotb 12 at dMR=0.50
edge contributes small further pull-down): realistic estimate ≈ 0.59–0.60.
The nitro n9/n10 values (0.40, 0.37 at AlogP4.07/4.46) show the high-logP regime is real
and steep; methoxy's offset keeps m05 from sinking as low as nitro, but it will be
below the 0.6 cutoff.

### PREDICTION: m05 QED ≈ 0.59 (range 0.58–0.60), declining high-logP regime.

### Round 3 — gap-fill m12
m12 (nitro n4, alogp2.125, rotb5, MW222): QED = 0.4709  !!  unexpectedly LOW.

Reconcile: this breaks the smooth "logP-shape-common" picture for nitro. Reconsider
nitro series on its OWN terms (not logP-aligned):
  n1 m09 0.5826 (alogp1.345, rotb3, MW194)
  n2 m02 0.6046 (alogp1.735, rotb4, MW208)  <- peak
  n4 m12 0.4709 (alogp2.125, rotb5, MW222)  <- sharp drop by n4
  n9 m06 0.4011 (alogp4.075, rotb10, MW292)
  n10 m10 0.3724 (alogp4.465, rotb11, MW306)
So nitro: peak at n2 (0.605), drops to 0.47 at n4, then gently 0.47→0.40→0.37 from
n4→n10. The STEEP part is n2→n4; the high-n tail is nearly flat (~-0.013/n).

Why nitro drops so hard by n4 while methoxy stays ~0.75 at n4: rotb. dMR(rotb):
rotb5 vs rotb4 is the dMR cliff edge (0.58→0.50 region). Methoxy m28 n4 (rotb5) QED0.752
survives because its logP(2.225)/MW(207) sit in desirability sweet spots so the rotb
penalty is diluted; nitro m12 n4 (rotb5, logP2.125, MW222) gets the same rotb cliff on
top of an already lower base (nitro PSA72/noisier). Either way: high-AlogP (≥4) members
of BOTH series are deep in the declining tail (nitro 0.37–0.40), and methoxy n11
(AlogP4.95) is even further out.

## Job 1 — revised extrapolation (use the FLAT high-n tail)
At high n, the tail is nearly flat: nitro n4→n10 (AlogP2.1→4.5) QED 0.471→0.372,
slope ≈ -0.029/logP-unit, i.e. ~-0.0048 per CH2. Methoxy sits ~0.05 above nitro at
matched (high) logP. Methoxy n6 (AlogP3.005) QED0.7377; extending n6→n11 (ΔlogP1.95)
with the high-logP tail slope -0.029/logP-unit:
  QED(m05) ≈ 0.7377 - 0.029*1.95 - (level/MW-penalty for MW305 near 0.50/1 region)
            ≈ 0.7377 - 0.057  ≈ 0.681, then a small further dip for rotb12 (dMR edge)
            and MW305 (within 0.5 zone) → realistic ≈ 0.64–0.66.
The nitro tail is FLATTER than I first thought (the steep n2→n4 drop is the rotb cliff,
which methoxy already crossed by n4 at 0.752 and won't re-trigger). So m05 should sit
well below the n3 peak (0.792) but stay clearly above nitro's n9/n10 (0.37–0.40) thanks
to methoxy's higher base. Estimate: QED(m05) ≈ 0.66 (range 0.63–0.68), steady decline.

## Job 2 — lead selection
m02 satisfies all three constraints: measured QED 0.6046 ≥ 0.6, psa 72.24 ≥ 40,
nitro series ≠ methoxy. Thin margin (0.0046 above 0.6). m12 (0.4709) fails, m09
(0.5826) fails, m06/m10 fail. So among measured nitro, m02 is the ONLY one ≥0.6.
Pentitol series has alogp<0 (very low logP desirability) and high rotb → expect
QED<0.5, not worth spending on. Budget left: 12-10 = 2.

Decision: keep m02 as lead (QED 0.6046). It is the eligible set's only ≥0.6 member
measured; alternatives (m09, m12, m06, m10) already measured and below. m02 is final.
