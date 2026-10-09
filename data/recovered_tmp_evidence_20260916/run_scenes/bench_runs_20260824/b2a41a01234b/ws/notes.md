# Triage notes

## Task
Find compounds with measured logS in [-3, -2] (inclusive) AND bbb = 1.
Budget: 8 assays total for 20 compounds. Must prioritize.

## Strategy
Can't assay all 20. Use cheap descriptors to pre-filter by likely solubility window
and likely BBB penetration.

### Solubility window reasoning (logS in [-3, -2])
General Solubility Equation (Yalkowsky): logS ≈ 0.5 - 0.01*(MP-25) - clogP.
For a typical crystalline solid (MP 100-200), the clogP window that lands logS in
[-3, -2] is roughly clogP 1.0-2.8. For flexible/low-MP molecules the window shifts
up (clogP ~2.5-3.5 still possible); for rigid high-MP fused scaffolds (purines,
quinazolinones, flavonoids, steroids) the window shifts DOWN — i.e. clogP 3+ rigid
scaffolds are usually too insoluble (logS < -3).

=> Sweet spot: clogP ~1.5-3.0, flexible molecules. Avoid clogP >=4 (too insoluble)
   and clogP <0 (too soluble). Avoid rigid high-MP scaffolds even at "sweet" clogP.

### BBB reasoning
BBB penetration favors: TPSA < ~70, HBD <= 2, clogP 2-4, MW < 450. Reject TPSA
>~80 (P-gp / too polar) and high HBD.

### Pre-filter results
REJECT (BBB unlikely, high TPSA):
- z000364 tpsa 83 (also podophyllotoxin-lignan, poor CNS)
- z001543 tpsa 119.6 hbd 5 (also clogp -1.64 too soluble)
- z002216 tpsa 98
- z002683 tpsa 100.7
- z002730 tpsa 83.6 (also clogp 0.09 too soluble)
- z015334 tpsa 81.6 (also clogp 0.74 too soluble)

REJECT (solubility way off):
- z000889 clogp 8.84 -> far too insoluble
- z001510 clogp -1.04 (caffeine/xanthine, logS ~ -1) too soluble
- z002010, z002011, z002015 clogp 4.1-4.3 -> too insoluble
- z002891 clogp 4.72 (steroid, logS ~ -3.8) too insoluble

BORDERLINE/PROBE (clogp 3.5+ rigid or BBB-risky, long-shots):
- z002003 clogp 3.58 triazole hbd0 tpsa43 — BBB good, solubility borderline-low
- z002764 clogp 3.56 chromanone hbd2 tpsa67 — borderline both
- z002170 clogp 3.0 quinazolinone (=methaqualone, logS ~-4.5) — likely insoluble

PRIORITY (clogp 1.5-3.0, flexible, good BBB) — most likely to satisfy BOTH:
1. z003001 clogp 2.9 tpsa 29.1 hbd1 (caprolactam, flexible) [best]
2. z002052 clogp 1.99 hbd0 tpsa65 (heroin-like, known CNS opioid)
3. z001999 clogp 1.5 tpsa 41.9 hbd1 (morphinan, known CNS opioid)
4. z002040 clogp 1.77 tpsa 46.2 hbd1 MW217 (cyclic imide, ethosuximide-like anticonvulsant)
5. z002746 clogp 1.55 tpsa 30.2 hbd0 (pyrazolone; aminopyrine-like, maybe too soluble)

## Assay plan (8 total)
Batch 1 (top 4): z003001, z002052, z001999, z002040
Batch 2 (4 more, decide after seeing batch 1): z002746 + 2-3 probes from
{z002003, z002764, z002170}.

## Results

### Batch 1 (assays 1-4)
| mol_id | logS | bbb | qualifies |
|--------|------|-----|-----------|
| z003001 | -0.137 | 1 | NO (too soluble) |
| z002052 | -2.7894 | 1 | YES |
| z001999 | -1.522 | 1 | NO (too soluble) |
| z002040 | -2.3374 | 1 | YES |

Confirmed qualifiers so far: z002052, z002040
Budget remaining: 4

### Reassessment
My solubility window was biased soluble for the flexible lactams/opioids
(z003001 -0.14, z001999 -1.52 both too soluble). The compound that hit the
window (z002052) is the diacetyl ester (heroin) — more lipophilic than its
clogP 1.99 suggested, because the ester masking raises effective lipophilicity.
z002040 (cyclic imide) sits right in window at -2.34.

Lesson: I under-estimated solubility for flexible lactams; the borderline-low
(probe) set at clogP 3.5 is more likely to actually land in [-3,-2]. Reprioritize
remaining 4 assays toward the clogP 3-3.6 rigid/borderline set, which I'd
previously flagged as "possibly too insoluble" — given the flexible ones came
out MORE soluble than predicted, the rigid ones may now sit right in window.

### Batch 2 plan (assays 5-8)
Probe the borderline-low-solubility set + the one untested mid-clogP pyrazolone:
- z002003 clogp 3.58 triazole tpsa43 hbd0 (BBB great; solubility the question)
- z002764 clogp 3.56 chromanone tpsa67 hbd2
- z002170 clogp 3.0 quinazolinone (methaqualone-like)
- z002746 clogp 1.55 pyrazolone tpsa30 hbd0 (mid-range, check if too soluble)

### Batch 2 results (assays 5-8) — budget exhausted
| mol_id | logS | bbb | qualifies |
|--------|------|-----|-----------|
| z002003 | -3.6 | 1 | NO (logS < -3, too insoluble) |
| z002764 | -2.9008 | 0 | NO (logS in window but BBB fails) |
| z002170 | -2.92 | 1 | YES |
| z002746 | -0.6286 | 1 | NO (too soluble) |

### Final
Confirmed qualifiers (each verified by a real measure_admet call, logS in
[-3, -2] inclusive AND bbb = 1):
- z002052  (logS -2.7894, bbb 1)
- z002040  (logS -2.3374, bbb 1)
- z002170  (logS -2.92,   bbb 1)

Budget 8/8 used; no further assays possible. Untested compounds cannot be
reported as qualifying (unconfirmed).

