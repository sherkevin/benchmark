# Notes — predicting m05 QED and picking a lead

## Budget
- Total 17, shared listing+measuring. Used 9 paging the 33-molecule catalog (4/page).
- **8 measurements left** for both jobs.

## Catalog — series identified by scaffold (chain = #carbons after N)

Four congeneric series (benzamide + N-alkyl chain), all single-ring/biaryl/sugar amides:

### Series A — p-METHOXYBENZAMIDE  `COc1ccc(cc1)C(=O)N{chain}`  ← TARGET's series, psa=38.33
- m05  : 11C  — TARGET (never made, unmeasurable)
- m15  : 2C   (mw179, logP1.45, rotb3)
- m24  : 3C   (mw193, logP1.84, rotb4)
- m28  : 4C   (mw207, logP2.23, rotb5)
- m17  : 5C   (mw221, logP2.62, rotb6)
- m22  : 6C   (mw235, logP3.01, rotb7)
=> congener_ids (other members of m05's series) = m15, m17, m22, m24, m28.
   Pool only holds SHORT chains (2–6C); m05=11C is extrapolation beyond the range.

### Series B — p-NITROBENZAMIDE  `O=[N+]([O-])c1ccc(cc1)C(=O)N{chain}`  psa=72.24 (≥40 ✓)
2C m09; 3C m02; 4C m12; 5C m32; 6C m33; 7C m29; 8C m31; 9C m06; 10C m10.
Mid-chain (m32 5C / m33 6C / m29 7C / m31 8C) are the most drug-like eligible candidates.

### Series C — BIPHENYL benzamide  `c1ccc(cc1)-c1ccc(cc1)C(=O)N{chain}`  psa=29.1 (<40 ✗)
All fail constraint 2 (psa<40). Excluded.

### Series D — sugar amide  `OCC(O)C(O)C(O)C(=O)N{chain}`  psa=110.02 (≥40 ✓)
High HBD (4 OH+NH=5), high HBA, very negative logP (≤0.3 even at 10C), high rotb.
Unlikely to reach QED≥0.6 (logP/PSA/HBD all penalised). Not preferred.

## Lead eligibility (Job 2): constraints 2 & 3 checked free, first
- psa≥40 → only Series B (72) and Series D (110) qualify. Series A (38) and C (29) fail.
- "not in m05's series" → Series A excluded (also already fails psa).
- So lead must come from Series B or D. Series B (nitro) much more drug-like → focus there.
- Must MEASURE to confirm QED≥0.6 (constraint 1).

## Round 1 measurements (budget 9→14, 3 left)
Job-1 trend (Series A, p-methoxybenzamide, psa=38.33):
- m15 2C = 0.7621
- m24 3C = 0.7919   ← peak
- m17 5C = 0.7498
- m22 6C = 0.7377
Shape: rises 2→3C (+0.030), then DECLINES 3→5→6C. Decline rates: 3→5 = (0.792-0.750)/2
= 0.021/C; 5→6 = (0.750-0.738)/1 = 0.012/C. → decline is DECELERATING (flattening),
not accelerating. m05=11C is 5 carbons past the last measured point → extrapolate the
tail. Need a model that decelerates (approach-to-floor), NOT a symmetric parabola
(which accelerates and would over-predict the drop).

Job-2 first shot: m33 (nitro 6C) = 0.4591 → FAILS (<0.6). All visible descriptors fine
(MW250/ALogP2.91/RotB7/PSA72/Arom1), so the low QED must come from HIDDEN props
(HBA raised by –NO₂, and/or a structural alert). That penalty is constant across the
nitro series → only the chain-peak (short-chain) members can possibly clear 0.6.
Methoxy peaked at 3C, so nitro's likely peak is also ~3C → m02 (3C nitro) is the best bet;
m09 (2C nitro) next.

## Round 2 result
m02 (3C nitro) = 0.6046 → clears ≥0.6. **LEAD = m02.**
Eligibility: nitro scaffold (≠ m05 methoxy series ✓), psa=72.24 ≥40 ✓, measured QED 0.6046 ≥0.6 ✓.
m28 (4C methoxy) = 0.7518 (fills Job-1 curve, straddling peak).
Budget 16/17; last unit saved (no remaining Job-1 point is measurable: all 5 pool
methoxy members done, m05 itself is probe_unavailable).

## Job 1 — full Series A trend (p-methoxybenzamide), chain n = #carbons after N
 n=2 m15 0.7621 ; n=3 m24 0.7919 (PEAK) ; n=4 m28 0.7518 ; n=5 m17 0.7498 ; n=6 m22 0.7377
Rises 2→3 (+0.030), drops 3→4 (-0.040), plateaus 4→5 (-0.002), drops 5→6 (-0.012).
Net 3→6 = -0.0542 over 3C. Decline is noisy but trending down; m05 n=11 is 5C past 6C.

### Extrapolation models to n=11 (5 carbons beyond last point)
(a) Linear fit on the declining region n=3..6 (slope -0.0165/C):  → 0.651
(b) Linear on post-peak plateau n=4..6 (slope -0.0071/C):          → 0.704
(c) Exponential approach to floor L:  Q(n)=L+(0.7919-L)·e^{-k(n-3)}.
    L=0.70,k=0.30 fits n=5 (0.7504 vs 0.7498) & n=6 (0.7374 vs 0.7377) almost exactly → 0.708
(d) Structural check via QED's 8 property desirabilities: from n=6→11 the two moving
    levers are ALogP 3.0→4.96 (high-logP penalty) and RotB 7→12 (crosses the rotb>10
    penalty). In the geometric mean, d_logp ~0.95→0.65 and d_rotb ~0.95→0.6 give
    ~0.7377·(0.68·0.63)^(1/8) ≈ 0.64. PSA(38.33),MW(305),Arom(1),HBD(1),HBA(2) constant.

(a)=0.651 and (d)=0.64 agree closely; (c)=0.708 ignores the penalty-zone entry (rotb>10,
logP~5) that kicks in between 6C and 11C and is therefore over-optimistic; (b) ignores the
peak drop. The data itself is stepped (QED is computed on binned properties → plateaus).

**Prediction for m05 (n=11) ≈ 0.65.** Captures the observed decline, stays below the
over-optimistic floor model because the rotb/logP penalty zones are genuinely entered
at 11C, and matches both the linear fit and the structural desirability estimate.

## Final deliverables
- target_qed (m05) = 0.65
- lead = m02, measured QED = 0.6046
- congener_ids (other pool members of m05's methoxy series) = m15, m24, m28, m17, m22
