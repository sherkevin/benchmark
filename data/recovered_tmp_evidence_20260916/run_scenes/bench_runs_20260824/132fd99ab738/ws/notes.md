# Task notes

## Task overview
- Target m05: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (methoxy-benzamide, N-alkyl chain length 11). Never synthesized → probe_unavailable. Must predict QED from its own congeneric series.
- Task 2: pick ONE molecule with (a) measured QED ≥ 0.6, (b) psa ≥ 40, (c) NOT in m05's congeneric series.
- Budget: 12 measurements total, shared.

## Congeneric series of m05 (methoxy-benzamide scaffold `COc1ccc(cc1)C(=O)N` + alkyl chain)
Scaffold = 4-methoxy-benzamide, varying only N-alkyl chain length:
- m15: chain 2  (`NCC`)         mw179.22 alogp1.445  psa38.33 rotb3 arom1
- m24: chain 3  (`NCCC`)         mw193.25 alogp1.835  psa38.33 rotb4 arom1
- m28: chain 4  (`NCCCC`)        mw207.27 alogp2.225  psa38.33 rotb5 arom1
- m17: chain 5  (`NCCCCC`)       mw221.30 alogp2.615  psa38.33 rotb6 arom1
- m22: chain 6  (`NCCCCCC`)     mw235.33 alogp3.005  psa38.33 rotb7 arom1
- m05: chain 11 (`NCCCCCCCCCCC`) mw305.46 alogp4.956  psa38.33 rotb12 arom1  ← TARGET (unsynthesized)

These all share scaffold and differ ONLY in N-alkyl chain length → same congeneric series.
Strategy for Task 1: measure m15,m24,m28,m17,m22 → trend vs chain length → extrapolate to chain 11.

## Task 2 candidate filtering
Constraint 2 (psa ≥ 40): eliminates all biphenyl-benzamides (psa=29.1) and m05's own series (psa=38.33).
Constraint 3 (not m05's series): eliminates m15,m24,m28,m17,m22,m05 anyway.

Remaining candidates (psa≥40, not m05 series), grouped by their own series:
- Nitro-benzamide (`O=[N+]...c1ccc(cc1)C(=O)N` + chain), psa=72.24, arom=1:
  - m09 chain2  mw194.19 alogp1.345 rotb3   ← most drug-like
  - m02 chain3  mw208.22 alogp1.735 rotb4
  - m12 chain4  mw222.24 alogp2.125 rotb5
  - m32 chain5  mw236.27 alogp2.515 rotb6
  - m33 chain6  mw250.30 alogp2.905 rotb7
  - m29 chain7  mw264.32 alogp3.295 rotb8
  - m31 chain8  mw278.35 alogp3.685 rotb9
  - m06 chain9  mw292.38 alogp4.075 rotb10
  - m10 chain10 mw306.41 alogp4.465 rotb11
- Carbohydrate (`OCC(O)C(O)C(O)C(=O)N` + chain), psa=110.02, arom=0, alogp very negative:
  - m14,m27,m01,m19,m03,m21,m18,m07,m25  → high PSA + negative alogp → likely LOW QED

Most promising for QED≥0.6: short-chain nitro-benzamides (m09, m02, m12, m32).

## Measurement budget plan (12 total)
- Task 1: m15, m24, m28, m17, m22  (5)
- Task 2: start with m09 (most drug-like), then m02/m12/m32 as fallbacks (≤7)

## Measurements so far
Task 1 — m05 congeneric series (4-methoxy-benzamide, chain n → QED):
- m15 n=2  alogp1.445  QED 0.7621
- m24 n=3  alogp1.835  QED 0.7919  (peak — alogp nearest optimal ~2.5 from below)
- m28 n=4  alogp2.225  QED 0.7518
- m17 n=5  alogp2.615  QED 0.7498
- m22 n=6  alogp3.005  QED 0.7377
m05 n=11 alogp4.956 rotb12 mw305.46  (unsynthesized → predict)
Decline n3→n6: 0.7919→0.7377 = -0.0542 over 3 steps (~-0.018/step).
Linear fit (n=3..6): slope -0.0165/carbon → n=11 ≈ 0.651.
Linear fit (all n=2..6): slope -0.0091/carbon → n=11 ≈ 0.695.
Linear-in-alogp from peak: 0.647.
BUT alogp weight drops sharply past ~3 (m05 alogp≈5, weight very low) + rotb=12 (low weight)
⇒ true value likely a bit BELOW linear. Estimate m05 QED ≈ 0.60-0.63. Report ~0.62.

Task 2 — m09 (nitro chain2, alogp1.345, psa72.24): QED 0.5826 → below 0.6, fails.
m09 is below-optimal alogp; pre-penalty weight model vs measured implies ~alert factor
(constant across nitro series). Nitro candidates with alogp near 2.5 should score higher.
Top candidates (not m05 series, psa=72.24≥40): m32(c5,alogp2.515), m12(c4,alogp2.125),
m02(c3,alogp1.735). Predicted ~0.61-0.62 (marginal). Measure to find which clears 0.6.
