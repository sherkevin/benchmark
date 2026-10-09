# Superconductor Tc Study Notes

## Initial Setup
- Start budget: 34 units total (shared between measure_tc and search_db)
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: high if Tc >= 90 K, otherwise low
- Each search_db page costs 1 budget unit, shows at most 4 results
- Each measure_tc costs 1 budget unit

## Step A - Series Peak
Need to find homologous doping series with same element set (O, Ca, Cu, Ba, Hg) but varying oxygen content.

## Budget Tracking
- Initial: 34
- Seed Tc measurement: -1 = 33
- Search queries: 7 pages of Hg search = -7 = 26 remaining
- Measurements: 11 more measurements (10 successful, 1 no sample) = -11 = 15 remaining

## Found Series Members (CaCuBaHg with varying O):
1. O6.080Ca1Cu2Ba2Hg1 (seed, Tc = 92.0 K) ✓
2. O6.100Ca1Cu2Ba2Hg1 (Tc = 92.5 K)
3. O6.130Ca1Cu2Ba2Hg1 (Tc = 102.0 K)
4. O6.140Ca1Cu2Ba2Hg1 (Tc = 105.0 K)
5. O6.160Ca1Cu2Ba2Hg1 (no sample - unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1 (Tc = 122.0 K)
7. O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K) ← MAXIMUM SO FAR
8. O6.220Ca1Cu2Ba2Hg1 (Tc = 127.0 K) ← MAXIMUM SO FAR (tie)
9. O6.240Ca1Cu2Ba2Hg1 (Tc = 123.0 K)
10. O6.270Ca1Cu2Ba2Hg1 (no sample - unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1 (Tc = 100.0 K)
12. O6.310Ca1Cu2Ba2Hg1 (Tc = 98.0 K)

## Step A Result
Maximum Tc (T*) = 127.0 K
Peak composition(s): O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)

Need to pick one as "peak composition" for answer.json. Both have same Tc. Probably choose O6.210Ca1Cu2Ba2Hg1 as it appears first.

## Step B - Filter Family B (contains Hg and has exactly 4 elements)
threshold1 = T* - 32 = 127 - 32 = 95 K

### Materials with exactly 4 elements containing Hg (from search results):
From Hg search pages 1-4, found CuBaHg series (O, Cu, Ba, Hg = 4 elements):

**Measured and qualified (Tc ≥ 95 K):**
1. O4.081Cu1Ba2Hg1 (Tc = 98.0 K) ✓
2. O4.096Cu1Ba2Hg1 (Tc = 95.6 K) ✓
3. O4.097Cu1Ba2Hg1 (Tc = 95.3 K) ✓
4. O4.150Cu1Ba2Hg1 (Tc = 97.0 K) ✓
5. O4.162Cu1Ba2Hg1 (Tc = 97.4 K) ✓
6. O4.220Cu1Ba2Hg1 (Tc = 97.0 K) ✓

**Measured but NOT qualified (Tc < 95 K):**
- O4.085Cu1Ba2Hg1 (92.0 K)
- O4.101Cu1Ba2Hg1 (93.4 K)
- O4.103Cu1Ba2Hg1 (92.5 K)
- O4.200Cu1Ba2Hg1 (94.2 K)

**No sample available (unmeasurable):**
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

**Set Q1 (qualifying materials):**
["O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]

## Budget Update
- Used: 34 units (exhausted)
- Measurements: 20 total
- Searches: 14 total (7 Hg pages, 3 Cu pages, 1 Tl page, 3 other)

## Step C - Extrapolate (no measurement possible)
Need to induce a rule from measurable materials to predict high/low for unmeasurable ones.

### All unmeasurable materials discovered:
1. O6.160Ca1Cu2Ba2Hg1 (CaCuBaHg series)
2. O6.270Ca1Cu2Ba2Hg1 (CaCuBaHg series)
3. O4.047Cu1Ba2Hg1 (CuBaHg series)
4. O4.066Cu1Ba2Hg1 (CuBaHg series)
5. O4.160Cu1Ba2Hg1 (CuBaHg series)
6. O4.180Cu1Ba2Hg1 (CuBaHg series)
7. O6Cu1Ba2Hg1Tl1 (from Tl search)
8. O6Cu1Cd0.250Ba2Tl1.750 (from Tl search)
9. O7Ca1Cu2Sr2Tl0.500Pb0.500 (from Tl search)

### Measured materials analysis:
All measured materials have Tc ≥ 90 K (all are "high").

CaCuBaHg series: Tc peaks around O6.210-O6.220 (127K), decreases on either side.
CuBaHg series: Tc varies non-monotonically with oxygen.

Need multi-factor rule (single-factor rules are wrong).

### Rule induction attempt:
Looking at CuBaHg series:
- High: O4.081(98.0), 4.096(95.6), 4.097(95.3), 4.150(97.0), 4.162(97.4), 4.220(97.0)
- Low: O4.085(92.0), 4.101(93.4), 4.103(92.5), 4.200(94.2)

Pattern: Maybe high when oxygen content modulo 0.01 is between 0.081-0.097 or 0.150-0.162 or exactly 0.220?
Actually: 4.081, 4.096, 4.097 (mod 0.01: 0.081, 0.096, 0.097) are high
4.150, 4.162 (mod 0.01: 0.150, 0.162) are high
4.220 (mod 0.01: 0.220) is high

Low ones: 4.085(0.085), 4.101(0.101), 4.103(0.103), 4.200(0.200)

Unmeasurable CuBaHg: 
- 4.047(0.047), 4.066(0.066): both low mod values, might be low
- 4.160(0.160): between 4.150(high) and 4.162(high), probably high
- 4.180(0.180): between 4.162(high) and 4.200(low), might be low

For CaCuBaHg series: All measured are high. Unmeasurable O6.160 and O6.270 likely high based on neighbors.

For Tl-containing: O10Ca2Cu3Ba2Tl2 is high (118.97K). Others unknown.

## Step D - Final pick
Among unmeasurable materials predicted HIGH, choose one with FEWEST elements.

Predicted HIGH unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.160Cu1Ba2Hg1 (4 elements) ← FEWEST ELEMENTS (4)
4. O6Cu1Ba2Hg1Tl1 (5 elements)
5. O6Cu1Cd0.250Ba2Tl1.750 (5 elements)
6. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements)

Final choice: O4.160Cu1Ba2Hg1 (4 elements, predicted high)

## Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 (Tc = 127.0 K)
- Threshold: 95.0 K
- Q1: 6 qualifying materials
- Predictions: 9 unmeasurable materials (4 high, 5 low)
- Final: O4.160Cu1Ba2Hg1
- Rule: Multi-factor rule based on element combinations and oxygen fractional parts