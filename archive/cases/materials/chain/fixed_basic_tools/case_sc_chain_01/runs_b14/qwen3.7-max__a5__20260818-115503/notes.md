# Notes: Superconductor Tc Study

## Task Overview
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 14 measure_tc calls
- 4 steps: A (series peak), B (filter family B), C (extrapolate), D (final pick)

## Data Files
### Measurable (26 materials):
- O6.xxxCa1Cu2Ba2Hg1 series (10 materials, 5 elements: O, Ca, Cu, Ba, Hg) — seed's series
- O4.xxxCu1Ba2Hg1 series (11 materials, 4 elements: O, Cu, Ba, Hg) — Family B (Hg + 4 elements)
- O6Cu1Ba2Hg1Tl1 (5 elements)
- O10Ca2Cu3Ba2Tl2 (5 elements)
- O6Cu1Cd0.250Ba2Tl1.750 (5 elements)
- O7Cu3Ba2La1 (4 elements)
- O7Cu3Y0.200Ba2La0.800 (5 elements)

### Unmeasurable (7 materials):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements, same series as seed)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements, same series as seed)
3. O4.180Cu1Ba2Hg1 (4 elements, same O4 series)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (6 elements)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements)
6. O8Cu3Y1Ba1.900La0.100 (5 elements)
7. O8Cu4Y1Ba1.870La0.130 (5 elements)

## Step A — Series Peak
Measuring all 10 members of the O6.xxxCa1Cu2Ba2Hg1 series.

## Measurements

### Step A: O6 series (10 measurements used, 4 remaining)
| Composition | Tc (K) |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**Peak: O6.210Ca1Cu2Ba2Hg1 (or O6.220) with T* = 127.0 K**
Tied at 127.0 K — will use O6.210Ca1Cu2Ba2Hg1 as the first peak.

**threshold1 = 127.0 - 32 = 95.0 K**

### Step B: Family B measurements (Hg + exactly 4 elements = O4.xxxCu1Ba2Hg1)
| Composition | Tc (K) | >= 95.0? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | 95.0 | YES ✓ |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |

Only 2 of 11 O4 materials measured (budget constraints). Trend suggests Tc decreases with increasing O in O4 series, so only O4.047 qualifies.

### Diverse measurements for rule induction
| Composition | Elements | #El | Hg | Tc | High/Low |
|---|---|---|---|---|---|
| O7Cu3Ba2La1 | O,Cu,Ba,La | 4 | no | 89.6 | LOW |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl | 5 | no | 118.97 | HIGH |

### Step C: Rule Induction
**All measured data summary:**
- O6 series (5 el, Hg): ALL HIGH (92-127 K)
- O4 series (4 el, Hg): HIGH (93.4-95.0 K)
- O7Cu3Ba2La1 (4 el, no Hg): LOW (89.6 K)
- O10Ca2Cu3Ba2Tl2 (5 el, no Hg): HIGH (118.97 K)

**Induced rule: "High if contains Hg OR has >= 5 elements"**
- Single factor "contains Hg" fails: O10Ca2Cu3Ba2Tl2 has no Hg but is high
- Single factor "has >= 5 elements" fails: O4 series has 4 elements but is high
- Combined rule correctly classifies all 14 measured materials

**Predictions for unmeasurable:**
1. O6.160Ca1Cu2Ba2Hg1 (5 el, Hg) → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (5 el, Hg) → HIGH
3. O4.180Cu1Ba2Hg1 (4 el, Hg) → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5 el, no Hg) → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 el, no Hg) → HIGH
6. O8Cu3Y1Ba1.900La0.100 (5 el, no Hg) → HIGH
7. O8Cu4Y1Ba1.870La0.130 (5 el, no Hg) → HIGH

### Step D: Final Pick
All 7 predicted high. Fewest elements: O4.180Cu1Ba2Hg1 (4 elements).
No tie to break.

**Final answer: O4.180Cu1Ba2Hg1**
