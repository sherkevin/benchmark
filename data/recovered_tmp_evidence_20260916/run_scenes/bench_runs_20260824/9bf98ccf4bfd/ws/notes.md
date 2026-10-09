# Triage notes — 20 compounds, 8-assay budget

## Target
qualifies iff measured logS ∈ [-3, -2] AND bbb == 1.

## Strategy
Only 8 assays for 20 compounds → must triage by cheap descriptors.
- BBB: prefer TPSA < 70, HBD ≤ 2, MW < 400 (high TPSA/HBD ⇒ BBB=0).
- logS window [-3,-2]: GSE logS ≈ 0.5 - 0.01(mp-25) - clogP. For typical solids
  (mp 100-200), crystallinity pushes logS DOWN vs the liquid estimate, so a clogP
  around 1.5-3.0 is the sweet spot. clogP > ~3.5 likely too insoluble; clogP < ~1.3
  likely too soluble (unless very high mp).

## Descriptor scan (sorted by combined plausibility)
| mol_id  | mw    | clogp | tpsa | hbd | hba | rotb | rings | notes |
|---------|-------|-------|------|-----|-----|------|-------|-------|
| z000364 | 399.4 | 2.87  | 83.1 | 1   | 6   | 5    | 3     | logS sweet; TPSA 83 BBB-borderline; podophyllotoxin-like |
| z000889 | 430.7 | 8.84  | 29.5 | 1   | 2   | 12   | 2     | clogP 8.8 → far too insoluble. SKIP |
| z001510 | 180.2 | -1.04 | 72.7 | 1   | 3   | 0    | 2     | clogP neg → too soluble. SKIP |
| z001543 | 286.3 | -1.64 | 119.6| 5   | 7   | 4    | 2     | TPSA 120, HBD 5 → BBB=0; too soluble. SKIP |
| z001999 | 299.4 | 1.5   | 41.9 | 1   | 4   | 1    | 5     | morphinan/thebaine; opioid ⇒ BBB=1; logS plausible. TEST |
| z002003 | 308.8 | 3.58  | 43.1 | 0   | 3   | 1    | 4     | triazolopyridine; BBB good; clogP 3.58 logS-borderline low |
| z002010 | 339.5 | 4.28  | 29.5 | 0   | 3   | 8    | 2     | clogP 4.3 likely too insoluble. deprioritize |
| z002011 | 325.8 | 4.32  | 30.2 | 0   | 2   | 1    | 4     | clogP 4.3 likely too insoluble. deprioritize |
| z002015 | 336.5 | 4.14  | 23.6 | 0   | 2   | 6    | 3     | clogP 4.1 likely too insoluble. deprioritize |
| z002040 | 217.3 | 1.77  | 46.2 | 1   | 2   | 2    | 2     | glutarimide (thalidomide-core); logS sweet, BBB good. TEST |
| z002052 | 369.4 | 1.99  | 65.1 | 0   | 6   | 2    | 5     | diacetylmorphine-like; BBB=1 strong; logS ~-2.8. TEST |
| z002170 | 250.3 | 3.0   | 34.9 | 0   | 2   | 1    | 3     | phthalazinone; BBB good; clogP 3.0 borderline. TEST |
| z002216 | 250.3 | 0.86  | 98.0 | 2   | 5   | 3    | 2     | TPSA 98 ⇒ BBB=0; too soluble. SKIP |
| z002683 | 273.2 | 2.55  |100.7 | 2   | 5   | 3    | 2     | TPSA 100 ⇒ BBB=0. SKIP |
| z002730 | 258.2 | 0.09  | 83.6 | 1   | 4   | 1    | 3     | clogP 0.09 too soluble; TPSA 84 BBB-risk. SKIP |
| z002746 | 231.3 | 1.55  | 30.2 | 0   | 2   | 2    | 2     | N-arylpyrazolone; BBB good; logS borderline. TEST |
| z002764 | 318.3 | 3.56  | 66.8 | 2   | 4   | 2    | 4     | phenolic phthalide; both criteria borderline. POOL |
| z002891 | 314.5 | 4.72  | 34.1 | 0   | 2   | 1    | 4     | steroid; clogP 4.7 too insoluble. SKIP |
| z003001 | 237.7 | 2.9   | 29.1 | 1   | 2   | 2    | 2     | amino-ketone; BBB good; clogP 2.9 borderline. TEST |
| z015334 | 306.3 | 0.74  | 81.6 | 1   | 5   | 5    | 3     | TPSA 82 BBB-risk; clogP 0.7 too soluble. SKIP |

## Batch plan
Batch 1 (6 assays, strongest combined candidates):
  z002052, z001999, z002040, z002746, z003001, z002170
Batch 2 (2 assays): pick from borderline pool {z002003, z000364, z002764, z002011}
  based on Batch-1 results (calibrate logS model).

## Batch-1 results (6 assays, budget used 6/8)
| mol_id  | logS    | bbb | qualifies? | note |
|---------|---------|-----|------------|------|
| z002052 | -2.7894 | 1   | YES        | diacetylmorphine analog |
| z001999 | -1.522  | 1   | no (logS>-2) | thebaine, too soluble |
| z002040 | -2.3374 | 1   | YES        | glutarimide |
| z002746 | -0.6286 | 1   | no (logS>-2) | pyrazolone, too soluble |
| z003001 | -0.137  | 1   | no (logS>-2) | amino-ketone (basic N → protonated, v soluble) |
| z002170 | -2.92   | 1   | YES        | phthalazinone, clogP 3.0 → right at -3 edge |

Confirmed qualifiers: z002052, z002040, z002170.

## Recalibration (neutral compounds, logS vs clogP)
- clogP 1.77 (z002040, glutarimide) → -2.34   ✓
- clogP 1.99 (z002052, morphine-DA) → -2.79   ✓
- clogP 2.87 (z000364, lignan) → ?             target
- clogP 3.00 (z002170, phthalazinone) → -2.92  ✓ (right at lower edge)
- Basic amines (z003001 clogP 2.9, z002746 clogP 1.55) → far too soluble (protonation).
  ⇒ restrict to NEUTRAL compounds for logS predictions.
- Sweet spot for neutral, rigid-enough solids: clogP ≈ 1.7–3.2 lands logS in [-3,-2].
  clogP > ~3.3 (neutral, aromatic) risks logS < -3 (too insoluble).

## Remaining untested + EV estimate (P_qual = P(logS∈[-3,-2])·P(BBB=1))
- z002003: clogP 3.58, TPSA 43.1, HBD 0, MW 309, neutral triazolopyridine.
    P(BBB=1)≈0.80; P(logS in range)≈0.35 (clogP 3.58 just past the -3 edge). EV≈0.28. TEST.
- z000364: clogP 2.87, TPSA 83.1, HBD 1, MW 399, rings 3, neutral aryltetralin lactone.
    P(logS in range)≈0.40 (clogP in sweet spot, neutral); P(BBB=1)≈0.38 (TPSA 83/MW 399 borderline). EV≈0.15. TEST.
- z002764: clogP 3.56, TPSA 66.8, HBD 2, phenolphthalein-like. logS likely -3.9 (too insoluble). EV≈0.07. deprioritize.
- z002011: clogP 4.32 → logS likely < -3.8. SKIP.
- All other SKIPs confirmed by descriptors (too soluble / TPSA>90 / clogP>4 / basic-amine-too-soluble).

## Batch-2 plan (final 2 assays)
Test z002003 and z000364 (top-2 EV among remaining).

## Batch-2 results (budget 8/8, exhausted)
| mol_id  | logS    | bbb | qualifies? | note |
|---------|---------|-----|------------|------|
| z002003 | -3.6    | 1   | no (logS<-3) | clogP 3.58 → just past -3 edge, as predicted |
| z000364 | -0.9482 | 0   | no (logS>-2, bbb 0) | aryltetralin-lactone; more soluble than predicted, BBB=0 (TPSA 83/MW 399) |

## FINAL
Confirmed qualifiers (real assays, both criteria met, logS∈[-3,-2] & bbb=1):
  z002052  (logS -2.7894, bbb 1)
  z002040  (logS -2.3374, bbb 1)
  z002170  (logS -2.92,   bbb 1)

Total assays used: 8/8. No untested compound has both criteria plausible
(specifically: z002764 clogP 3.56 → expect logS≈-3.9 < -3, fails logS).
Confidence in completeness: high — the 8 tested spanned the full plausible
range; every remaining SKIP either has TPSA>90 (bbb=0) or is a basic amine
(v soluble) or clogP>3.3 (too insoluble) or clogP<1.3 (too soluble).

