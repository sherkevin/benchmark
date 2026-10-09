# Post hoc error decomposition

**Status: completed, pending full independent audit.** This descriptive analysis follows the card frozen after the aggregate results were seen. It retains every one of the 157 molecules, 124 scaffolds, 128 orders, 30 configurations, and both conditions. No new intervals, tests, provider inference, fitting, model updates, or sampling were performed.

## Contribution accounting

| Condition | Size stratum | Scaffolds | Molecules | Rescued | Harmed | Net correct | Pooled contribution (pp) | Equal-scaffold contribution (pp) |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| natural | singleton | 104 | 104 | 7234 | 5200 | 2034 | 0.33738057 | 0.42716734 |
| natural | more_than_one | 20 | 53 | 4858 | 8622 | -3764 | -0.62433652 | -0.21613883 |
| controlled_drift | singleton | 104 | 104 | 45418 | 6442 | 38976 | 6.46496815 | 8.18548387 |
| controlled_drift | more_than_one | 20 | 53 | 37612 | 9022 | 28590 | 4.74223726 | 2.52275146 |

Every molecule has 3,840 paired exposures. Net correctness equals rescued minus harmed at molecule, scaffold, stratum and condition levels. The two contribution sums reconcile with the frozen full-result aggregates within 1e-10 pp. `summary.json` preserves the numerical residuals.

## Largest recorded contributions

The complete sorted table is retained in `scaffolds.csv` and `scaffolds.json`; the rows below describe extrema of that table and do not define a selected cohort or a new score.

| Condition | Direction in full sorted table | Scaffold size | All molecule IDs | Net correct | Pooled contribution (pp) | Equal-scaffold contribution (pp) |
|---|---|---:|---|---:|---:|---:|
| natural | lowest contributions | 3 | confirm_056, confirm_057, confirm_064 | -1490 | -0.24714703 | -0.10430668 |
| natural | lowest contributions | 6 | confirm_024, confirm_043, confirm_044, confirm_045, confirm_046, confirm_047 | -1404 | -0.23288217 | -0.04914315 |
| natural | lowest contributions | 1 | confirm_026 | -1300 | -0.21563163 | -0.27301747 |
| natural | highest contributions | 1 | confirm_092 | 1086 | 0.18013535 | 0.22807460 |
| natural | highest contributions | 1 | confirm_013 | 1144 | 0.18975584 | 0.24025538 |
| natural | highest contributions | 1 | confirm_099 | 1222 | 0.20269374 | 0.25663642 |
| controlled_drift | lowest contributions | 1 | confirm_120 | -1362 | -0.22591561 | -0.28603831 |
| controlled_drift | lowest contributions | 6 | confirm_024, confirm_043, confirm_044, confirm_045, confirm_046, confirm_047 | -866 | -0.14364384 | -0.03031194 |
| controlled_drift | lowest contributions | 1 | confirm_151 | -774 | -0.12838376 | -0.16255040 |
| controlled_drift | highest contributions | 2 | confirm_031, confirm_032 | 2678 | 0.44420117 | 0.28120800 |
| controlled_drift | highest contributions | 3 | confirm_070, confirm_071, confirm_142 | 4100 | 0.68006900 | 0.28701837 |
| controlled_drift | highest contributions | 3 | confirm_009, confirm_012, confirm_156 | 4114 | 0.68239119 | 0.28799843 |

## Interpretation and reuse

The sign difference is an accounting consequence of assigning scaffold weight proportional to its molecule count in the pooled statistic versus equal scaffold weight in the second statistic. These tables locate the recorded positive and negative contributions under both weighting rules. They do not establish a chemical mechanism, calibration failure, or the cause of a learned ranking.

Keep the primary pooled result and its negative natural contrast visible. Retain the equal-scaffold statistic with its distinct definition. The reusable artifacts are the complete paired molecule counts, scaffold membership/contributions, fixed-stratum sums, and consumed-input hashes. They can support a later independently frozen study; they provide no permission to tune on, discard, or relabel these viewed molecules.

The full independent replay is still pending at this analysis run. Do not merge this diagnosis into the paper or label it verified until that audit closes. The broader scientific gate is unchanged.
