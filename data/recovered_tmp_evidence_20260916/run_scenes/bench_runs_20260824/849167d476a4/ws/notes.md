# Notes: Drug-likeness optimization

## Parent compound diagnosis
SMILES: `c1ccc(cc1)-c1ccc(cc1)C(=O)NCCCCCCCC`
- QED = 0.6136 (poor)
- MW = 309.45 (fine)
- ALOGP = 5.444 (TOO HIGH — main problem)
- PSA = 29.1 (fine)
- ROTB = 9 (borderline high)
- AROM = 2 (fine)

**Root cause**: The octyl chain (CCCCCCCC) makes the molecule far too lipophilic (ALOGP 5.444). It also contributes heavily to rotatable bonds.

## Strategy
The productive direction is to shorten the alkyl chain, which simultaneously reduces ALOGP and ROTB. Adding polar groups at the terminus of the long chain was expected to help but actually trades one problem for another (PSA and ROTB go out of range).

## Candidates examined

### Passing free constraints (PSA ≤ 40, ROTB ≤ 9):
| ID | Edit | PSA | ROTB | ALOGP | QED measured |
|---|---|---|---|---|---|
| e04 | Shortened to butyl (NCCCC) | 29.1 | 5 | 3.884 | **0.8048** |
| e15 | Shortened to hexyl (NCCCCCC) | 29.1 | 7 | 4.664 | 0.7345 |
| e05 | 4-F-phenyl | 29.1 | 9 | 5.583 | not measured |
| e13 | Shortened to heptyl (NCCCCCCC) | 29.1 | 8 | 5.054 | not measured |

### Failing free constraints:
| ID | Edit | PSA | ROTB | Why failed |
|---|---|---|---|---|
| e01 | Terminal amide | 72.19 | 10 | PSA too high, ROTB too high |
| e02 | Terminal acid | 66.4 | 10 | PSA too high, ROTB too high |
| e03 | Nitro on ring | 72.24 | 10 | PSA too high, ROTB too high |
| e06 | Extended to nonyl | 29.1 | 10 | ROTB too high, ALOGP worse |
| e07 | Terminal amine | 55.12 | 10 | PSA too high, ROTB too high |
| e10 | Extended to decyl | 29.1 | 11 | ROTB too high, ALOGP much worse |
| e11 | Pyridine ring | 41.99 | 9 | PSA slightly too high |
| e12 | Terminal dimethylamine | 32.34 | 11 | ROTB too high |

## Key findings
1. **Best QED**: e04 with QED = 0.8048 (shortening to butyl chain)
2. **Optimised lead pick**: e04 — passes all three constraints (QED 0.8048 ≥ 0.8, PSA 29.1 ≤ 40, ROTB 5 ≤ 9)
3. **Failed direction**: Extending the alkyl chain (e06, e10) made ALOGP and ROTB worse. Adding terminal polar groups (e01, e02, e03, e07, e12) reduced ALOGP but pushed PSA and ROTB out of range. The clear lesson: the chain needed to be shortened, not decorated.

## Budget usage
- 7 search_pool calls (7 units)
- 2 measure_qed calls (2 units: e04, e15)
- 2 empty search results still counted (NC(=O), NCCO, NCCN)
- Total: 11/11 budget used
