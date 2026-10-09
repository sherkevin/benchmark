# Notes — QED prediction & lead selection

## Task summary
- Job 1: Predict QED of unmade `m05` (COc1ccc(cc1)C(=O)NCCCCCCCCCCC). Extrapolate from short-chain congeners (same scaffold, N-alkyl chain length differs).
- Job 2: Pick one lead with measured QED ≥ 0.6, psa ≥ 40, and NOT in m05's series.
- Budget: 17 measurements total, shared by list_pool pages and measure_qed.

## Plan
1. Use `describe_structure(m05)` to learn the scaffold + chain length of m05.
2. `list_pool` (page by page) to gather all candidates + descriptors. Cost = pages.
3. Use `describe_structure` (free) on each pool mol to find same-scaffold congeners of m05.
4. Measure congeners of m05, fit trend, extrapolate to m05's chain length → predict target QED.
5. Use free constraints (psa ≥ 40 and not-same-series) to pre-screen leads, then measure sparingly.
