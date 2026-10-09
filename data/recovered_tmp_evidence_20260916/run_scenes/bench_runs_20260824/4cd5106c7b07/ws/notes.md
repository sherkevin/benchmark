# Notes

## Task understanding

- Task 1: Report QED of `m05` (`COc1ccc(cc1)C(=O)NCCCCCCCCCCC`, n=11). Never synthesized →
  `measure_qed` returns `probe_unavailable`. Predict QED from the trend of its OWN
  congeneric series (same scaffold, only N-alkyl chain length differs).
- Task 2: Pick ONE molecule satisfying all three:
  1. measured QED ≥ 0.6
  2. psa ≥ 40
  3. NOT in m05's congeneric series (same scaffold, differ only in N-alkyl chain length).
- Budget: 12 total `measure_qed` calls, shared across both tasks.

## Congeneric series of m05

m05 scaffold = `COc1ccc(cc1)C(=O)N(C)(alkyl)` (methoxy-benzamide, N-alkyl chain).
Pool members sharing this scaffold (differ only in N-alkyl chain length n):

- m15  n=2   `COc1ccc(cc1)C(=O)NCC`           psa 38.33
- m24  n=3   `COc1ccc(cc1)C(=O)NCCC`          psa 38.33
- m28  n=4   `COc1ccc(cc1)C(=O)NCCCC`         psa 38.33
- m17  n=5   `COc1ccc(cc1)C(=O)NCCCCC`        psa 38.33
- m22  n=6   `COc1ccc(cc1)C(=O)NCCCCCC`       psa 38.33
- m05  n=11  `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`  psa 38.33  (TARGET, unsynthesized)

So m05's series = {m15, m24, m28, m17, m22, m05}. The synthesizable members
(m15, m24, m28, m17, m22) give the chain-length vs QED trend to extrapolate to n=11.

## Task 1 — measured series (QED vs n)

| mol_id | n  | QED    |
|--------|----|--------|
| m15    | 2  | 0.7621 |
| m24    | 3  | 0.7919 |
| m28    | 4  | 0.7518 |
| m17    | 5  | 0.7498 |
| m22    | 6  | 0.7377 |

Trend: peak at n=3 (0.7919), gently declining thereafter. Linear fit over the
declining region n=3..6 gives slope ≈ −0.0165/n, intercept ≈ 0.832 → at n=11 ≈ 0.65.
Accounting for accelerating QED penalties at longer chain (m05: rotb=12 high,
alogp=4.956 elevated, mw=305.46) which depress QED below the linear trend,
estimate slightly below linear → **target_qed ≈ 0.62**.

## Task 2 — candidate pool

Constraint 2 (psa ≥ 40) excludes the methoxy series (psa 38.33 = m05's series — also
excluded by constraint 3) and the biphenyl series (psa 29.1 < 40). Remaining
candidates are the nitro-benzamide series (psa 72.24) and the polyol series (psa 110.02).

Measured:

| mol_id | series      | n  | QED    | psa     | ≥0.6? |
|--------|-------------|----|--------|---------|-------|
| m02    | nitro       | 3  | 0.6046 | 72.24   | ✓     |
| m09    | nitro       | 2  | 0.5826 | 72.24   | ✗     |
| m12    | nitro       | 4  | 0.4709 | 72.24   | ✗     |
| m32    | nitro       | 5  | 0.4683 | 72.24   | ✗     |
| m29    | nitro       | 7  | 0.4446 | 72.24   | ✗     |
| m14    | polyol      | 2  | 0.3237 | 110.02  | ✗     |
| m27    | polyol      | 3  | 0.3330 | 110.02  | ✗     |

**m02** satisfies all three constraints:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. nitro-benzamide series (`O=[N+]([O-])c1ccc(cc1)C(=O)N(C)(alkyl)`) ≠ m05's
   methoxy-benzamide series ✓

→ lead_mol_id = "m02", lead_qed_measured = 0.6046.

## Budget usage

12/12 used: m15, m24, m28, m17, m22 (Task 1 series, 5); m09, m02, m12, m32, m29
(nitro candidates, 5); m14, m27 (polyol candidates, 2). Budget exhausted.
