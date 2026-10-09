# Triage notes

## Task
Find compounds that satisfy BOTH:
1. measured logS in [-3, -2] (inclusive)
2. measured bbb = 1

Budget: 8 assays total (`measure_admet(mol_id)`). 20 candidates. Catalogue has
only cheap descriptors (mw, clogp, tpsa, hbd, hba, rotb, rings) — no solubility/BBB.

## Strategy
- BBB penetration needs low TPSA (<~70) and low HBD (<~3). Rule out high-TPSA/high-HBD.
- logS roughly anti-correlates with clogp (GSE: logS ≈ k - clogp, k~0.2-0.5). Target
  clogp roughly 2.5-3.5 for logS in [-3,-2], but crystallinity (MP) shifts this per
  chemotype, so each compound is somewhat independent.
- Run first batch of 4 to calibrate, then spend remaining 4 on best borderline picks.

## Candidate triage (clogp | tpsa | hbd | identity | logS est | BBB est)
- z000364  2.87 | 83.1 | 1 | morphinan/thebaine-like | ~-2.4 | borderline BBB
- z000889  8.84 | 29.5 | 1 | tocopherol-like | ~-8 | yes -> SKIP (way too insoluble)
- z001510 -1.04 | 72.7 | 1 | caffeine | ~-1 | yes -> SKIP (too soluble)
- z001543 -1.64 |119.6 | 5 | glycoside | ~+2 | no -> SKIP
- z001999  1.50 | 41.9 | 1 | morphine | ~-2.7? | yes -> borderline (free base insoluble)
- z002003  3.58 | 43.1 | 0 | triazolobenzodiazepine | ~-3.5? | yes -> risk too insoluble
- z002010  4.28 | 29.5 | 0 | PCP-ester | ~-3.8 | yes -> SKIP (too insoluble)
- z002011  4.32 | 30.2 | 0 | triazolo-F-benzo | ~-3.8 | yes -> SKIP (too insoluble)
- z002015  4.14 | 23.6 | 0 | fentanyl | ~-3.2 | yes -> risk slightly too insoluble
- z002040  1.77 | 46.2 | 1 | glutethimide | ~-2.8? | yes -> borderline
- z002052  1.99 | 65.1 | 0 | heroin (3,6-diacetylmorphine) | ~-2.8 | yes -> likely qualifier
- z002170  3.00 | 34.9 | 0 | methaqualone | ~-2.8 | yes -> likely qualifier
- z002216  0.86 | 98.0 | 2 | sulfonamide | ~-0.4 | no -> SKIP
- z002683  2.55 |100.7 | 2 | nitro-catechol ketone | ~-2 | no -> SKIP (BBB fail)
- z002730  0.09 | 83.6 | 1 | phthalimido-succinimide | ~+0.4 | no -> SKIP
- z002746  1.55 | 30.2 | 0 | aminopyrine-like | ~-1 | yes? -> SKIP (too soluble)
- z002764  3.56 | 66.8 | 2 | phenolphthalein | ~-3.5? | no? -> SKIP (BBB doubtful)
- z002891  4.72 | 34.1 | 0 | steroid (testosterone-like) | ~-4.2 | yes -> SKIP (too insoluble)
- z003001  2.90 | 29.1 | 1 | ketamine | ~-2.2 | yes -> likely qualifier
- z015334  0.74 | 81.6 | 1 | bis-triazole-benzyl | ~-0.2 | no -> SKIP

## Batch 1 (calibration, 4 assays): clogp spread 1.77-3.00
- z002052 (heroin, clogp 1.99)  -> logS -2.7894, BBB 1  => QUALIFIES
- z002040 (glutethimide, 1.77)  -> logS -2.3374, BBB 1  => QUALIFIES
- z002003 (triazoloBDZ, 3.58)  -> logS -3.6000, BBB 1  => too insoluble (matches pred)
- z002170 (methaqualone, 3.00) -> logS -2.9200, BBB 1  => QUALIFIES

Calibration: clogp ~1.77-3.00 lands in [-3,-2] for rigid, low-TPSA heterocycles;
clogp 3.58 was too insoluble. So pick remaining borderline candidates with clogp in
~1.5-3.0 and good BBB (low TPSA).

## Batch 2 (final 4 assays)
- z003001 (ketamine, 2.90)     -> logS -0.1370, BBB 1  => too soluble (HCl-like salt behavior)
- z001999 (morphine, 1.50)    -> logS -1.5220, BBB 1  => too soluble
- z002015 (fentanyl, 4.14)    -> logS -3.8200, BBB 1  => too insoluble
- z000364 (thebaine, 2.87)    -> logS -0.9482, BBB 0  => too soluble + BBB fail

Budget exhausted (8/8).

## Confirmed qualifiers (all via real measure_admet calls)
- z002052 : logS -2.7894 (in [-3,-2]), BBB 1
- z002040 : logS -2.3374 (in [-3,-2]), BBB 1
- z002170 : logS -2.9200 (in [-3,-2], > -3), BBB 1

## Untested candidates that could plausibly qualify (no budget left — reasoning only)
- z002764 (phenolphthalein, clogp 3.56, TPSA 66.8, HBD 2): rigid planar polyaromatic
  with two phenolic HBDs -> likely logS < -3 (z002003 at clogp 3.58 was -3.6) and
  borderline BBB (TPSA near 67, planar). Low odds of qualifying.
- z002746 (aminopyrine-like, clogp 1.55, TPSA 30.2): pyrazolones are quite soluble
  (logS ~ -0.5 to -1) -> likely too soluble.
No high-probability qualifier left untested.
