# Notes — QED prediction task

## Pool structure (from group_by_scaffold, free)
4 congeneric series (N-alkyl chain length varies within each), 33 molecules total:

1. **methoxy-benzamide** `COc1ccc(cc1)C(=O)N` — **m05's series (TARGET)**
   - m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)** ← target, not synthesizable
2. **nitro-benzamide** `O=[N+]([O-])c1ccc(cc1)C(=O)N`
   - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), **m10(10)**
3. sugar-amide `OCC(O)C(O)C(O)C(=O)N` — m14(2)…m25(10)
4. biphenyl-benzamide `c1ccc(cc1)-c1ccc(cc1)C(=O)N` — m13(2)…m16(10)

Budget = 12 units, shared by list_pool + measure_qed.

## Strategy
- **Task 1**: m05 (chain 11) not measurable. Measure its series (chains 2-6), then extrapolate.
  To get a real long-chain anchor (the extrapolation of overall QED alone is unstable),
  also measure m10 from the nitro series at chain 10 (rotb=11, one from m05's rotb=12) —
  the ROTB desirability score depends only on rotatable-bond count, scaffold-independent,
  so it transfers directly.
- **Task 2**: need QED≥0.6 AND psa≥40 AND not m05's series.
  - methoxy series excluded (same series as m05).
  - biphenyl series: only an amide → psa 29.1 (<40). Excluded.
  - sugar series: 4 OH + amide → psa 110, but very polar (alogp<0), high HBD/HBA → QED likely low.
  - nitro series: nitro(2 O) + amide → psa 72.24 (≥40 ✓), moderate MW/alogp, arom=1 → QED likely ≥0.6
    for short chains. Best bet.

## Measurements (budget = 12 total)
| mol | series | chain | rotb | QED | components (key) |
|---|---|---|---|---|---|
| m15 | methoxy | 2 | 4 | 0.7621 | MW0.33 ROTB0.99 ALOGP0.87 ALERTS0.84 |
| m24 | methoxy | 3 | 5 | 0.7919 | MW0.41 ROTB0.97 ALOGP0.94 ALERTS0.84 |
| m28 | methoxy | 4 | 5*| 0.7518 | MW0.49 ROTB0.85 ALOGP0.98 ALERTS0.64 |
| m17 | methoxy | 5 | 6 | 0.7498 | MW0.59 ROTB0.69 ALOGP1.00 ALERTS0.64 |
| m22 | methoxy | 6 | 7 | 0.7377 | MW0.68 ROTB0.53 ALOGP1.00 ALERTS0.64 |
| m02 | nitro   | 3 | 4 | 0.6046 | (psa 72.24 ≥40, mw 208, alogp 1.735, arom 1) |
| m10 | nitro   | 10| 11| 0.3724 | MW1.00 ROTB0.151 ALOGP0.704 ALERTS0.082 HBA0.985 |

(*m28 rotb reported as 5 by catalogue but component curve consistent with chain 4.)

list_pool pages used: 1, 6, 7, 8 (cost 4). Total used: 5 meas + 4 pages = 9? tool counter says 11.
(any discrepancy is the shared counter; final budget_used=11 reported by instrument.)

## Task 1 — prediction of m05 QED

### m05 descriptors (chain 11, from regular methoxy increments: +14.02 mw, +0.39 alogp, +1 rotb per CH2)
- MW = 235.33 + 5*14.02 = **305.43** (peak MW zone → MW score ~1.0, like m10's 1.0)
- ALOGP = 3.005 + 5*0.39 = **4.95** (== m10's alogp 4.95 → ALOGP score 0.704)
- PSA = 38.33 → PSA score 0.933
- ROTB = 7 + 5 = **12**
- HBA = 3 (methoxy O + amide O + N) → 0.931
- HBD = 1 → 0.987 ; AROM = 1 → 0.827 ; ALERTS ≈ 0.08 (chain 11, near floor)

### Why overall-QED extrapolation alone is unreliable
Polynomial fits of the methoxy overall-QED vs chain (2-6) are unstable beyond the data:
- deg-2 poly on (2-6) → x=11: 0.5416 (accelerating down)
- deg-1 line on (2-6)  → x=11: 0.6950 (gentle slope)
- deg-1 line on (3-6)  → x=11: 0.6508
The two functional forms diverge because the curvature comes from ROTB/ALERTS decay that
*further accelerates* once past the measured range. A straight line drawn through chains 3-6
understates how hard ROTB/ALERTS collapse at very long chains; the deg-2 poly gets the
direction right but, fed only short-chain points, can't place the floor.

### The long-chain anchor — m10 (nitro, chain 10, rotb=11)
m10 is one rotatable bond from m05 (rotb 11 vs 12) and shares the amide long-chain character.
Its measured QED = 0.3724 with components:
  MW 1.00, ALOGP 0.704, HBA 0.985, HBD 0.987, PSA 0.930, ROTB 0.151, AROM 0.827, ALERTS 0.082
m05 has *identical* MW(~305), identical ALOGP(4.95), identical AROM, and PSA/HBD that map to the
same scores. m05 differs only in:
  - ROTB: 12 vs 11 → slightly below 0.151, ~0.13 (steep tail, near asymptote ~0.1)
  - HBA: methoxy (3 acceptors) vs nitro (4) → m05 HBA 0.931 < m10 0.985 (m05 worse by ~0.05)
  - ALERTS: 11 vs 10 → both already at the ~0.08 floor
So m05's QED sits just below m10's 0.3724 — one more rotb penalty + lower HBA, against an
otherwise identical (MW, ALOGP, AROM, PSA, HBD) profile.

### Final prediction for m05
**target_qed ≈ 0.35** (slightly below m10's 0.3724; ROTB 0.151→~0.13, HBA 0.985→0.931).
The quadratic extrapolation (0.54) is too high — it cannot resolve the ROTB/ALERTS collapse
past chain 6. The linear extrapolation (0.65-0.70) is far too high — it ignores the acceleration.
m10's real long-chain measurement is the most trustworthy anchor.

## Task 2 — lead pick
**m02** (nitro series, chain 3):
- measured QED = **0.6046** ≥ 0.6 ✓ (real measure_qed call)
- psa = 72.24 ≥ 40 ✓ (catalogue descriptor)
- nitro-benzamide scaffold ≠ methoxy-benzamide (m05's) ✓ — different congeneric series
All three constraints satisfied. Picked.
