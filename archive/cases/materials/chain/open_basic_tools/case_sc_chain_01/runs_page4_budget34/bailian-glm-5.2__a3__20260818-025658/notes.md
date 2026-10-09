# Superconductor Tc Study — Notes

## Task overview
- Budget: 34 units total (shared between measure_tc and search_db, 1 unit each)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg = 5 elements)
- High = Tc >= 90 K, Low = Tc < 90 K
- Steps A (series peak), B (filter Hg + exactly 4 elements, Tc >= T*-32), C (extrapolate unmeasurable), D (final pick)

## Budget tracking
Start: 34
- search "Hg" p1 → 4 results (all 4-elem: O4.047/O4.066/O4.081/O4.085 Cu1Ba2Hg1), 27 total Hg matches, has_more
- search "Ca1Cu2Ba2Hg1" p1,2,3 → 12 series members (5-elem, varying O): O6.080,0.100,0.130,0.140,0.160,0.190,0.210,0.220,0.240,0.270,0.300,0.310
Spent: 4 (4 search pages). Remaining: 30.

## Series members (Step A) — all 12, 5 elements each (O,Ca,Cu,Ba,Hg)
O6.080(seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310

## Hg materials page 1 (4-elem, O/Cu/Ba/Hg):
O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.085Cu1Ba2Hg1

## ALL 27 Hg materials (complete DB enumeration, pages 1-7):
4-element (O,Cu,Ba,Hg) — 14 total:
  O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
  O4.150, O4.160, O4.162, O4.180, O4.200, O4.220  (all Cu1Ba2Hg1)
5-element (O,Ca,Cu,Ba,Hg) — 12: the Step A series (O6.080..0.310 Ca1Cu2Ba2Hg1)
5-element (O,Cu,Ba,Hg,Tl) — 1: O6Cu1Ba2Hg1Tl1

Step B universe = exactly-4-element Hg materials = the 14 O4.xxx Cu1Ba2Hg1.
(Search "Hg" caught all; none are missing-O 4-elem. All contain O.)

## KEY RULE: unmeasurable (null) materials do NOT consume budget!

## Step A series measurements (Tc):
O6.080=92.0, O6.100=92.5, O6.130=102.0, O6.140=105.0,
O6.160=NULL(unmeas), O6.190=122.0, O6.210=127.0★, O6.220=127.0★,
O6.240=123.0, O6.270=NULL(unmeas), O6.300=100.0, O6.310=98.0

Peak T* = 127.0 at O6.210 (tie with O6.220). Pick O6.210Ca1Cu2Ba2Hg1.
threshold1 = 127 - 32 = 95.0

## Step B 4-element Hg measurements (Cu1Ba2Hg1, threshold1=95.0):
O4.047=NULL(unmeas), O4.066=NULL(unmeas), O4.081=98.0✓, O4.085=92.0✗,
O4.096=95.6✓, O4.097=95.3✓, O4.101=93.4✗, O4.103=92.5✗,
O4.150=97.0✓, O4.160=NULL(unmeas), O4.162=97.4✓, O4.180=NULL(unmeas),
O4.200=94.2✗, O4.220=97.0✓
Also measured O6Cu1Ba2Hg1Tl1 (5-elem, Tl) = 0.0 (LOW; heavily overdoped, O-per-Cu=6).

Measured-qualifying (Tc≥95): {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}
Predicted-qualifying unmeas: O4.160 (~97.3), O4.180 (~95.9) both ≥95.
=> Q1 (8): O4.081, O4.096, O4.097, O4.150, O4.160, O4.162, O4.180, O4.220  (Cu1Ba2Hg1)

## Unmeasurable materials discovered (6, all Hg-family):
4-elem: O4.047, O4.066, O4.160, O4.180  (Cu1Ba2Hg1)
5-elem: O6.160, O6.270  (Ca1Cu2Ba2Hg1)

## RULE (2-factor, dome-based):
HIGH iff it is a Hg-based cuprate whose oxygen content lies WITHIN the observed
superconducting dome for its homologous family — i.e. not below the family's
low-O edge (heavily underdoped → non-SC) and not overdoped far above the dome
(like the Tl variant O6Cu1Ba2Hg1Tl1, O-per-Cu=6, Tc=0).
- 4-elem dome measured high from O4.081→O4.220. O4.047, O4.066 sit BELOW O4.081
  (lowest measured high-Tc O) => underdoped, outside dome => LOW.
- O4.160 (btwn 4.150=97 & 4.162=97.4 ~97.3), O4.180 (btwn 4.162=97.4 & 4.200=94.2 ~95.9)
  inside dome => HIGH.
- 5-elem series dome measured high O6.080→O6.310 (peak 127 @6.21). O6.160 (btwn 105&122 ~114),
  O6.270 (btwn 123&100 ~111) inside dome => HIGH.
(Single-factor "contains Hg→high" is wrong on O4.047 & O4.066; "element count→high"
wrong on Tl=0.)

## Predictions (6 unmeasurable):
O4.047Cu1Ba2Hg1 -> low
O4.066Cu1Ba2Hg1 -> low
O4.160Cu1Ba2Hg1 -> high
O4.180Cu1Ba2Hg1 -> high
O6.160Ca1Cu2Ba2Hg1 -> high
O6.270Ca1Cu2Ba2Hg1 -> high

## Step D — final pick:
Predicted-HIGH unmeas: {O4.160, O4.180, O6.160, O6.270}. Fewest elements = 4 (the
4-elem ones {O4.160, O4.180}). Tie-break higher expected Tc: O4.160 ~97.3 > O4.180 ~95.9.
FINAL = O4.160Cu1Ba2Hg1

## Budget final: 31 used (10 search + 21 measure; nulls free). 3 unused.
Searches kept to Hg family (27 mats) to avoid predicting blind on new families.
