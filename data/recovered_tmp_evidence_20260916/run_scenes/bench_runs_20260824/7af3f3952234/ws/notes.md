# Notes for Molecular Design Task

## Task Overview
- Need to find QED for m05 (which cannot be measured directly)
- Need to find a molecule that meets all three criteria:
  1. QED ≥ 0.6
  2. PSA ≥ 40
  3. Not in the same congeneric series as m05

## Step 1: Explore the compound pool
First, I'll examine the molecular pool to understand the scaffolds and structures available.

I've now seen all 33 compounds. Let me group them by scaffold to identify congeneric series:

- Series 1: Compounds with nitrobenzene scaffold (O=[N+]([O-])c1ccc(cc1)C(=O)N-R): m02, m06, m09, m10, m12, m29, m31, m32, m33
- Series 2: Compounds with sugar-like scaffold (OCC(O)C(O)C(O)C(=O)N-R): m01, m03, m07, m14, m18, m19, m21, m25, m27
- Series 3: Compounds with biphenyl scaffold (c1ccc(cc1)-c1ccc(cc1)C(=O)N-R): m04, m08, m11, m13, m16, m20, m23, m26, m30
- Series 4: Compounds with methoxybenzene scaffold (COc1ccc(cc1)C(=O)N-R): m05, m15, m17, m22, m24, m28

From the group_by_scaffold tool, I can see:
- Methoxybenzene series: m15(chain_len=2), m24(chain_len=3), m28(chain_len=4), m17(chain_len=5), m22(chain_len=6), m05(chain_len=11)
- Since m05 is in the same series as m15, m24, m28, m17, m22, I cannot use any of these for Task 2.

For Task 1, I need to predict the QED of m05 based on the trend in its own congeneric series.

For Task 2, I need to find a molecule with:
1. QED ≥ 0.6
2. PSA ≥ 40
3. Not in the methoxybenzene series (not m15, m24, m28, m17, m22, m05)

Looking at the descriptors I saw earlier, the nitrobenzene series had PSA of 72.24, which meets requirement 2. The sugar-like series had PSA of 110.02, which also meets requirement 2. The biphenyl series had PSA of 29.1, which does not meet requirement 2.

So for Task 2, I should focus on the nitrobenzene series or sugar-like series.

Let me measure some compounds from these series to find one with QED ≥ 0.6.

I measured m02 (nitrobenzene series) and found its QED is 0.6046, which meets the requirement of ≥ 0.6. Its PSA is 72.24, which meets the requirement of ≥ 40. And it's not in the same series as m05, so it satisfies all three requirements for Task 2.

For Task 1, I measured several compounds from the methoxybenzene series (the same as m05):
- m15 (chain_len=2): QED = 0.7621
- m24 (chain_len=3): QED = 0.7919
- m28 (chain_len=4): QED = 0.7518
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Using these points, I fitted a polynomial trend to predict the QED for m05 (chain_len=11), which is 0.5416.