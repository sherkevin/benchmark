# Notes: highest-Tc composition under a measurement budget

## Setup
- 30 candidates, all of form `O7Cu3Ba2Pr{x}R{1-x}`, R ∈ {Er, Gd, Nd}, x = Pr fraction.
- Budget: 8 measurement credits. Cannot measure all 30.
- Framework = Ba-Cu-O cuprate (123-type), RE site split between Pr and a rare earth.

## Physical hypothesis (prior)
- These are 123-type cuprate superconductors (like YBCO / REBCO).
- Pr is famous for **suppressing** superconductivity in the RE-123 lattice (PrBCO is non-superconducting).
- Most other rare earths (Y, Nd, Gd, Er) give high Tc (~90 K).
- ⇒ Expect Tc **decreases monotonically as Pr fraction x increases**.
- ⇒ Maximum Tc likely at the **smallest Pr fraction x=0.030**, for whichever R (Er/Gd/Nd) is best.

## Candidate layout (Pr fraction x per rare earth)
- x=0.030 (lowest Pr): Er, Gd, Nd  ← prime max suspects
- mid x (0.15–0.50): all three
- x=0.900 (highest Pr): Er, Gd, Nd  ← prime min suspects / trend confirmation

## Measurement plan (8 credits)
1. Measure the three x=0.030 candidates (3 credits) — directly compares R at the likely-optimal Pr.
2. Measure 1–2 high-x points to confirm the decreasing trend (Pr suppresses Tc).
3. Spend remaining credits refining / confirming the argmax.

## Measurements

### Round 1 — three x=0.030 candidates (lowest Pr = prime max suspects)
- O7Cu3Ba2Pr0.030Er0.970 → **93.9 K**  ← highest so far
- O7Cu3Ba2Pr0.030Gd0.970 → 93.1 K
- O7Cu3Ba2Pr0.030Nd0.970 → 85.1 K

Observations:
- Hypothesis confirmed at low Pr: Tc is high (~90 K), as expected for REBCO-type.
- Rare-earth ranking at x=0.030: **Er (93.9) > Gd (93.1) > Nd (85.1)**.
- Er is the best rare earth. Gap Er→Gd is small (0.8 K); Er→Nd is large (8.8 K).

### Plan for remaining 5 credits
- Confirm Pr suppresses Tc: measure Er at higher x (e.g. 0.50, 0.900).
- Check whether a slightly higher x could beat 93.9 (unusual, but Pr could tune carriers). Test Er at one intermediate x.
- Keep ≥1 credit to lock the argmax.

### Round 2 — trace the Er curve (confirm Pr suppresses Tc, rule out a dome)
- O7Cu3Ba2Pr0.150Er0.850 → 82.9 K
- O7Cu3Ba2Pr0.300Er0.700 → 61.1 K
- O7Cu3Ba2Pr0.500Er0.500 → 28.6 K

Er curve is **monotonically decreasing** in x. Pr strongly suppresses Tc (hole
filling / magnetic pair breaking), exactly as expected. No dome — the peak sits
at the lowest available Pr.

### Round 3 — confirm no rising crest just above x=0.030, and Gd parallels Er
- O7Cu3Ba2Pr0.076Er0.924 → 91.1 K  (between 0.030=93.9 and 0.076=91.1, already dropping — no bump)
- O7Cu3Ba2Pr0.078Gd0.922 → 84.0 K  (Gd also drops from its 93.1 peak; parallels Er)

## Verdict

Global maximum = **O7Cu3Ba2Pr0.030Er0.970 = 93.9 K**.

Why I'm confident this is the argmax (not just the best measured):
1. Er is the best rare earth at the optimal (lowest) Pr: 93.9 > Gd 93.1 > Nd 85.1.
2. The Er curve is monotone-decreasing in x across 4 points (0.030→0.076→0.150→0.300→0.500):
   93.9 → 91.1 → 82.9 → 61.1 → 28.6. Peak is at the lowest x; no dome.
3. Gd's curve also decreases from its x=0.030 peak (93.1 → 84.0), so no Gd point can beat Er@0.030.
4. Nd peaks at only 85.1 (< 93.9) at x=0.030 and only falls from there.
5. The lowest-Pr point (x=0.030) is an endpoint of the candidate grid, and all evidence says
   the Tc-vs-x slope is negative there — so the boundary endpoint is the max, not an interior point.

All 8 credits used. Every cited Tc is from a real measure_tc call.
