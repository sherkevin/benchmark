# Notes

## Approach

### Step 1: Scaffold grouping (free)
Used group_by_scaffold() to identify 4 scaffold groups:
1. Scaffold A: COc1ccc(cc1)C(=O)N - m15(2), m24(3), m28(4), m17(5), m22(6), **m05(11)**
2. Scaffold B: O=[N+]([O-])c1ccc(cc1)C(=O)N - m09(2), m02(3), m12(4), m32(5), m33(6), m29(7), m31(8), m06(9), m10(10)
3. Scaffold C: OCC(O)C(O)C(O)C(=O)N - m14(2), m27(3), m01(4), m19(5), m03(6), m21(7), m18(8), m07(9), m25(10)
4. Scaffold D: c1ccc(cc1)-c1ccc(cc1)C(=O)N - m13(2), m11(3), m04(4), m30(5), m23(6), m20(7), m26(8), m08(9), m16(10)

m05 is in scaffold A with chain length 11.

### Step 2: Read catalogue (3 pages, budget cost 3)
PSA by scaffold:
- Scaffold A: psa ~38 (below 40 threshold - fails Task 2 psa constraint)
- Scaffold B: psa = 72.24 (passes)
- Scaffold C: psa = 110.02 (passes)
- Scaffold D: psa = 29.1 (fails)

### Step 3: Measure QED for m05's congeneric series (5 measurements)
| mol_id | chain_len | QED     |
|--------|-----------|---------|
| m15    | 2         | 0.7621  |
| m24    | 3         | 0.7919  |
| m28    | 4         | 0.7518  |
| m17    | 5         | 0.7498  |
| m22    | 6         | 0.7377  |

### Step 4: Predict m05 QED (chain_len = 11)
Fitted polynomials through the 5 data points, extrapolated to x=11:
- Degree 1: prediction = 0.695 (RMSE = 0.0131)
- Degree 2: prediction = 0.5416 (RMSE = 0.0119) ← **chosen**
- Degree 3: prediction = 2.13 (unphysical, QED must be in [0,1])
- Degree 4: prediction = -12.1 (unphysical)

Chose degree 2: lowest RMSE among physically plausible models. The declining QED with increasing chain length is consistent with increasing lipophilicity (m05 alogp = 4.956).

**Predicted QED for m05: 0.5416**

### Step 5: Task 2 — find qualifying molecule (4 measurements)
Constraints: QED >= 0.6, PSA >= 40, not in scaffold A.

| mol_id | scaffold | chain | QED    | psa     | Qualifies? |
|--------|----------|-------|--------|---------|------------|
| m09    | B        | 2     | 0.5826 | 72.24   | No (QED<0.6) |
| m02    | B        | 3     | 0.6046 | 72.24   | **YES**    |
| m01    | C        | 4     | 0.3187 | 110.02  | No (QED<0.6) |
| m12    | B        | 4     | 0.4709 | 72.24   | No (QED<0.6) |

**Selected: m02** — QED=0.6046, PSA=72.24>=40, scaffold B != scaffold A.

## Budget usage
- 3 list_pool pages: 3 units
- 5 measurements (m05 series): 5 units
- 4 measurements (Task 2 candidates): 4 units
- **Total: 12/12**
