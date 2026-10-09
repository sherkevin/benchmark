# Task notes

## Setup
- 33 molecules in `data/pool.tsv` with free descriptors: mw, alogp, psa, rotb, arom.
- Several congeneric series: same scaffold, only N-alkyl chain length differs.
- Each scaffold has its OWN QED curve (shape + level differ between series).
- Budget: 12 measurements TOTAL shared by both jobs.

## Job 1 — predict QED of m05
- m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` (never synthesized → probe_unavailable).
- Scaffold = `COc1ccc(cc1)C(=O)N`, N-alkyl chain = 11 carbons.
- Same-series candidates (by eye): m15 (CC), m24 (CCC), m28 (CCCC), m17 (CCCCC), m22 (CCCCCC).
- Pool only holds SHORT-chain members (2-6), target is chain 11 → extrapolation beyond range.
- Fit a curve (degree 2), extrapolate to chain=11. Do NOT assume linear.

## Job 2 — pick a lead (one molecule) meeting ALL THREE:
1. measured QED ≥ 0.6 (must measure)
2. psa ≥ 40 (free, from tsv)
3. NOT in m05's series (free, from SMILES)

## Budget plan
- Job 1: measure methoxy series (m15,m24,m28,m17,m22) → fit quad → extrapolate to chain 11. Aim ~5.
- Job 2: remaining budget for measuring psa≥40 non-methoxy candidates until QED≥0.6 found.

## psa scan of pool (free)
- psa=38.33 (methoxy series = m05's series): m05,m15,m17,m22,m24,m28 → EXCLUDED by constraint 3
- psa=29.1 (biphenyl series): m04,m08,m11,m13,m16,m20,m23,m26,m30 → fails psa≥40
- psa=72.24 (nitro series, O=[N+]...): m02,m06,m09,m10,m12,m29,m31,m32,m33 → OK candidates
- psa=110.02 (sugar/polyol series, OCC(O)C(O)C(O)...): m01,m03,m07,m14,m18,m19,m21,m25,m27 → OK candidates

So Job 2 lead must come from nitro or sugar series.

## describe_structure (free) — m05's series confirmed
m05 scaffold `COc1ccc(cc1)C(=O)N`, chain_len 11. Pool congeners (same scaffold):
- m15 chain 2, m24 chain 3, m28 chain 4, m17 chain 5, m22 chain 6.
Target m05 (chain 11) is far beyond the measured range 2-6 → extrapolation.

## Job 1 measurements (methoxy series) + fit
Measured (chain → QED):
- m15 chain 2 → 0.7621
- m24 chain 3 → 0.7919  (peak)
- m28 chain 4 → 0.7518
- m17 chain 5 → 0.7498
- m22 chain 6 → 0.7377
Non-monotonic: rises to peak at chain 3, then declines — curved, NOT linear.

Quadratic fit (degree 2): QED = 0.74932 + 0.017024·x − 0.003264·x², RMSE 0.0119.
Extrapolation to chain 11 → **0.5416**. (Linear fit → 0.695 but RMSE worse 0.0131 and
ignores the downward curvature; task warns against assuming linear.) Chose quadratic 0.5416.

## Job 2 — lead search (psa≥40, non-methoxy, measured QED≥0.6)
Probed both eligible series at short chains:
- m09 (nitro, chain 2) → 0.5826 (just under 0.6)
- m14 (sugar, chain 2) → 0.3237 (far too low; sugar/polyol series penalized by QED,
  the heavy HBD/heteroatom load — dropped sugar series)
- m12 (nitro, chain 4) → 0.4709 (too low)
- m02 (nitro, chain 3) → **0.6046 ≥ 0.6 ✓**

m02 satisfies all three: QED 0.6046≥0.6; psa 72.24≥40; scaffold
`O=[N+]([O-])c1ccc(cc1)C(=O)N` ≠ m05's methoxy scaffold (confirmed by describe_structure).
→ **lead = m02, measured QED = 0.6046.** Stopped: one valid lead is enough; 3 budget left.

## Measurements used: 9/12
Job1: m15,m24,m28,m17,m22 (5). Job2: m09,m14,m12,m02 (4).

## Answers
- target_qed = 0.5416
- lead_mol_id = "m02"
- lead_qed_measured = 0.6046
- congener_ids = ["m15","m24","m28","m17","m22"]
