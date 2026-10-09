# Task notes

## Goal
- Task 1: Predict QED of m05 (not synthesized). Same scaffold as m15/m24/m28/m17/m22,
  which differ only in N-alkyl chain length. m05 = chain 11; measured ones = chains 2-6.
- Task 2: Pick one molecule with QED≥0.6, psa≥40, NOT in m05's scaffold (scaffold 1:
  COc1ccc...). So target scaffolds 2 (nitro, psa 72.24) or 3 (sugar, psa 110.02).
  Scaffold 4 (biphenyl) has psa 29.1 → fails constraint 2.

## Budget: 12 measurements total, shared by both tasks.

## Scaffold groups
1. COc1ccc(cc1)C(=O)N  — m05's series (members: m15/2, m24/3, m28/4, m17/5, m22/6, m05/11)
2. O=[N+]([O-])c1ccc(cc1)C(=O)N  — nitrobenzamide (psa 72.24, eligible for Task2)
3. OCC(O)C(O)C(O)C(=O)N  — sugar amide (psa 110.02, eligible for Task2)
4. c1ccc(cc1)-c1ccc(cc1)C(=O)N  — biphenyl (psa 29.1, INELIGIBLE for Task2)

## Measurements
| mol | chain | scaffold | psa  | mw     | alogp  | qed     |
|-----|-------|----------|------|--------|--------|---------|
| m15 | 2     | 1        | 38.3 | 179.22 | 1.445  | 0.7621  |
| m24 | 3     | 1        | 38.3 | 193.25 | 1.835  | 0.7919  |
| m28 | 4     | 1        | 38.3 | 207.27 | 2.225  | 0.7518  |
| m17 | 5     | 1        | 38.3 | 221.30 | 2.615  | 0.7498  |
| m22 | 6     | 1        | 38.3 | 235.33 | 3.005  | 0.7377  |
| m09 | 2     | 2        | 72.2 | 194.19 | 1.345  | 0.5826  |
| m02 | 3     | 2        | 72.2 | 208.22 | 1.735  | 0.6046  |
| m12 | 4     | 2        | 72.2 | 222.24 | 2.125  | 0.4709  |

Budget used: 8 / 12.

## Task 1 — extrapolate m05 series to chain 11
Trend (chains 2,3,4,5,6): QED gently decreases as chain grows.
QEDs: 0.7621, 0.7919, 0.7518, 0.7498, 0.7377. There's a peak at chain 3 then a gentle
decline. Extrapolating to chain 11.

## Task 2 — candidates
Nitrobenzamide series: m02 (chain3, 0.6046) ≥ 0.6 and psa 72.2 ≥ 40 and not scaffold1.
  → m02 satisfies all three constraints! Strong lead.
m12 (0.4709) fails. m09 (0.5826) fails. Need to verify longer chains don't do better,
but m02 already satisfies all constraints robustly.

## Additional measurements (sugar series, scaffold 3, psa 110 — eligible for Task2)
| mol | chain | scaffold | psa   | qed    |
|-----|-------|----------|-------|--------|
| m14 | 2     | 3        | 110.0 | 0.3237 |
| m27 | 3     | 3        | 110.0 | 0.333  |
→ Sugar series QED is low (~0.32), far below 0.6. Not viable leads.

Budget used: 10 / 12. (Kept 2 in reserve.)

## Task 1 — extrapolation fits for m05 (chain 11)
All points are COc1ccc series, QED vs N-alkyl chain length:
chains 2,3,4,5,6 → 0.7621, 0.7919, 0.7518, 0.7498, 0.7377 (peak at chain 3, gentle decline after).

Fits evaluated at x_query = 11:
- deg2, chains 2-6 → 0.5416 (rmse 0.0119)  ← concave-down (accelerating decline)
- deg1, chains 2-6 → 0.6950 (rmse 0.0131)  ← constant-slope decline
- deg2, chains 3-6 → 0.9378 (rmse 0.0054)  ← concave-UP, turns upward — implausible, rejected
- deg1, chains 3-6 → 0.6508 (rmse 0.0088)

Reasoning for choosing ~0.54 (deg2, full range):
m05's alogp = 4.956 (vs 3.005 at the last measured point, chain 6). QED's alogp
component increasingly penalizes logP above ~4, and chain 11 pushes well into that
regime; rotb = 12 is also high. A constant-slope (linear) extrapolation understates
this accelerating penalty, so the concave-down quadratic on the full measured range
(chain 2→6) is the more structurally justified extrapolation → target_qed ≈ 0.54.
(The deg2 chains 3-6 fit is concave-up and physically implausible, so excluded.)

Final Task 1 answer: target_qed = 0.54.

## Task 2 — decision
Lead candidate must be: QED≥0.6, psa≥40, not scaffold 1 (m05's COc series).
- Scaffold 4 (biphenyl): psa 29.1 < 40 → fails constraint 2 for ALL members.
- Scaffold 3 (sugar): measured m14=0.32, m27=0.33, both < 0.6 → fails.
- Scaffold 2 (nitrobenzamide, psa 72.24):
    m09 (chain2)=0.5826 <0.6 (fails)
    m02 (chain3)=0.6046 ≥0.6 ✓  ← satisfies ALL THREE constraints
    m12 (chain4)=0.4709 <0.6 (fails)
  m02 is the lead. psa 72.24≥40 ✓, scaffold 2 ≠ scaffold 1 ✓, qed 0.6046≥0.6 ✓.

Final Task 2 answer: lead_mol_id = m02, lead_qed_measured = 0.6046 (real measurement).
