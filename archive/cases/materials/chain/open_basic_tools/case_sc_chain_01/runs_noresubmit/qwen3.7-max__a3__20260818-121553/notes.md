# Notes - Superconductor Tc Study

## Budget tracking
- Total: 18 units, ALL SPENT (12 search + 6 measurements)
- Searches: Hg p1, Ca p1-5, Hg p2-7 = 12 pages
- Measurements: O6.080(92), O6.190(122), O6.210(127), O6.220(127), O6.240(123), O6.310(98) = 6 paid
- O6.160 returned "no sample" (unmeasurable)

## Seed material
- O6.080Ca1Cu2Ba2Hg1 (elements: O, Ca, Cu, Ba, Hg - 5 elements)

## Step A: Homologous series (5-element, varying O)
All compositions found: O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310
Measured Tc values:
- O6.080: 92 K (HIGH)
- O6.190: 122 K (HIGH) 
- O6.210: 127 K (HIGH) ← PEAK
- O6.220: 127 K (HIGH) ← PEAK
- O6.240: 123 K (HIGH)
- O6.310: 98 K (HIGH)
- O6.160: UNMEASURABLE (no sample)

PEAK: T* = 127 K at O6.220Ca1Cu2Ba2Hg1 (and O6.210 also 127)

## Step B: 4-element Hg family (O, Cu, Ba, Hg)
threshold1 = 127 - 32 = 95 K
Materials found (14 seen, ~15 total):
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
(All Cu1Ba2Hg1 with varying O)
CANNOT MEASURE - budget exhausted!

## Non-Hg materials from Ca search:
- O10Ca2Cu3Ba2Tl2 (5 elements: O,Ca,Cu,Ba,Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb)
- O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl)

## Confirmed UNMEASURABLE materials (no sample):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elements: O,Cu,Ba,Hg)
4. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O,Ca,Cu,Sr,Tl,Pb)
5. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O,Ca,Cu,Ba,Tl)

Materials with samples but can't measure (budget exhausted):
- O6.100Ca1Cu2Ba2Hg1, O6.130Ca1Cu2Ba2Hg1, O6.140Ca1Cu2Ba2Hg1, O6.300Ca1Cu2Ba2Hg1
- O10Ca2Cu3Ba2Tl2
- Most 4-element materials

"Unknown composition" errors: O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1 (appeared in search but not recognized)

## Step C: Predictions
Rule: Multi-factor rule based on Cu count (CuO2 planes) and key element presence.
- Materials with ≥2 Cu AND Ba → HIGH (all measured 5-elem series are HIGH, Tc 92-127 K)
- Materials with 1 Cu → LOW (single CuO2 plane, lower Tc dome)
- Materials with Sr replacing Ba and partial site occupancy → LOW

Predictions:
1. O6.160Ca1Cu2Ba2Hg1: HIGH (2Cu+Ba+Hg, dome interp ~115 K)
2. O6.270Ca1Cu2Ba2Hg1: HIGH (2Cu+Ba+Hg, dome interp ~110 K)
3. O4.180Cu1Ba2Hg1: LOW (1Cu, Hg-1201 over-doped side)
4. O7Ca1Cu2Sr2Tl0.500Pb0.500: LOW (no Ba, Sr substitution, partial Tl)
5. O8Ca0.850Cu2.200Ba2Tl1.800: HIGH (Ba + 2.2Cu, Tl-based high-Tc)

## Step D: Final pick
Predicted HIGH: O6.160(5elem), O6.270(5elem), O8...(5elem)
Fewest elements = 5 (all tied). Higher expected Tc: O6.160 (~115K) wins.
FINAL: O6.160Ca1Cu2Ba2Hg1

## Summary of approach and limitations
- Used 12/18 budget on searches (too many Hg pages), only 6 on measurements
- Identified the 5-element dome: peak at O6.220 with Tc=127 K
- Found 5 unmeasurable materials by testing all discovered compositions (free for no-sample)
- Could not measure any 4-element materials for Step B (all have samples but budget exhausted)
- Step B qualifying list is a prediction based on dome model for Hg-1201 system
- Two "unknown composition" errors: O4.066 and O4.160 appeared in search but not recognized by measure_tc
- Possible 15th 4-element material on unseen Hg pages 8-9 not identified
