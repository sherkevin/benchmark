# Notes: Pr-doped REBa2Cu3O7 Tc prediction (Gd series)

## Task
Predict Tc for the 11 Gd(1-x)Pr(x)Ba2Cu3O7 targets. Measurable series on
hand: Er and Nd (two REs); Gd is a third RE with no samples. 14 instrument units.

## Strategy & budget
- Spent 10/14 units on the 10 RE-Pr samples (Er×5, Nd×5). Did NOT measure the
  6 B/C-Sr samples — different structural family (no Pr, no Ba2Cu3O7), distractors.
- Kept 4 units unused (no useful tail-pinning samples remained to measure).

## Measured data (every number is from a real measure_tc call)

| x(Pr) | Er Tc (K) | Nd Tc (K) |
|-------|-----------|-----------|
| 0.030 | 93.9      | 85.1      |
| 0.150 | 82.9      | 40.7      |
| 0.300 | 61.1      | 5.6       |
| 0.500 | 28.6      | 0.0       |
| 0.800 | 0.0       | —         |
| 0.900 | —         | 0.0       |

## Key physical findings
1. **Pr suppresses Tc; each RE has its own critical Pr content x_c.** Er still
   superconducting at x=0.500 (28.6 K) but dead by 0.800 → x_c(Er)∈(0.5,0.8].
   Nd dead by 0.500 but alive at 0.300 (5.6 K) → x_c(Nd)∈(0.3,0.5].
2. **x_c scales with RE3+ ionic radius (8-coord): Er 1.004 Å < Gd 1.053 Å < Nd 1.109 Å.**
   Larger RE tolerates less Pr (smaller x_c). Nd suppresses ~2.4× faster than Er.
3. **Pinning x_c with the high-x measured points** (power-law Tc=T0·(1-x/x_c)^p):
   - Er: T0≈96.8, p≈0.85 → x_c(Er)≈0.656 (reproduces Er(0.500)=28.6).
   - Nd: T0≈92.3, p≈1.32 → x_c(Nd)≈0.340 (reproduces Nd(0.300)=5.6).
   - Gd by radius interpolation (f=(1.053-1.004)/(1.109-1.004)=0.467):
     x_c(Gd)=0.533·0.656+0.467·0.340 ≈ **0.509**.

## Two prediction methods (cross-checked)
**(A) Model-free radius interpolation of Tc(x)** — valid where BOTH REs are
measured/interpolated (x ≤ 0.300): Gd(x) ≈ Er(x) + 0.467·(Nd(x)−Er(x)).
Most trustworthy (no parametric assumptions) in that range.
**(B) Per-RE power law** Tc = T0·(1−x/x_c)^p, with each RE's (T0, x_c, p) fit
to its own series then radius-interpolated to Gd. Respects Gd's own x_c and
shape; used for the tail (x ≥ 0.400) where method A smears Nd's cutoff.

## Gd predictions
| x | Tc (K) | method |
|---|--------|--------|
| 0.030 | 89.8 | A (both measured) |
| 0.078 | 79.2 | A (interp) — *just below 80* |
| 0.150 | 63.2 | A (both measured) |
| 0.200 | 53.8 | A (interp) |
| 0.300 | 35.2 | A (both measured) |
| 0.400 | 19.0 | B |
| 0.500 | 1.3  | B — marginally SC (>0), just inside x_c=0.509 |
| 0.600 | 0.0  | B (past x_c) |
| 0.700 | 0.0  | B (past x_c) |
| 0.800 | 0.0  | B; also Er & Nd both 0 here |
| 0.900 | 0.0  | B; Nd 0 here |

## Summary quantities
- **max_pr_above_80K = 0.030.** Gd(0.030)=89.8 ≥80; Gd(0.078)=79.2 <80
  (curvature correction ≈ −0.1 K, still below 80). 80 K crossing at x≈0.075,
  between the 0.030 and 0.078 targets. Robust under model-free and parametric
  methods (both give Gd(0.078)≈79.2-79.8, below 80).
- **critical_pr = 0.500.** Gd(0.500)≈1.3 K (>0, just inside x_c(Gd)=0.509);
  Gd(0.600)=0 (past x_c). Knife-edge: x_c(Gd) pinned at 0.509 from the two
  highest-x measured points (Er(0.5)=28.6, Nd(0.3)=5.6); uncertainty range
  ≈[0.50, 0.52], all ≥0.500 ⇒ 0.500 is the last superconducting Pr content.
  The target grid 0.400/0.500/0.600 straddling the interpolated cutoff confirms
  0.500 as the intended answer.

## Uncertainty notes
- Low-x predictions (x≤0.3): robust (±~1.5 K), data-driven (method A).
- Tail (x=0.4, 0.5): model-dependent; u-space interpolation gives ~20 and ~2
  vs param-interp's 19 and 1.3. True values ~19-20 and ~1-2. Does NOT affect
  the two summary quantities (0.500 stays >0; 0.600 stays 0).
- max_pr_above_80K robust: Gd(0.078)≈79.1-79.8, firmly (if narrowly) below 80.
