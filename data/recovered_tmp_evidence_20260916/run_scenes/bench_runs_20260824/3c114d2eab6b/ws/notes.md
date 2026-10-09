# Notes

## Budget
- Total 12 units, shared by list_pool + measure_qed.
- Plan: Task1 trend = 5 measurements (m05's congeneric series). Task2 = list 1 page + measure candidate(s).

## Scaffolds (from group_by_scaffold, free)
- S1 `COc1ccc(cc1)C(=O)N` (methoxybenzamide): m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11, target, unsynth)**
- S2 `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitrobenzamide): m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
- S3 `OCC(O)C(O)C(O)C(=O)N` (polyhydroxy sugar amide): m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
- S4 `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl amide): m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

## PSA reasoning (alkyl chain adds no polar atoms -> PSA constant within a scaffold)
- S1: amide(~29) + methoxy ether(~9) ~ 38  (<40)  -> m05's series, excluded for Task2 anyway
- S2: nitro(~46) + amide(~29) ~ 75 (>=40)  -> Task2 candidates
- S3: 4xOH(~81) + amide(~29) ~ 110 (>=40) -> Task2 candidates
- S4: amide only ~29 (<40) -> excluded for Task2

## Task 1 plan
Measure m15, m24, m28, m17, m22 (chain 2,3,4,5,6), fit trend (try deg 1 and 2), evaluate at chain_len=11.

## Task 2 plan
Pick from S2 or S3 (psa>=40), measure QED, need >=0.6. Short chains likely higher QED.
