# Task notes

Budget: 17 total, shared by list_pool + measure_qed. Pool paging cost 10 units.
**Total measurements used = 7 (all 17/17 exhausted).**

## Pool (33 molecules) grouped by scaffold
Chain length n = #carbons after the amide N.

### Series A — methoxy-benzamide `COc1ccc(cc1)C(=O)N(C)n` (psa 38.33, arom 1) — TARGET m05's series
- m15  C2 (179,  alogp1.445, rotb3)
- m24  C3 (193,  alogp1.835, rotb4)
- m28  C4 (207,  alogp2.225, rotb5)
- m17  C5 (221,  alogp2.615, rotb6)
- m22  C6 (235,  alogp3.005, rotb7)
- m05  C11(305,  alogp4.956, rotb12) — TARGET, unsynthesized → extrapolate beyond C6

### Series B — nitro-benzamide `O=[N+]([O-])c1ccc(cc1)C(=O)N(C)n` (psa 72.24, arom 1)
m09 C2, m02 C3, m12 C4, m32 C5, m33 C6, m29 C7, m31 C8, m06 C9, m10 C10

### Series C — sugar-amide `OCC(O)C(O)C(O)C(=O)N(C)n` (psa 110.02, arom 0)
m14 C2, m27 C3, m01 C4, m19 C5, m03 C6, m21 C7, m18 C8, m07 C9, m25 C10
(alogp range −2.8..+0.3, all far below optimum → expect QED < 0.6)

### Series D — biphenyl-benzamide `c1ccc(cc1)-c1ccc(cc1)C(=O)N(C)n` (psa 29.1, arom 2)
m13 C2, m11 C3, m04 C4, m30 C5, m23 C6, m20 C7, m26 C8, m08 C9, m16 C10

## Job 2 eligibility (constraints 2 & 3, free)
- psa≥40: drops methoxy(38.33) & biphenyl(29.1). Keeps nitro(72.24) & sugar(110.02).
- not-methoxy-series: drops methoxy (already dropped).
→ eligible pool = nitro B + sugar C.

## MEASURED — Series A (m05's series), QED vs chain length
| n | mol | QED    |
|---|-----|--------|
| 2 | m15 | 0.7621 |
| 3 | m24 | 0.7919 |  ← peak
| 4 | m28 | 0.7518 |
| 5 | m17 | 0.7498 |
| 6 | m22 | 0.7377 |

Trend: rises C2→C3, peak at C3, then declines to C6. Not monotonic-linear; the
peak + gentle decline is why a straight line / nearest-point quote is wrong.

## Job 1 — extrapolation to m05 (n=11, 5 steps past measured C6)

Two independent estimates:

### (1) Quadratic fit to the 5 observed points
Fit QED(n)=A+B·x+C·x² with x=n−4 (x=−2..2). Least squares gives:
A=0.7652, B=−0.00909, C=−0.003264  (downward-opening; accelerates).
At n=11 → x=7:  QED = 0.7652 −0.00909·7 −0.003264·49 = 0.7652−0.0636−0.1599 = **0.542**.
The negative C means decline accelerates past the data — physically right (see (2)).

### (2) Component reasoning (why it accelerates)
QED is a weighted geometric mean of 8 property desirabilities; psa(38.33) & arom(1)
are CONSTANT across the series (psa=38 is sub-optimal, capping the whole series
~0.74–0.79). The chain trend is driven by alogp, rotb, MW:
- alogp crosses its optimum (~3.7) at n≈5.8; at n=11 alogp=4.956 → alogp penalty active.
- rotb crosses its soft threshold (~7) at n=6; at n=11 rotb=12 → rotb penalty active.
- MW 305 at n=11 is mildly past optimum.
So decline must steepen beyond C6 (accelerate), exactly as the quadratic's negative C says.
Component-weighted estimate (alogp d≈0.4–0.5, rotb d≈0.3–0.4, MW d≈0.85 vs ~0.9/0.9/0.95 at n=6;
weights alogp0.28 rotb0.18 MW0.17): QED ≈ 0.7377×(0.45^0.28)×(0.4^0.18)×(0.9^0.17) ≈ **0.50**.

Both estimates land ~0.50–0.54. Quadratic (fit to observed trend) is the headline value 0.54;
component model (mechanism, accounts for alogp=4.956 & rotb=12 deep in penalty) pulls to ~0.50.
Reported prediction: **0.50** (central value; quadratic 0.54 is the explicit-trend upper bound).

## Job 2 — lead selection (all 17 budget spent)
Strategy: use the methoxy peak (n=3, alogp1.84, rotb4) as the "proven-good" region and
find the eligible (nitro/sugar) molecule closest to it.
- m12 (nitro, n=4, alogp2.125, rotb5): measured 0.4709 — nitro scaffold ~0.28 below
  methoxy at same chain (m28=0.7518). Nitro's extra PSA/HBA heavily penalized here.
- With 1 measurement left, go to the nitro member at the proven peak chain n=3:
  m02 (nitro, n=3, alogp1.735, rotb4, MW208, psa72.24) — closest analog to the methoxy peak.
- m02 measured = **0.6046** ≥ 0.6 ✓. psa 72.24≥40 ✓. nitro ≠ methoxy ✓.

All three constraints satisfied. **Lead = m02.**

## Final answer
- target_qed (m05, n=11): 0.50
- lead_mol_id: m02
- lead_qed_measured: 0.6046
- congener_ids (methoxy-benzamide pool members, chain differs from m05): m15, m24, m28, m17, m22
