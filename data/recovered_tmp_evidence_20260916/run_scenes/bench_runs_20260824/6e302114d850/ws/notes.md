# Notes

## Task overview
- Pool: 33 molecules, 4 scaffolds (from `group_by_scaffold`, free).
- Budget: 12 total (list_pool pages + measure_qed calls share it).
- m05 is in scaffold 1 (methoxy-benzamide `COc1ccc(cc1)C(=O)N`), chain_len 11, never synthesized.

## Scaffolds (from group_by_scaffold)
- S1 `COc1ccc(cc1)C(=O)N` (methoxy-benzamide): m15(2), m24(3), m28(4), m17(5), m22(6), m05(11)  <- Task 1 series
- S2 `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro-benzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- S3 `OCC(O)C(O)C(O)C(=O)N` (sugar-alcohol amide): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- S4 `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl amide): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## Descriptors from list_pool page 1 (cost 1)
- m01 (S3, chain4): mw 221.25, alogp -2.022, psa 110.02, rotb 7, arom 0
- m02 (S2, chain3): mw 208.22, alogp 1.735, psa 72.24, rotb 4, arom 1
- m03 (S3, chain6): mw 249.31, alogp -1.242, psa 110.02, rotb 9, arom 0
- m04 (S4, chain4): mw 253.34, alogp 3.884, psa 29.1, rotb 5, arom 2

## Key insight: PSA is constant within a scaffold
The N-alkyl chain adds only carbons (nonpolar), so PSA depends only on the scaffold:
- S1 (methoxy): excluded from Task 2
- S2 (nitro): psa 72.24  >= 40  -> OK for Task 2
- S3 (sugar): psa 110.02 >= 40  -> OK for Task 2
- S4 (biphenyl): psa 29.1 < 40   -> FAILS Task 2

So no more list_pool pages needed; I have all psa info I need.
Task 2 candidates: scaffolds S2 or S3.

## Reasoning about which scaffold passes QED >= 0.6
- S3 (sugar): very negative alogp (m01 -2.022), psa 110, many HBD -> QED likely low.
- S2 (nitro): m02 mw 208, alogp 1.735, psa 72, rotb 4, arom 1 -> drug-like; main risk is whether nitro is a QED alert.
- Start Task 2 by measuring m02 (descriptors known, short chain = most drug-like).

## Plan
- Task 1: measure m15, m24, m28, m17, m22 (chain 2,3,4,5,6 of m05's scaffold). Fit trend, evaluate at chain 11.
- Task 2: measure m02. If QED>=0.6 and psa>=40 and not S1 -> done. Else try other S2 short-chain (m09, m12) or S3 mid-chain.

## Measurements (budget 12)
1. list_pool p1 -> 1 used (m01,m02,m03,m04 descriptors)
2. m15 (S1,c2) = 0.7621
3. m24 (S1,c3) = 0.7919
4. m28 (S1,c4) = 0.7518
5. m02 (S2,c3) = 0.6046  <- Task 2 lead candidate
6. m17 (S1,c5) = 0.7498
7. m22 (S1,c6) = 0.7377
Total used: 7. Remaining: 5.

## Task 1: extrapolation to m05 (S1, chain 11)
Data (chain -> QED): {2:0.7621, 3:0.7919, 4:0.7518, 5:0.7498, 6:0.7377}
Non-monotonic (peak ~c3), then gentle decline c3->c6 of ~0.018/chain-unit.
fit_trend predictions at x=11:
  degree 2, all 5 pts: 0.5416  (RMSE 0.0119, coeffs 0.7493 +0.0170 x -0.00326 x^2)  <- lowest RMSE, best fit
  degree 1, all 5 pts: 0.6950  (RMSE 0.0131)
  degree 1, pts c3-c6: 0.6508 (RMSE 0.0088 on the declining tail)
Decision: report target_qed = 0.5416 (quadratic, best-supported by 5 points, captures
the curvature and is consistent with the physical expectation that a C11 chain pushes
MW/logP past QED optima and the drug-like range).

## Task 2: lead
m02: S2 (nitro-benzamide), NOT m05's series (m05 is S1).
- measured QED = 0.6046 >= 0.6  PASS
- psa = 72.24 >= 40  PASS
- different scaffold from m05  PASS
All three constraints satisfied. lead_mol_id = m02, lead_qed_measured = 0.6046.

## Hedging: searched for a safer lead (m02 is a marginal pass at 0.6046)
8. m09 (S2,c2) = 0.5826  (< 0.6, fails)
9. m12 (S2,c4) = 0.4709  (< 0.6, fails; S2 QED peaks at c3=m02)
10. m03 (S3,c6) = 0.3282  (very low; S3's high psa=110/HBD count tanks QED)
Total used: 10. Remaining: 2.
S2 QED series: c2=0.5826, c3=0.6046(peak), c4=0.4709 -> peak is m02.
S3 is non-viable (0.3282). S4 fails psa (29.1<40). S1 is excluded (m05's series).
=> m02 is the best achievable lead; 0.6046 is a genuine pass of all three constraints.

## Final answers
- target_qed (m05) = 0.5416  (quadratic fit through 5 same-series points, RMSE 0.0119)
- lead_mol_id = m02
- lead_qed_measured = 0.6046
