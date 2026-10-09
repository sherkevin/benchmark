# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 18 measure_tc calls
- High Tc: >= 90K, Low: < 90K

## Step A: Series Peak
Seed's homologous series = {O, Ca, Cu, Ba, Hg} with varying oxygen.
Series members in measurable.txt (10 entries):
1. O6.080Ca1Cu2Ba2Hg1
2. O6.100Ca1Cu2Ba2Hg1
3. O6.130Ca1Cu2Ba2Hg1
4. O6.140Ca1Cu2Ba2Hg1
5. O6.190Ca1Cu2Ba2Hg1
6. O6.210Ca1Cu2Ba2Hg1
7. O6.220Ca1Cu2Ba2Hg1
8. O6.240Ca1Cu2Ba2Hg1
9. O6.300Ca1Cu2Ba2Hg1
10. O6.310Ca1Cu2Ba2Hg1

Other measurable materials (16 entries):
- 4-element Hg series {O, Cu, Ba, Hg}: 11 entries (items 11-21)
- O6Cu1Ba2Hg1Tl1 (5 elements)
- O10Ca2Cu3Ba2Tl2 (5 elements)
- O6Cu1Cd0.250Ba2Tl1.750 (5 elements)
- O7Cu3Ba2La1 (4 elements)
- O7Cu3Y0.200Ba2La0.800 (5 elements)

## Unmeasurable (7 entries):
1. O6.160Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
2. O6.270Ca1Cu2Ba2Hg1 (5 elem: O,Ca,Cu,Ba,Hg)
3. O4.180Cu1Ba2Hg1 (4 elem: O,Cu,Ba,Hg)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem: O,Ca,Cu,Ba,Tl)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem: O,Ca,Cu,Sr,Tl,Pb)
6. O8Cu3Y1Ba1.900La0.100 (5 elem: O,Cu,Y,Ba,La)
7. O8Cu4Y1Ba1.870La0.130 (5 elem: O,Cu,Y,Ba,La)

## Measurements
### Step A: Ca-series (10 calls) - COMPLETED
Results:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K (high)
- O6.100Ca1Cu2Ba2Hg1: 92.5 K (high)
- O6.130Ca1Cu2Ba2Hg1: 102.0 K (high)
- O6.140Ca1Cu2Ba2Hg1: 105.0 K (high)
- O6.190Ca1Cu2Ba2Hg1: 122.0 K (high)
- O6.210Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K (high) ← PEAK (tied)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K (high)
- O6.300Ca1Cu2Ba2Hg1: 100.0 K (high)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K (high)

**T* = 127.0 K** (peak composition: O6.210Ca1Cu2Ba2Hg1)
**threshold1 = 127.0 - 32 = 95.0 K**

### Step B: 4-element Hg series (8 calls) - COMPLETED
Measured results (threshold = 95.0 K):
- O4.047Cu1Ba2Hg1: 95.0 K (qualifies)
- O4.081Cu1Ba2Hg1: 98.0 K (qualifies)
- O4.097Cu1Ba2Hg1: 95.3 K (qualifies)
- O4.103Cu1Ba2Hg1: 92.5 K (does not qualify)
- O4.150Cu1Ba2Hg1: 97.0 K (qualifies)
- O4.162Cu1Ba2Hg1: 97.4 K (qualifies)
- O4.200Cu1Ba2Hg1: 94.2 K (does not qualify)
- O4.220Cu1Ba2Hg1: 97.0 K (qualifies)

Interpolated (not measured, budget exhausted):
- O4.085Cu1Ba2Hg1: ~97.3 K (between 4.081@98.0 and 4.097@95.3) → qualifies
- O4.096Cu1Ba2Hg1: ~95.5 K (between 4.081@98.0 and 4.097@95.3) → qualifies
- O4.101Cu1Ba2Hg1: ~93.4 K (between 4.097@95.3 and 4.103@92.5) → does not qualify

**Q1 (qualifying materials with Hg and 4 elements, Tc >= 95.0 K):**
1. O4.047Cu1Ba2Hg1 (95.0 K)
2. O4.081Cu1Ba2Hg1 (98.0 K)
3. O4.085Cu1Ba2Hg1 (~97.3 K, interpolated)
4. O4.096Cu1Ba2Hg1 (~95.5 K, interpolated)
5. O4.097Cu1Ba2Hg1 (95.3 K)
6. O4.150Cu1Ba2Hg1 (97.0 K)
7. O4.162Cu1Ba2Hg1 (97.4 K)
8. O4.220Cu1Ba2Hg1 (97.0 K)

### Step C: Rule Induction
All 18 measured materials are HIGH Tc (>= 90 K):
- Ca-series (5 elements: O,Ca,Cu,Ba,Hg): 92-127 K
- 4-element Hg (4 elements: O,Cu,Ba,Hg): 92.5-98.0 K

Key observation: All measured materials contain Hg. Materials without Hg were not measured.

Induced rule (interaction of multiple factors):
"High Tc if: (has 4 or 5 elements) AND (contains Hg OR contains Y)"

Rationale:
- Single factor "contains Hg" would be wrong for YBCO-type materials (#6, #7)
- Single factor "has 4-5 elements" would be wrong for Tl-based materials (#4)
- The interaction captures both Hg-based and Y-based cuprates

Predictions for unmeasurable materials:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, has Hg) → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, has Hg) → HIGH
3. O4.180Cu1Ba2Hg1 (4 elem, has Hg) → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg, no Y) → LOW
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg, no Y) → LOW
6. O8Cu3Y1Ba1.900La0.100 (5 elem, has Y) → HIGH
7. O8Cu4Y1Ba1.870La0.130 (5 elem, has Y) → HIGH

### Step D: Final Pick
Among HIGH predictions (#1, #2, #3, #6, #7), find fewest elements:
- #1: 5 elements
- #2: 5 elements
- #3: 4 elements ← FEWEST
- #6: 5 elements
- #7: 5 elements

**Final answer: O4.180Cu1Ba2Hg1**

## Budget Summary
- Step A: 10 calls (Ca-series)
- Step B: 8 calls (4-element Hg series)
- Total: 18/18 calls used
