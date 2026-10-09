# Superconductor Tc Study Notes

## Task overview
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg)
- High = Tc >= 90 K, Low = Tc < 90 K
- Each search page shows 3 results; 27 Hg materials total; 9 search pages used

## All Hg-containing materials found (27 total) — via search_db("Hg") pages 1-9
### Cu1Ba2Hg1 series (elements: O, Cu, Ba, Hg = 4 ELEMENTS) — Family B candidates
1. O4.047Cu1Ba2Hg1
2. O4.066Cu1Ba2Hg1
3. O4.081Cu1Ba2Hg1  → 98.0 (measured) HIGH
4. O4.085Cu1Ba2Hg1
5. O4.096Cu1Ba2Hg1  → 95.6 (measured) HIGH
6. O4.097Cu1Ba2Hg1  → 95.3 (measured) HIGH
7. O4.101Cu1Ba2Hg1
8. O4.103Cu1Ba2Hg1
9. O4.150Cu1Ba2Hg1  → 97.0 (measured) HIGH
10. O4.160Cu1Ba2Hg1  (error "unknown composition" on measure — no Tc)
11. O4.162Cu1Ba2Hg1
12. O4.180Cu1Ba2Hg1
13. O4.200Cu1Ba2Hg1
14. O4.220Cu1Ba2Hg1
### Ca1Cu2Ba2Hg1 series (elements: O, Ca, Cu, Ba, Hg = 5 ELEMENTS) — Step A series (varying O)
- O6.080Ca1Cu2Ba2Hg1 (seed) → 92.0 (measured)
- O6.100Ca1Cu2Ba2Hg1 (unmeasured)
- O6.130Ca1Cu2Ba2Hg1 (unmeasured)
- O6.140Ca1Cu2Ba2Hg1 (unmeasured)
- O6.160Ca1Cu2Ba2Hg1 → NO SAMPLE (unmeasurable, predicted)
- O6.190Ca1Cu2Ba2Hg1 → 122.0 (measured)
- O6.210Ca1Cu2Ba2Hg1 → 127.0 (measured) *** PEAK ***
- O6.220Ca1Cu2Ba2Hg1 → 127.0 (measured) (ties peak)
- O6.240Ca1Cu2Ba2Hg1 (unmeasured)
- O6.270Ca1Cu2Ba2Hg1 (unmeasured)
- O6.300Ca1Cu2Ba2Hg1 (unmeasured)
- O6.310Ca1Cu2Ba2Hg1 → 98.0 (measured)
### Tl series (5 elements: O,Cu,Ba,Hg,Tl) — excluded from B (5 elements)
- O6Cu1Ba2Hg1Tl1 (unmeasured)

## Step A — Series peak (Ca1Cu2Ba2Hg1, varying O)
Measured dome shape:
- O6.080 → 92.0  (low end)
- O6.160 → no sample (gap, predict)
- O6.190 → 122.0
- O6.210 → 127.0  ← PEAK (max measured)
- O6.220 → 127.0  (plateau, ties peak)
- O6.310 → 98.0  (falling)
**T* = 127.0**, peak composition = O6.210Ca1Cu2Ba2Hg1
(Maximum measured Tc is 127.0, shared by O6.210 and O6.220. Pick O6.210 as THE peak.)

## Step B — threshold1 = T* - 32 = 127 - 32 = 95.0
Family B = contains Hg AND exactly 4 elements = the 14 Cu1Ba2Hg1 materials.
Q1 = those with Tc >= 95.0.
Measured 4-element values: O4.081=98.0, O4.096=95.6, O4.097=95.3, O4.150=97.0 — all >= 95.0.
The 4-element Cu1Ba2Hg1 dome is narrow and flat-top around 95-98 K across O4.081 to O4.150.
All measured 4-element points are >= 95.0 (the peak region).
Likely Q1 (Tc>=95.0): the central measured ones + neighbors by dome continuity.

## Step C — Unmeasurable materials (no sample)
Known unmeasurable: O6.160Ca1Cu2Ba2Hg1.
(O4.160Cu1Ba2Hg1 gave "unknown composition" error — ambiguous, treat as not measurable / no sample.)
predictions must include ALL unmeasurable discovered via search_db.

## Budget tracking
- Used: 18 (9 search + 9 measure) — EXHAUSTED
- Note: "no sample" / "unknown composition" / "instrument exhausted" calls at the end
  all returned errors and did NOT increment budget (budget stayed 18). The only real
  extra measurement gained was O4.081=98.0.
