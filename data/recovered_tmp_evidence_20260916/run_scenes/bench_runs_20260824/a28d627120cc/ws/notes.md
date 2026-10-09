# Task notes

## Setup
- Budget: 12 `measure_qed` calls total, shared by Task 1 and Task 2.
- m05 (target, chain_len 11) is NOT synthesizable → must predict from its series.

## Scaffolds (from group_by_scaffold)
m05 belongs to scaffold **`COc1ccc(cc1)C(=O)N`** (methoxy-benzamide). Its series members:
- m15 chain 2, m24 chain 3, m28 chain 4, m17 chain 5, m22 chain 6, m05 chain 11 (target, not synthesizable)

Other scaffolds:
- `O=[N+]([O-])c1ccc(cc1)C(=O)N` (nitro) — PSA 72.24 (≥40 ✓)
- `OCC(O)C(O)C(O)C(=O)N` (polyol) — PSA 110.02 (≥40 ✓)
- `c1ccc(cc1)-c1ccc(cc1)C(=O)N` (biphenyl) — PSA 29.1 (<40 ✗) — excluded by psa constraint

## Task 2 candidates (psa ≥ 40 AND not m05's series)
Nitro series: m02, m06, m09, m10, m12, m29, m31, m32, m33
Polyol series: m01, m03, m07, m14, m18, m19, m21, m25, m27
(Biphenyl all excluded — psa 29.1 < 40)

## Plan
- Task 1: measure m15, m24, m28, m17, m22 (5 calls), fit trend, evaluate at chain 11.
- Task 2: with remaining ~7 calls, measure promising candidates (short-chain nitro first — small MW/ALOGP/ROTB) until QED ≥ 0.6 found.

## Task 1 measurements (m05 series, scaffold `COc1ccc(cc1)C(=O)N`)
| mol | chain | QED |
|-----|-------|-----|
| m15 | 2 | 0.7621 |
| m24 | 3 | 0.7919 |
| m28 | 4 | 0.7518 |
| m17 | 5 | 0.7498 |
| m22 | 6 | 0.7377 |

5/12 budget used.

## Task 1 fit / prediction for m05 (chain 11)
- Quadratic direct fit → 0.5416 (rmse 0.012)
- Linear direct fit → 0.695 (rmse 0.013)
- Disagreement because chain 11 is far extrapolation. Used free QED-component breakdowns to adjudicate.
- Component view: ROTB score (0.993→0.970→0.848→0.689→0.533 across chains 2-6) drops sharply; at chain 11 (12 rotb) it → ~0, a hard ceiling (real component ≈ 0). Recombined component prediction ≈ 0.548.
- Quadratic (0.5416) and component recombination (0.548) agree → **m05 QED ≈ 0.5416**.

## Task 2 candidates (psa ≥ 40, not m05's series)
Nitro (psa 72.24): m09(c2), m02(c3), m12(c4), m32(c5), m33(c6), m29(c7), m31(c8), m06(c9), m10(c10)
Polyol (psa 110.02): m14(c2), m27(c3), m01(c4), m19(c5), m03(c6), m21(c7), m18(c8), m07(c9), m25(c10)
- Start with short chains (best ROTB/MW/ALOGP): m09, m02, m12 (nitro); m14, m27 (polyol).

## Task 2 measurements
| mol | scaffold | psa | QED | ≥0.6? |
|-----|----------|-----|-----|-------|
| m09 | nitro | 72.24 | 0.5826 | no |
| m02 | nitro | 72.24 | 0.6046 | **yes** ✓ |
| m12 | nitro | 72.24 | 0.4709 | no |

8/12 budget used. m02 qualifies on all three:
1. measured QED 0.6046 ≥ 0.6 ✓
2. psa 72.24 ≥ 40 ✓
3. nitro scaffold ≠ m05's methoxy scaffold ✓ (verified by group_by_scaffold)
Stopped — no need to spend remaining 4 calls.

m02 component check: ALERTS 0.2414 (nitro warheads the alerts score, the only weak part), but the other 7 parts are strong so overall still clears 0.6.

## Final answers
- target_qed (m05): 0.5416 (quadratic extrapolation of its own congeneric series, corroborated by component-level recombination)
- lead_mol_id: m02
- lead_qed_measured: 0.6046
