# Triage notes — compound collection

## Target profile (both required)
1. measured `logS` in [-3, -2] inclusive
2. measured `bbb` = 1 (brain penetrant)

Budget: 8 assays total for 20 compounds. Must be selective.

## Descriptor-based reasoning
- BBB favored by: low TPSA (<~60-70), low HBD (0-1), moderate clogp, MW <~400.
  HBD >= 2 and TPSA > ~90 strongly disfavour BBB.
- logS in [-3,-2] roughly tracks clogp: higher clogp -> lower solubility.
  Sweet spot (rough, uncertain) clogp ~2.5-3.5. clogp >~4.2 likely too insoluble
  (logS < -3); clogp <~1 likely too soluble (logS > -2). Melting point (unknown)
  adds scatter, so this is noisy.

## Quick triage of all 20
Clear BBB-fail / too-polar (skip): z001543(tpsa119,hbd5), z002216(tpsa98,hbd2),
z002683(tpsa100,hbd2), z015334(tpsa81,clogp0.74).
Clear solubility-fail / too-lipophilic (skip): z000889(clogp8.84), z002010(4.28),
z002011(4.32), z002891(4.72). Borderline-too-insoluble: z002015(4.14).
Too soluble by clogp (skip): z001510(caffeine,clogp-1.04), z002746(1.55).
Note caffeine (z001510): real logS ~-1 -> fails solubility; confirmed skip.

## Drug-identification hints (real drugs -> known logS/BBB)
- z003001 = ketamine (2-chlorophenyl Me-amino cyclohexanone): BBB yes, logS ~ -2. -> likely QUAL
- z002040 = glutethimide (phenylethyl glutarimide): sedative, BBB yes, logS ~ -2.5 -> likely QUAL
- z002730 = thalidomide (phthalimido-glutarimide): BBB yes; logS uncertain (-2.4 vs -3.7) -> test
- z002052 = heroin (diacetyl morphine): BBB excellent, logS ~ -2..-2.8 -> test
- z001999 = oripavine/thebaine-type opiate: BBB yes, logS ~ -2.2 -> test
- z000364 = lignan/podophyllotoxin-like (tpsa83): BBB uncertain, clogp2.87 sweet -> test (calibration)
- z002170 = benzotriazinone, clogp3.0/tpsa34.9/hbd0: clean CNS profile -> likely QUAL
- z002003 = triazole-chlorophenyl, clogp3.58/tpsa43.1/hbd0 -> test

## Strategy: adaptive, 2 batches of 4
Batch 1 (most confident + clogp-spread for calibration):
  z002170 (clogp3.0), z003001 (clogp2.9 ketamine), z002040 (clogp1.77 glutethimide),
  z002730 (clogp0.09 thalidomide)
Then calibrate logS-vs-clogp offset and pick Batch 2 (4) from remaining candidates.

## Results

### Batch 1 (4 assays)
| mol_id | (identity) | clogp | tpsa | hbd | logS | bbb | QUAL? |
|---|---|---|---|---|---|---|---|
| z002170 | 3-Me-benzotriazin-4-one | 3.0 | 34.9 | 0 | -2.92 | 1 | YES |
| z003001 | ketamine | 2.9 | 29.1 | 1 | -0.137 | 1 | no (too soluble) |
| z002040 | glutethimide | 1.77 | 46.2 | 1 | -2.3374 | 1 | YES |
| z002730 | thalidomide | 0.09 | 83.6 | 1 | -2.6758 | 1 | YES |

3 of 4 qualify. Offset (logS - clogp): z002170 -5.92, z003001 -3.04, z002040 -4.11, z002730 -2.77.
=> clogp alone is a weak logS predictor (huge spread). All four are BBB-permeable
(low tpsa + hbd<=1 confirms). The 3 qualfiers span clogp 0.09-3.0.

### Batch 2 plan (4 assays left)
Remaining plausible candidates (BBB-favorable descriptors), pick 4 across clogp spread:
- z002052 heroin (clogp1.99, tpsa65.1, hbd0) — strong BBB, logS likely -2..-2.8 -> test
- z002003 (clogp3.58, tpsa43.1, hbd0) — logS could be just under -3 (borderline) -> test
- z000364 (clogp2.87, tpsa83.1, hbd1) — borderline BBB (tpsa83) -> test
- z001999 (clogp1.5, tpsa41.9, hbd1) — logS ~-2 -> test

### Batch 2 (4 assays; budget now exhausted)
| mol_id | (identity) | clogp | tpsa | hbd | logS | bbb | QUAL? |
|---|---|---|---|---|---|---|---|
| z002052 | heroin (diacetylmorphine) | 1.99 | 65.1 | 0 | -2.7894 | 1 | YES |
| z002003 | triazole-chlorophenyl | 3.58 | 43.1 | 0 | -3.6 | 1 | no (too insoluble) |
| z000364 | lignan (podophyllo-type) | 2.87 | 83.1 | 1 | -0.9482 | 0 | no (both fail) |
| z001999 | thebaine-type opiate | 1.5 | 41.9 | 1 | -1.522 | 1 | no (too soluble) |

### Final confirmed qualifying compounds (logS in [-3,-2] AND bbb=1)
1. z002170 — logS -2.92, bbb 1
2. z002040 — logS -2.3374, bbb 1
3. z002730 — logS -2.6758, bbb 1
4. z002052 — logS -2.7894, bbb 1

Budget fully spent (8/8). Untested compounds remain unconfirmed and are NOT
reported, per instructions.

## Summary
- 8 assays run on: z002170, z003001, z002040, z002730, z002052, z002003, z000364, z001999.
- 4 qualify: z002170, z002040, z002730, z002052.
- Notable false predictions vs descriptor heuristics:
  - z003001 (ketamine): BBB-perfect + clogp2.9 predicted ~-2, but logS=-0.137 (too soluble).
  - z000364: clogp2.87/tpsa83 predicted borderline-qualify, but logS=-0.95 + bbb=0 (double fail).
  => clogp is not a reliable logS proxy here; measurement was essential.


