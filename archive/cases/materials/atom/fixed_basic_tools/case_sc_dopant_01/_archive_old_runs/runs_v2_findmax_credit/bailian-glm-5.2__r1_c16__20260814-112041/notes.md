# Notes — find highest-Tc composition under 16-credit budget

## Candidate structure analysis

30 candidates, all of form `O7Cu3Ba2Pr{p}{RE}{1-p}` = Ba2(Pr_p RE_{1-p})Cu3O7, a 123-type cuprate
where the rare-earth (Y) site is a Pr/RE mixture.

Two axes vary:
- **Dopant RE type**: Er, Gd, Nd (3 types)
- **Pr fraction p**: 0.030, 0.076/0.078/0.080, 0.150, 0.200, 0.300, 0.400, 0.500, 0.600, 0.700, 0.800, 0.900

Grid (Pr fraction × RE), with gaps (some cells absent):
- p=0.030 → Er, Gd, Nd (3)   ← lowest Pr (most RE-rich)
- p≈0.078 → Er0.924, Gd0.922, Nd0.920 (3)
- p=0.150 → Er, Gd, Nd (3)
- p=0.200 → Er, Gd, Nd (3)
- p=0.300 → Er, Gd, Nd (3)
- p=0.400 → Er, Gd (2)  [no Nd]
- p=0.500 → Er, Gd, Nd (3)
- p=0.600 → Gd, Nd (2)  [no Er]
- p=0.700 → Gd, Nd (2)  [no Er]
- p=0.800 → Er, Gd, Nd (3)
- p=0.900 → Er, Gd, Nd (3)   ← highest Pr (most Pr-rich)

## Physics prior (first-principles)

PrBa2Cu3O7 is the *only* non-superconducting REBCO: Pr 4f hybridizes with O(2p)/Cu(3d),
acting as a magnetic pair-breaker. Replacing Pr with a normal RE restores superconductivity.
**Suppression law (the case's stated real law): Tc decreases monotonically with Pr fraction p.**

So expected: **highest Tc at the lowest p (=0.030, RE-rich)**; lowest Tc at p=0.900 (Pr-rich).
Real REBCO Tcs: NdBCO≈96K, GdBCO≈93K, ErBCO≈92K → Nd likely best at the top, but must verify.

## Strategy (16 credits, 30 candidates)

Cannot measure all. Plan:
1. **Map the curve + compare REs at top** (6 cr): Nd across p (0.030, 0.150, 0.500, 0.900) to get
   the Tc-vs-Pr shape; plus Er0.970 & Gd0.970 to compare REs at the likely-maximum.
2. **Confirm monotonicity & find winner** (rest): based on round-1, sample the leading RE at
   intermediate p to rule out a dome, and double-check the runner-up RE near the top.

Goal: confirm max is at p=0.030, then pick the best of {Er0.970, Gd0.970, Nd0.970}.

## Measurements

### Round 1 (6 credits) — map trend + compare REs at top

| composition | Tc (K) | note |
|---|---|---|
| O7Cu3Ba2Pr0.030Nd0.970 | 85.1 | Nd lowest at top — surprising (expected ~96) |
| O7Cu3Ba2Pr0.030Er0.970 | **93.9** | current max |
| O7Cu3Ba2Pr0.030Gd0.970 | 93.1 | close 2nd |
| O7Cu3Ba2Pr0.150Nd0.850 | 40.7 | steep drop from p=0.03→0.15 |
| O7Cu3Ba2Pr0.500Nd0.500 | 0.0 | no superconductivity |
| O7Cu3Ba2Pr0.900Nd0.100 | 0.0 | no superconductivity (Pr-rich, as expected) |

**Findings:**
- Monotonic-decreasing trend confirmed for Nd: 85.1 → 40.7 → 0.0 → 0.0. Suppression law holds.
- At the top (p=0.030): **Er (93.9) > Gd (93.1) > Nd (85.1)**. Ordering is NOT the textbook
  REBCO order (Nd would usually lead). Must respect the *measured* values, not priors.
- Steep falloff between p=0.030 (85+) and p=0.150 (~41): the maximum is definitely at the
  lowest-p end. p≥0.500 already gives 0.0, so nothing at higher p can win.

### Decision for Round 2
Remaining: 10 credits. The max so far is Er0.970@p=0.030 = 93.9. Two questions:
1. Could an unmeasured RE at p=0.030 beat 93.9? All 3 REs measured at p=0.030 → no, top is settled
   among REs. Er0.970 is the leader at p=0.030.
2. Could a *slightly higher* p (still low) for Er or Gd beat 93.9? The curve drops fast (Nd lost
   44K going 0.030→0.150). But the very next point up is p≈0.076/0.078. Worth one probe each for
   Er & Gd at p≈0.078 to confirm the top really is at p=0.030 (rule out a small dome just above 0.030).
   Also probe Er & Gd at p=0.150 to bracket the falloff and confirm Er>Gd pattern holds mid-curve.

### Round 2 (4 credits) — confirm max sits at p=0.030, not slightly higher

| composition | Tc (K) | note |
|---|---|---|
| O7Cu3Ba2Pr0.076Er0.924 | 91.1 | < Er0.970's 93.9 ✓ max stays at p=0.030 |
| O7Cu3Ba2Pr0.078Gd0.922 | 84.0 | < Gd0.970's 93.1 ✓ |
| O7Cu3Ba2Pr0.150Er0.850 | 82.9 | Er keeps falling |
| O7Cu3Ba2Pr0.150Gd0.850 | 67.9 | Gd keeps falling |

**Findings:**
- Monotonic decrease now confirmed for **all three** REs across p=0.030→0.078→0.150:
  Er 93.9→91.1→82.9 ; Gd 93.1→84.0→67.9 ; Nd 85.1→40.7. No dome. Suppression law airtight.
- **Er is the dominant RE at every p measured** (Er>Gd at p=0.030, 0.078, 0.150). Er0.970 is
  the clear winner.
- No candidate with p<0.030 exists (0.030 is the list minimum), and all p>0.030 are lower.
  → **The winner is locked: O7Cu3Ba2Pr0.030Er0.970 = 93.9 K.**

### Round 3 (4 credits) — full Er trend across p (rule out any late dome)

| composition | Tc (K) |
|---|---|
| O7Cu3Ba2Pr0.200Er0.800 | 76.4 |
| O7Cu3Ba2Pr0.400Er0.600 | 45.8 |
| O7Cu3Ba2Pr0.500Er0.500 | 28.6 |
| O7Cu3Ba2Pr0.800Er0.200 | 0.0 |

**Er full curve (7 points, monotonic — no dome anywhere):**
p: 0.030→0.076→0.150→0.200→0.400→0.500→0.800
Tc: 93.9→91.1→82.9→76.4→45.8→28.6→0.0

## Conclusion

- The Pr-suppression law holds for all three REs: Tc decreases monotonically with Pr fraction p.
- The lowest p in the candidate list is 0.030 (most RE-rich), and at p=0.030 the three REs rank
  **Er (93.9) > Gd (93.1) > Nd (85.1)**. Er dominates Gd at *every* p measured (0.030, 0.078, 0.150).
- Since p=0.030 is the list minimum (nothing lower exists) and all higher-p Er points are strictly
  lower, the global maximum is **O7Cu3Ba2Pr0.030Er0.970 = 93.9 K**.
- Used 14/16 credits. The 2 unused credits would only re-confirm already-known lower values.

## All measurements (14)

| composition | Tc (K) |
|---|---|
| O7Cu3Ba2Pr0.030Er0.970 | 93.9 |
| O7Cu3Ba2Pr0.030Gd0.970 | 93.1 |
| O7Cu3Ba2Pr0.030Nd0.970 | 85.1 |
| O7Cu3Ba2Pr0.076Er0.924 | 91.1 |
| O7Cu3Ba2Pr0.078Gd0.922 | 84.0 |
| O7Cu3Ba2Pr0.150Er0.850 | 82.9 |
| O7Cu3Ba2Pr0.150Gd0.850 | 67.9 |
| O7Cu3Ba2Pr0.150Nd0.850 | 40.7 |
| O7Cu3Ba2Pr0.200Er0.800 | 76.4 |
| O7Cu3Ba2Pr0.400Er0.600 | 45.8 |
| O7Cu3Ba2Pr0.500Er0.500 | 28.6 |
| O7Cu3Ba2Pr0.500Nd0.500 | 0.0 |
| O7Cu3Ba2Pr0.800Er0.200 | 0.0 |
| O7Cu3Ba2Pr0.900Nd0.100 | 0.0 |

**Final answer: O7Cu3Ba2Pr0.030Er0.970 (93.9 K).**
