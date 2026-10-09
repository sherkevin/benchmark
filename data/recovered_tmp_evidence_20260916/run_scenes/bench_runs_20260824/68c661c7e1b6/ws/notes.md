# Notes

## Initial Observations
- The task involves screening 20 compounds from `data/candidates.tsv` for two specific ADMET properties: `logS` (aqueous solubility) between -3 and -2, and `bbb` (blood-brain-barrier penetration) equal to 1.
- We have a budget of 8 assays (calls to `measure_admet`).
- The compounds vary in molecular weight (`mw`), calculated logP (`clogp`), and other descriptors.

## Strategy
1. **Prioritize Compounds**: Focus on compounds with descriptors that suggest favorable solubility and BBB penetration:
   - `clogp` between 1 and 3 (moderate lipophilicity).
   - `tpsa` (Topological Polar Surface Area) below 80 Å² (better BBB penetration).
   - `hbd` (Hydrogen Bond Donors) ≤ 2 (better solubility).

2. **Test the Top Candidates**: Use the budget to assay the most promising candidates first.

3. **Record Results**: Track the results in `notes.md` and update `answer.json` with qualifying IDs.

## Assay Results
1. **z000364**: `logS` = -0.9482, `bbb` = 0 → **Does not qualify**.
2. **z001999**: `logS` = -1.522, `bbb` = 1 → **Does not qualify** (logS outside range).
3. **z002010**: `logS` = -5.0097, `bbb` = 1 → **Does not qualify** (logS outside range).
4. **z002040**: `logS` = -2.3374, `bbb` = 1 → **Qualifies**.
5. **z002052**: `logS` = -2.7894, `bbb` = 1 → **Qualifies**.
6. **z002170**: `logS` = -2.92, `bbb` = 1 → **Qualifies**.
7. **z002216**: `logS` = -2.16, `bbb` = 1 → **Qualifies**.
8. **z002730**: `logS` = -2.6758, `bbb` = 1 → **Qualifies**.

## Summary
- **Qualifying Compounds**: `z002040`, `z002052`, `z002170`, `z002216`, `z002730`.
- **Budget Used**: 8 assays (full budget).