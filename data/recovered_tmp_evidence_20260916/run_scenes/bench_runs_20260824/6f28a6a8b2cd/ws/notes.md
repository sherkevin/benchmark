# Task notes

## Goal
- Task 1: Report QED of m05 (COc1ccc(cc1)C(=O)NCCCCCCCCCCC), never synthesized -> predict from its own congeneric series. m05 chain_len = 11.
- Task 2: Pick one measured molecule with QED >= 0.6, psa >= 40, NOT same congeneric series as m05.
- Budget: 17 units total, shared between list_pool (1/page) and measure_qed (1/measurement).

## Scaffold grouping (free via group_by_scaffold)
4 scaffolds. m05 is in scaffold 1 (methoxybenzamide COc1ccc(cc1)C(=O)N).
- Scaffold 1 (methoxy, m05's): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)
- Scaffold 2 (nitro O=[N+]([O-])c1ccc(cc1)C(=O)N): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- Scaffold 3 (sugar OCC(O)C(O)C(O)C(=O)N): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- Scaffold 4 (biphenyl c1ccc(cc1)-c1ccc(cc1)C(=O)N): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Key reasoning
- N-alkyl chain is pure hydrocarbon (-CH2-), adds 0 PSA => PSA constant within a scaffold.
  Page-1 list confirms: nitro m02 psa=72.24; sugar m01/m03 psa=110.02; biphenyl m04 psa=29.1.
- Task-2 PSA filter: biphenyl 29.1 <40 EXCLUDED; methoxy = m05's own series EXCLUDED.
  Remaining candidate scaffolds: nitro (72.24), sugar (110.02).
- Per-scaffold caps (free component breakdowns):
  * nitro m09: ALERTS=0.2414 (nitroaromatic structural alert) => permanent cap; series best at chain 2 (0.5826), then DECREASES (c4=0.4709, c5=0.4683, c6=0.4591). BUT chain 3 is the benzamide sweet-spot (methoxy peaked at m24 c3=0.7919). m02 (nitro c3) = 0.6046 => the ONE passing member.
  * sugar m14: HBD=0.0555 (4 OH donors, fixed), ALOGP=0.0494 (logP -2.0), AROM=0 => permanent cap; c2=0.3237, c5=0.3244 (flat, dead).

## Measurements (budget used 13/17)
m05 series (Task 1): m15=0.7621, m24=0.7919, m28=0.7518, m17=0.7498, m22=0.7377
list page 1 = 1 unit
Task-2 nitro: m09=0.5826(fail), m12=0.4709(fail), m32=0.4683(fail), m33=0.4591(fail), m02=0.6046 (PASS)
Task-2 sugar: m14=0.3237(fail), m19=0.3244(fail)

## Task 1 trend (fit_trend, free) for m05 at chain 11
Points (chain, qed): (2,0.7621),(3,0.7919),(4,0.7518),(5,0.7498),(6,0.7377)
- degree 2: 0.5416, coeffs [0.74932,0.017024,-0.003264], rmse 0.01188 (lower RMSE; captures curvature, peak at c3)
- degree 1: 0.695, rmse 0.01308
=> target_qed = 0.5416 (quadratic, lower RMSE, respects non-monotonic peak).

## Task 2 answer
lead_mol_id = m02 (nitro scaffold, chain 3). Measured QED = 0.6046 (>=0.6).
Constraints: (1) QED 0.6046>=0.6 YES; (2) psa 72.24>=40 YES; (3) nitro scaffold != m05's methoxy scaffold => different series YES. All three satisfied.

## ANSWER
target_qed = 0.5416
lead_mol_id = "m02"
lead_qed_measured = 0.6046
