# Triage notes

## Target
- logS in [-3, -2] (inclusive) AND bbb == 1.
- Budget: 8 assays for 20 compounds. No catalogue solubility/BBB data.

## Strategy
The catalogue only has cheap descriptors. Use them to pick candidates likely to satisfy
BOTH criteria simultaneously:

- **Solubility (logS ~ -3 to -2 = moderately soluble):** driven mostly by clogP.
  Very low/negative clogP -> too soluble (logS > -2). Very high clogP -> too insoluble
  (logS < -3). Sweet spot ~ clogP 1.5-3.5, adjusted by MW/TPSA/HBD/ionization.
- **BBB (bbb=1):** needs low TPSA (<~70), low HBD (<=1), basic/neutral, MW < ~450.
  Compounds with TPSA>80 or HBD>=2 are very unlikely to cross BBB -> can never qualify
  regardless of logS. This is the harder filter, so prioritize BBB-plausible compounds.

## Descriptor filter (TPSA<~70 AND HBD<=1 AND clogP in [~1, ~3.8])
Pass: z001999, z002003, z002040, z002052, z002170, z002746, z002764, z003001,
z002010, z002011, z002015 (the last three clogP 4.1-4.3, flexible basic amines).

## Drug-ID / chemistry insight (helps judge bbb=1 and logS)
Recognized scaffolds among BBB-plausible candidates:
- z002040 = glutethimide (piperidinedione sedative) clogP1.77, MW217 -> BBB yes,
  logS ~-2.3. STRONG.
- z002170 = methaqualone (quinazolinone sedative) clogP3.0, MW250 -> BBB yes,
  logS ~-2.9 to -3.0. STRONG.
- z001999 = thebaine/oripavine-family opioid (basic amine, O-bridge) clogP1.5 -> BBB yes,
  logS ~-2.5 to -3. STRONG.
- z002052 = diacetyl congener of z001999 skeleton (heroin-like) clogP1.99 -> BBB yes,
  logS ~-2.7 to -3.2. MEDIUM-STRONG.
- z003001 = N-methyl-4-(2-chlorophenyl)piperidin-4-one (basic amine, fentanyl precursor)
  clogP2.9 -> BBB yes, logS ~-2 to -3 (ionization-dependent). MEDIUM.
- z002015 = fentanyl (potent opioid) clogP4.14 -> BBB certain, logS ~-3.2 (borderline low).
  MEDIUM.
- z002010 = dextropropoxyphene (opioid ester, basic amine) clogP4.28 -> BBB yes,
  logS ~-3 to -4 (borderline low). MEDIUM.
- z002746 = aminopyrine/aminophenazone (pyrazolone, weak base) clogP1.55 -> BBB yes,
  logS ~-1.1 (likely too soluble). LONG-SHOT (kept for bbb certainty + logS uncertainty).

## Excluded (with reason)
- z000364: TPSA 83, MW399 -> BBB unlikely + too insoluble.
- z000889: clogP 8.84 (tocopherol-like) -> far too insoluble.
- z001510: theophylline, clogP -1.04 -> too soluble (logS ~ -1.4 > -2) though bbb=1.
- z001543: glycoside, TPSA119, HBD5 -> no BBB, too polar.
- z002003 / z002011: tetrazoles, clogP 3.6-4.3, rigid -> logS likely < -3.5; weaker than picks.
- z002216 / z002683 / z002730: TPSA ~83-101, HBD>=2 -> no BBB.
- z002764: phenolphthalein, HBD2 -> BBB unlikely (0). logS ~-3.2 close to range but bbb fails.
- z002891: steroid, clogP 4.72 -> too insoluble.
- z015334: fluconazole, clogP 0.74 -> too soluble (~-1.6) despite bbb=1.

## Decision: assay these 8 (max budget), all BBB-plausible + logS-plausible:
z002040, z002170, z001999, z003001, z002015, z002052, z002010, z002746

## Results (from measure_admet) — budget 8/8 exhausted

| mol_id     | logS     | bbb | Qualifies (logS in [-3,-2] AND bbb=1)? |
|------------|----------|-----|----------------------------------------|
| z002040    | -2.3374  | 1   | YES  (-2.3374 in range)                |
| z002170    | -2.9200  | 1   | YES  (-2.92 in range)                  |
| z001999    | -1.5220  | 1   | no   (logS > -2, too soluble)          |
| z003001    | -0.1370  | 1   | no   (logS > -2, too soluble)          |
| z002015    | -3.8200  | 1   | no   (logS < -3, too insoluble)        |
| z002052    | -2.7894  | 1   | YES  (-2.7894 in range)                |
| z002010    | -5.0097  | 1   | no   (logS < -3, too insoluble)        |
| z002746    | -0.6286  | 1   | no   (logS > -2, too soluble)          |

## Confirmed qualifiers (reported in answer.json)
- z002040  (glutethimide)
- z002170  (methaqualone)
- z002052  (diacetyloripavine-family congener of z001999 skeleton)

Budget exhausted (8/8); no further assays possible. Non-assayed candidates are
NOT confirmed and therefore NOT reported (TASK.md requires every reported id be
confirmed by a real measure_admet call).
