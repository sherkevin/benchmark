# Task Notes

## Goal
Triage 20 compounds. Qualifying compound = measured `logS` in [-3, -2] AND `bbb` = 1.
Budget: 8 `measure_admet` calls total.

## Strategy
The catalogue has no solubility or BBB data — I must measure. With 20 compounds and 8 assays,
I cannot measure all. Use cheap descriptors to pre-filter to the most plausible candidates,
then confirm with assays.

### Reasoning about logS (aqueous solubility, log10 mol/L)
logS in [-3, -2] is moderately-low solubility (1-10 mM roughly). General trends:
- Higher clogp (more lipophilic) → lower solubility (more negative logS).
- More HBD/HBA and TPSA → higher solubility (less negative logS).
- For logS ~ -2 to -3, we want moderate lipophilicity, not extreme.

Rough logS estimate rule of thumb (General Solubility Equation, GSE):
  logS ≈ 0.5 - 0.01(mw - 20) - clogp   (when not ionized) → roughly 0.8 - clogp - 0.01*(mw)
So logS ≈ 0.8 - clogp (ignoring mw term for moderate mw). For logS = -2 to -3:
  -2 = 0.8 - clogp → clogp ≈ 2.8
  -3 = 0.8 - clogp → clogp ≈ 3.8
So clogp roughly in [2.8, 3.8] is a sweet spot (very rough).

### Reasoning about BBB (bbb=1 means penetrant)
BBB penetration favored by: low TPSA (<~70-90), low HBD (≤~1-2), moderate clogp (~2-4),
low mw (<~450). High TPSA, many HBD, high mw, or very polar → poor BBB.

### Combined filter
Want compounds that are BOTH moderately lipophilic (for logS -2 to -3) AND BBB-friendly.
These are somewhat aligned (moderate clogp, low polarity helps both sides of BBB, but
too lipophilic kills solubility).

Let me list candidates with key descriptors (mw, clogp, tpsa, hbd):

| mol_id | mw | clogp | tpsa | hbd | notes |
|--------|-----|------|------|-----|-------|
| z000364 | 399.4 | 2.87 | 83.1 | 1 | many O, rings 3, rotb5 — tpsa borderline-high for BBB |
| z000889 | 430.7 | 8.84 | 29.5 | 1 | clogp way too high → logS far below -3 (insoluble) |
| z001510 | 180.2 | -1.04 | 72.7 | 1 | clogp negative → logS too high (>0), fails logS window |
| z001543 | 286.3 | -1.64 | 119.6 | 5 | polar sugar, logS too high, tpsa/HBD too high for BBB |
| z001999 | 299.4 | 1.5 | 41.9 | 1 | clogp 1.5 → logS ~ -0.7, too soluble (logS>-2) |
| z002003 | 308.8 | 3.58 | 43.1 | 0 | clogp 3.58 in sweet spot, tpsa low, hbd0 → strong BBB+logS candidate |
| z002010 | 339.5 | 4.28 | 29.5 | 0 | clogp 4.28 → logS maybe ~-3.5 (too low?), tpsa low BBB ok |
| z002011 | 325.8 | 4.32 | 30.2 | 0 | clogp 4.32 → logS maybe too low, tpsa low BBB ok |
| z002015 | 336.5 | 4.14 | 23.6 | 0 | clogp 4.14 → logS maybe -3.3, borderline; tpsa very low → BBB ok |
| z002040 | 217.3 | 1.77 | 46.2 | 1 | clogp 1.77 → logS ~ -1, too soluble |
| z002052 | 369.4 | 1.99 | 65.1 | 0 | clogp 1.99 → logS ~ -1.2, too soluble |
| z002170 | 250.3 | 3.0 | 34.9 | 0 | clogp 3.0 → logS ~ -2.2, in window! tpsa low → BBB ok. CANDIDATE |
| z002216 | 250.3 | 0.86 | 98.0 | 2 | tpsa high, hbd2, clogp 0.86 → logS high & BBB poor |
| z002683 | 273.2 | 2.55 | 100.7 | 2 | tpsa high + nitro → BBB poor; logS maybe ok but BBB fails |
| z002730 | 258.2 | 0.09 | 83.6 | 1 | clogp ~0 → logS too high; tpsa high BBB poor |
| z002746 | 231.3 | 1.55 | 30.2 | 0 | clogp 1.55 → logS ~ -0.8, too soluble |
| z002764 | 318.3 | 3.56 | 66.8 | 2 | clogp 3.56 in window, tpsa 66.8 borderline, hbd2 borderline BBB |
| z002891 | 314.5 | 4.72 | 34.1 | 0 | clogp 4.72 → logS ~ -3.9 too low (steroid), BBB ok but solubility fail |
| z003001 | 237.7 | 2.9 | 29.1 | 1 | clogp 2.9 → logS ~ -2.1 in window! tpsa low → BBB ok. CANDIDATE |
| z015334 | 306.3 | 0.74 | 81.6 | 1 | clogp 0.74 → logS too high; tpsa high BBB poor |

### Strong candidates (ranked)
1. z003001 — clogp 2.9, tpsa 29.1, hbd1, mw 237.7 → BEST fit both
2. z002170 — clogp 3.0, tpsa 34.9, hbd0, mw 250.3 → strong fit both
3. z002003 — clogp 3.58, tpsa 43.1, hbd0, mw 308.8 → good (logS maybe ~-2.8)
4. z002764 — clogp 3.56, tpsa 66.8, hbd2, mw 318.3 → logS ok, BBB borderline (hbd2/tpsa)
5. z002015 — clogp 4.14, tpsa 23.6, hbd0 → logS borderline (-3.3?), BBB strong
6. z002010 — clogp 4.28, tpsa 29.5, hbd0 → logS maybe too low, BBB strong
7. z002011 — clogp 4.32, tpsa 30.2, hbd0 → logS maybe too low
8. z000364 — clogp 2.87, tpsa 83.1, hbd1 → logS ok, BBB borderline-high tpsa

Plan: measure the top candidates. Start with the 4-5 most promising, use remaining budget
to expand.

## Measurements (budget 8 total)

| # | mol_id | logS | bbb | qualifies? |
|---|--------|------|-----|-------------|
| 1 | z003001 | -0.137 | 1 | NO (logS too high; amide/lactam much more soluble than GSE predicted) |
| 2 | z002170 | -2.92 | 1 | **YES** ✓ |
| 3 | z002003 | -3.6 | 1 | NO (logS below -3; simple aromatic, clogp 3.58) |
| 4 | z002764 | -2.9008 | 0 | NO (bbb=0; tpsa 66.8/hbd2 too polar for BBB) |

### Calibration insights (from real measurements)
- z002170 (clogp 3.0, simple aromatic) → logS -2.92. Good.
- z002003 (clogp 3.58, simple chloro-aromatic) → logS -3.6. More negative than GSE.
- z003001 (clogp 2.9, but a LACTAM/tertiary-amine) → logS -0.14. WAY more soluble than GSE.
  ⇒ Ionizable / H-bonding groups (amides, amines) raise logS substantially and unpredictably.
- BBB: z002764 (tpsa 66.8, hbd2) → bbb 0. So tpsa ≳ 60 + hbd ≥ 2 tends to fail BBB.
  ⇒ For bbb=1, want tpsa low (<~50) and hbd ≤ 1.

### Remaining budget: 4 assays (used 4 of 8)

### Revised candidate ranking for remaining assays
Want BOTH: low tpsa (BBB) + logS-in-window. Using calibration:
- Simple aromatics: logS ≈ 0.6 - 1.17*clogp → window [-3,-2] needs clogp ≈ [2.9, 3.1] — NARROW. clogp>3.5 → too insoluble; clogp<2.8 → too soluble (unless amide raises it).
- Amides/ionizable: logS shifted up; high-clogp amides might land in window.

Best remaining dual candidates (low tpsa + plausible logS):
1. z002015 — tpsa 23.6, hbd0 (strong BBB), clogp 4.14, amide → amide may pull logS up into window. TOP bet.
2. z002010 — tpsa 29.5, hbd0 (strong BBB), clogp 4.28, ester+tert-amine → amine may help. 2nd bet.
3. z002011 — tpsa 30.2, hbd0, clogp 4.32; structural analog of z002003 (→-3.6); extra F likely even more insoluble. Lower priority.
4. z002891 — tpsa 34.1, hbd0, clogp 4.72 steroid → likely too insoluble. Low priority.
5. z000364 — clogp 2.87 (logS likely in window like z002170) BUT tpsa 83.1/hbd1/6HBA → BBB likely 0. Low priority.

| 5 | z002015 | -3.82 | 1 | NO (clogp 4.14 too insoluble despite amide) |
| 6 | z002010 | -5.0097 | 1 | NO (clogp 4.28 ester+amine, way too insoluble) |

### Updated calibration (6 assays used, 2 left)
Confirmed trend: **clogp ≥ 4.1 → logS ≤ -3.8** (too insoluble). Even amides/amines
can't compensate. So exclude ALL clogp ≥ 4: z000889, z002010, z002011, z002015, z002891.
Also confirmed z003001 (clogp 2.9, lactam/amine) → logS -0.14 (too soluble): clogp ≤ ~2.9
WITH ionizable group → too soluble.

### The narrow window for logS ∈ [-3,-2] (after 6 measurements)
- z002170 (clogp 3.0, neutral aromatic) → logS -2.92 ✓ (only hit so far)
- z002003 (clogp 3.58, Cl-aromatic) → logS -3.6 (just below)
So for neutral aromatics, sweet spot clogp ≈ [3.0, 3.3]; clogp ~3.5 borderline-low.

### Remaining unmeasured candidates & their dual outlook
- z000364: clogp 2.87 (good for logS ~-2.9 like z002170) BUT tpsa 83.1 + hbd1 + 6HBA → BBB very likely 0. Not worth assay (BBB likely fail).
- z001999: clogp 1.5 → logS too soluble (~-0.7); even with O-bridge won't reach -2. Skip.
- z000889: clogp 8.84 → way too insoluble. Skip.
- z001510: clogp -1.04, xanthine-ish, mw 180 → logS >> -2. Skip.
- z001543: clogp -1.64, sugar → too soluble, too polar for BBB. Skip.
- z002040: clogp 1.77 → logS too soluble (~-1). Skip.
- z002052: clogp 1.99 → logS too soluble (~-1.2). Skip.
- z002746: clogp 1.55, dimethyl-pyrazolone-ish → logS too soluble (~-0.8). Skip.
- z002216: clogp 0.86, tpsa 98 → too soluble + BBB fail. Skip.
- z002683: clogp 2.55, tpsa 100.7 + nitro → BBB fail. Skip.
- z002730: clogp 0.09, tpsa 83.6 → too soluble + BBB fail. Skip.
- z015334: clogp 0.74, tpsa 81.6 → too soluble + BBB fail. Skip.
- z002011: clogp 4.32, Cl-F analog → too insoluble (≥4). Skip.
- z002891: clogp 4.72 → too insoluble. Skip.
- z000364 again: tpsa too high for BBB; but clogp 2.87 is in solubility window. **AMBIGUOUS on BBB.**

### Decision: spend final 2 assays where BOTH criteria have a real chance
Only candidates with clogp ∈ ~[3.0, 3.3] AND low tpsa (BBB-favorable) AND neutral stand a
chance. After excluding the above, NONE of the remaining neutral-aromatic candidates
sit cleanly in [3.0,3.3] with low tpsa except z002170 (already confirmed ✓).

z000364 is the single ambiguous case: clogp 2.87 (solubility window ✓) but tpsa 83.1/hbd1
(BBB uncertain — z002764 with tpsa 66.8/hbd2 failed BBB, but z000364 has hbd1 which is
friendlier; still tpsa 83 is high). It's a real candidate. → assay it.

For the 8th assay, the best remaining "real chance" candidates are limited. z000364
already covers the clogp≈2.9 slot. No other remaining compound has BOTH a plausible logS
window AND low tpsa. The most defensible 8th pick: z000364 is the only real shot at a
2nd qualifier. Re-examining — actually re-assaying an uncertain one isn't possible
(single result per compound). With 1 remaining assay after z000364 and no other strong
dual candidate, I'll spend it on z000364 and hold the last if no dual candidate exists.

Actually reassess: is there ANY other compound with a genuine dual chance?
- z000364: clogp 2.87 (logS likely ~-2.9 ✓), tpsa 83.1 (BBB ~ maybe 0). Dual chance ~40%.
- That's it for dual plausibility. The 8th assay: best use is z000364 (only dual shot).
  I'll measure z000364 with assay 7, then decide on assay 8.

**Refined logS model** (using the two clean neutral-aromatic points):
  z002170: clogp 3.0 → logS -2.92
  z002003: clogp 3.58 → logS -3.6
  ⇒ logS ≈ 0.59 − 1.17·clogp  →  logS window [-3,-2] needs clogp ≈ [2.2, 3.1] (neutral aromatics).

Dark-horse 8th pick: z002040 — clogp 1.77 (GSE predicts logS ~-1, too soluble) BUT it's a
rigid cyclic imide (3-ring fused, succinimide) with strong crystal packing → logS penalty
could push it to ~-2.0. tpsa 46.2 + hbd1 = BBB-favorable. Real dual chance.

### Assay 7 & 8 results
| 7 | z000364 | -0.9482 | 0 | NO (logS too soluble AND bbb=0 — 6 OMe/tonsk polarity) |
| 8 | z002040 | -2.3374 | 1 | **YES** ✓ (cyclic imide crystal-packing penalty confirmed the dark-horse call) |

## Final results (8/8 budget used)
Qualifying compounds (measured logS ∈ [-3,-2] AND bbb=1):
- **z002170** (logS -2.92, bbb 1)
- **z002040** (logS -2.3374, bbb 1)

All other measured compounds failed at least one criterion:
- z003001: logS -0.14 (too soluble)
- z002003: logS -3.6 (too insoluble)
- z002764: bbb 0 (tpsa 66.8/hbd2)
- z002015: logS -3.82 (too insoluble)
- z002010: logS -5.01 (too insoluble)
- z000364: logS -0.95 (too soluble) + bbb 0

Unmeasured compounds were triaged out by descriptor analysis (no dual chance):
clogp≥4 → too insoluble (excluded z000889, z002010✓, z002011, z002015✓, z002891);
clogp≤2 → too soluble for neutral (excluded z001510, z001543, z001999, z002040✓ [imide exception], z002746, z015334, z002730, z002216, z002683);
high tpsa/BBB-fail (excluded z002764✓, z000364✓, z002683, z002216, z015334, z001543).



