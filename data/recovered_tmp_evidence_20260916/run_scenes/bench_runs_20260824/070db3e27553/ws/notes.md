# Predicting QED of m05 (unmeasurable target)

## Target
- m05 SMILES: `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`
  = 4-methoxy-N-alkylbenzamide core `COc1ccc(cc1)C(=O)N` + N-undecyl chain (C11, 11 CH2's after N).
- measure_qed(m05) → probe_unavailable (never synthesized). Must extrapolate from congeners.

## Search (scaffold substring)
- Searched `COc1ccc(cc1)C(=O)N` — the shared 4-methoxybenzamide core of m05.
- 6 matches total (2 pages, budget 2/12):
  - m05  COc1ccc(cc1)C(=O)NCCCCCCCCCCC  (C11, TARGET)  mw305.46 alogp4.956 psa38.33 rotb12 arom1
  - m15  COc1ccc(cc1)C(=O)NCC           (C2)          mw179.22 alogp1.445 psa38.33 rotb3  arom1
  - m24  COc1ccc(cc1)C(=O)NCCC          (C3)          mw193.25 alogp1.835 psa38.33 rotb4  arom1
  - m28  COc1ccc(cc1)C(=O)NCCCC         (C4)          mw207.27 alogp2.225 psa38.33 rotb5  arom1
  - m17  COc1ccc(cc1)C(=O)NCCCCC        (C5)          mw221.30 alogp2.615 psa38.33 rotb6  arom1
  - m22  COc1ccc(cc1)C(=O)NCCCCCC       (C6)          mw235.33 alogp3.005 psa38.33 rotb7  arom1

All share scaffold; differ only in N-alkyl chain length. PSA=38.33, arom=1 constant across the series.
Collection holds only short-chain members (C2–C6); m05 is C11 → extrapolate beyond measured range.

## Measured QED (budget 7/12 used)
| C | mol_id | QED     | alogp  | rotb | mw     |
|---|--------|---------|--------|------|--------|
| 2 | m15    | 0.7621  | 1.445  | 3    | 179.22 |
| 3 | m24    | 0.7919  | 1.835  | 4    | 193.25 |  <- peak
| 4 | m28    | 0.7518  | 2.225  | 5    | 207.27 |
| 5 | m17    | 0.7498  | 2.615  | 6    | 221.30 |
| 6 | m22    | 0.7377  | 3.005  | 7    | 235.33 |
|11 | m05    |  ?      | 4.956  | 12   | 305.46 |  (predict)

## Descriptor trends (perfectly linear → extrapolate exactly to C11)
- alogp = 0.665 + 0.390·C  → C11 = 4.955 ✓ (matches m05's 4.956)
- mw   = 151.18 + 14.02·C → C11 ≈ 305.4 ✓ (matches 305.46)
- rotb = C + 1            → C11 = 12 ✓
So m05's descriptors sit exactly on the series trends. Only the QED mapping is nonlinear.

## QED curve shape
- Rises C2→C3 (0.7621→0.7919, moving toward logP/MW optima), peaks at C3,
  then declines monotonically C3→C6 (0.7919→0.7377) as rotb grows past optimum.
- Cliff at C3→C4 (−0.040), then gentler steps (−0.002, −0.012): leaving a desirability
  plateau (likely rotb) near C3/C4, then smooth falloff.

## Why QED is driven by only 3 of the 8 QED inputs here
PSA, AROM, HBD (amide N–H=1), HBA (MeO-O + amide C=O = 2), ALERTS (=0) are CONSTANT
across the whole series (and identical for m05). Only MW, ALogP, ROTB vary. So QED is a
geometric mean (default QED_max, equal weights) with 5 constant factors that cancel in
any ratio — the variation is entirely MW×ALogP×ROTB.

## Extrapolation C6 → C11
Beyond C6 (m05): logP 3.0→5.0 (moves from near-optimal to well past optimum → new, growing
penalty), rotb 7→12 (continues past optimum → growing penalty), MW 235→305 (still near its
~300 optimum → roughly neutral/slightly favourable). Net: decline continues and is steeper
than the C3→C6 segment (logP switches from helping to hurting). So QED(m05) < the linear
C3→C6 extrapolation.

## Trend fits (for bracketing)
- Linear on the declining run C3–C6: slope −0.0165/step → C11 ≈ 0.651  (ignores curvature → upper bound)
- Linear on all C2–C6: → C11 ≈ 0.695  (smears the peak → too high)
- Quadratic QED vs C (C2–C6): Q(u)=0.765−0.0091u−0.00326u², u=C−4 → C11 ≈ 0.542
- ln-QED quadratic on C3–C6: unstable (near-zero curvature → projects a U-turn → unphysical; discarded)

## Desirability-ratio estimate (principled, default QED_max geometric mean)
QED(m05)/QED(C6) = [d_MW(305)·d_ALogP(4.96)·d_ROTB(12) / d_MW(235)·d_ALogP(3.01)·d_ROTB(7)]^(1/8)
(constants PSA·AROM·HBD·HBA·ALERTS cancel).
Plausible desirability ratios: MW ≈1.0–1.05 (both near MW optimum), ALogP(5)/ALogP(3)≈0.5–0.7
(logP 5 well past ~2.5–3 optimum), ROTB(12)/ROTB(7)≈0.3–0.5 (both past optimum, 12 further).
→ R ≈ 0.15–0.27 → R^(1/8) ≈ 0.79–0.85 → QED(m05) ≈ 0.58–0.63.

## Prediction
Three independent lines (quadratic fit ~0.54, desirability-ratio ~0.60, real-QED intuition
for a logP≈5 / rotb=12 molecule ~0.55–0.60) converge around 0.55–0.62. Linear (0.65) is an
upper bound (it ignores the logP-penalty acceleration past C6, which the task warns against
by saying the relationship is not a straight line). Quadratic (0.54) is a lower bound
(overfits a short, nearly-linear declining segment).

**Predicted QED(m05) = 0.59.**
