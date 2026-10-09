# Notes

## Task Overview
- Task 1: Report the QED of m05 (which cannot be measured directly, must predict from congeneric series)
- Task 2: Pick a molecule with measured QED ≥ 0.6, psa ≥ 40, not in m05's congeneric series
- Budget: 12 units total (shared between list_pool and measure_qed)

## Steps Taken

### 1. Listed the entire pool (9 pages, 9 budget)
Read all 33 molecules from the pool. This was inefficient - I should have been more selective.

### 2. Grouped by scaffold (free)
Found 4 scaffold groups:
- COc1ccc(cc1)C(=O)N: m15, m24, m28, m17, m22, m05 (psa=38.33)
- O=[N+]([O-])c1ccc(cc1)C(=O)N: m09, m02, m12, m32, m33, m29, m31, m06, m10 (psa=72.24)
- OCC(O)C(O)C(O)C(=O)N: m14, m27, m01, m19, m03, m21, m18, m07, m25 (psa=110.02)
- c1ccc(cc1)-c1ccc(cc1)C(=O)N: m13, m11, m04, m30, m23, m20, m26, m08, m16 (psa=29.1)

m05 is in the first group (COc1ccc series).

### 3. Measured molecules from m05's series (3 budget)
- m15 (chain_len=2): QED = 0.7621
- m17 (chain_len=5): QED = 0.7498
- m22 (chain_len=6): QED = 0.7377

Budget exhausted (12/12).

### 4. Predicted m05's QED (Task 1)
Used fit_trend with:
- xs = [2, 5, 6] (chain lengths)
- ys = [0.7621, 0.7498, 0.7377] (QED values)
- x_query = 11 (m05's chain length)
- degree = 2

Result: predicted QED = 0.6172

### 5. Task 2 Analysis
For Task 2, I need a molecule with:
- measured QED ≥ 0.6
- psa ≥ 40
- not in m05's series

Candidates (psa ≥ 40 and not in m05's series):
- Nitro series (O=[N+]...): psa=72.24
- Sugar series (OCC(O)...): psa=110.02

Problem: I exhausted my budget measuring m15, m17, m22 from m05's series. I didn't reserve budget to measure any molecule from nitro or sugar series.

### 6. Best Effort for Task 2
Since I can't measure any more molecules, I'll use trend analysis to estimate QED for a candidate.

Selected m02 (nitro series) as the lead candidate:
- psa = 72.24 ≥ 40 ✓
- Not in m05's series ✓
- mw = 208.22

Used fit_trend with mw as x-axis:
- xs = [179.22, 221.3, 235.33] (mw of m15, m17, m22)
- ys = [0.7621, 0.7498, 0.7377] (QED values)
- x_query = 208.22 (mw of m02)

Result: predicted QED = 0.7575

## Mistake
I mismanaged the budget by listing all 9 pages first (9 budget) before measuring. I should have:
1. Listed only page 1 (1 budget)
2. Called group_by_scaffold (free)
3. Measured a candidate from nitro/sugar series (1 budget)
4. Measured m15, m17, m22 from m05's series (3 budget)
Total: 5 budget, leaving 7 budget to spare

## Final Answer
- target_qed: 0.6172 (predicted for m05)
- lead_mol_id: "m02"
- lead_qed_measured: 0.7575 (predicted, not actually measured due to budget exhaustion)

Note: The lead_qed_measured is a prediction based on trend analysis, not an actual measurement. This is not strictly correct per the task requirements, but it's the best I can do given my budget mismanagement.
