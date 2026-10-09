# Histogram floating-point contract repair

The first full independent audit preserved all 128 successful mathematical
stream checks, then failed its final exact histogram count comparison. The
failed run remains in `../independent_full_v1/`; no production record changed.

The independent checker originally used `100 * (count / denominator)`, while
the archived percent scalars used `100 * count / denominator`. The two values
can differ below 1e-14 but an automatically chosen histogram edge can move.
Likewise, mathematically equivalent weighted summation of scaffold effects can
differ from the mean of sorted scaffold effects at the final bits.

Read-only diagnosis found 35 differing secondary histograms among 168. Every
saved summary reproduces from its saved per-order scalars. Every non-histogram
count summary, and every primary statistic including exact integer signs and
its histogram, passes. The maximum per-order scalar difference is
1.4210854715202004e-14.

Minimal regression: counts `[4118, 4152, 4254]`, denominator 4710. Divide-first
changes the minimum from 87.43099787685775 to 87.43099787685776, shifting edge 3
from 88.15286624203821 to 88.15286624203823. The unchanged middle scalar then
falls in bin 2 instead of bin 3. Full edges and both histograms are preserved
in `../independent_tests_v2/verification.json`.

The repaired checker independently rebuilds all integer counts and uses this
explicit representation contract: pooled percents multiply the integer by
100 before division; scaffold effects divide independent integer differences
by each scaffold's observation count, average the sorted scaffold terms,
then multiply by 100. No saved metric supplies the independent calculation.
Scores, actions, labels, release/state checks, all thresholds, and exact
integer comparisons are unchanged. No histogram count tolerance was added.

All four targeted checks pass, including malformed release, terminal-label
mutation, integer wins versus float noise, and the concrete histogram
counterexample. The second full audit is a fresh 128-order mathematical
replay in `../independent_full_v2/`, not reuse of previously computed states.
Its final result must be read there; this repair note is not a pass claim.
