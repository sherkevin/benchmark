# Notes - Superconductor Tc Study

## Task Overview
- Seed: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measure_tc calls
- High Tc: >= 90 K, Low: < 90 K

## Step A - Series Peak
Seed homologous series (same element set O-Ca-Cu-Ba-Hg, varying oxygen):

| Composition | Tc (K) | High/Low |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | High |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | High |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | High |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | High |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | High |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | High ← MAX |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | High ← MAX |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | High |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | High |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | High |

T* = 127.0 K, Peak = O6.210Ca1Cu2Ba2Hg1

## Other Measurable Materials

| Composition | #Elem | Elements | Hg? | Ca? | Tl? | Tc | H/L |
|---|---|---|---|---|---|---|---|
| O4.081Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 98.0 | H |
| O4.162Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 97.4 | H |
| O4.150Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 97.0 | H |
| O4.220Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 97.0 | H |
| O4.096Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 95.6 | H |
| O4.097Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 95.3 | H |
| O4.047Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 95.0 | H |
| O4.200Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 94.2 | H |
| O4.101Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 93.4 | H |
| O4.103Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 92.5 | H |
| O4.085Cu1Ba2Hg1 | 4 | O,Cu,Ba,Hg | Y | N | N | 92.0 | H |
| O6Cu1Ba2Hg1Tl1 | 5 | O,Cu,Ba,Hg,Tl | Y | N | Y | 0.0 | L |
| O10Ca2Cu3Ba2Tl2 | 5 | O,Ca,Cu,Ba,Tl | N | Y | Y | 118.97 | H |
| O6Cu1Cd0.250Ba2Tl1.750 | 5 | O,Cu,Cd,Ba,Tl | N | N | Y | 88.0 | L |
| O7Cu3Ba2La1 | 4 | O,Cu,Ba,La | N | N | N | 89.6 | L |
| O7Cu3Y0.200Ba2La0.800 | 5 | O,Cu,Y,Ba,La | N | N | N | 81.0 | L |

## Step B
threshold1 = 127.0 - 32 = 95.0 K

Hg + 4 elements (O,Cu,Ba,Hg) materials with Tc >= 95.0:
- O4.081Cu1Ba2Hg1: 98.0 ✓
- O4.162Cu1Ba2Hg1: 97.4 ✓
- O4.150Cu1Ba2Hg1: 97.0 ✓
- O4.220Cu1Ba2Hg1: 97.0 ✓
- O4.096Cu1Ba2Hg1: 95.6 ✓
- O4.097Cu1Ba2Hg1: 95.3 ✓
- O4.047Cu1Ba2Hg1: 95.0 ✓

NOT qualifying:
- O4.200Cu1Ba2Hg1: 94.2
- O4.101Cu1Ba2Hg1: 93.4
- O4.103Cu1Ba2Hg1: 92.5
- O4.085Cu1Ba2Hg1: 92.0

Q1 = 7 materials

## Step C - Rule Induction

HIGH materials (22):
- All 10 O-Ca-Cu-Ba-Hg: contain Ca AND Hg
- All 11 O-Cu-Ba-Hg: contain Hg, no Ca, no Tl
- O10Ca2Cu3Ba2Tl2: contains Ca, no Hg, has Tl

LOW materials (4):
- O6Cu1Ba2Hg1Tl1: has Hg AND Tl, no Ca → Tc=0
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, no Ca → Tc=88
- O7Cu3Ba2La1: no Hg, no Ca → Tc=89.6
- O7Cu3Y0.200Ba2La0.800: no Hg, no Ca → Tc=81

Induced rule: HIGH if contains Ca OR (contains Hg AND does NOT contain Tl)
- Equivalently: LOW if (no Ca) AND (no Hg OR has Tl)

Verification:
- All Ca-containing → HIGH ✓ (11 materials)
- All Hg-only (no Tl) → HIGH ✓ (11 materials)
- O6Cu1Ba2Hg1Tl1: no Ca, has Hg+Tl → LOW ✓
- O6Cu1Cd0.250Ba2Tl1.750: no Ca, no Hg → LOW ✓
- O7Cu3Ba2La1: no Ca, no Hg → LOW ✓
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg → LOW ✓

100% accuracy on training set!

### Predictions for unmeasurable:
1. O6.160Ca1Cu2Ba2Hg1 (O,Ca,Cu,Ba,Hg): has Ca → HIGH
2. O6.270Ca1Cu2Ba2Hg1 (O,Ca,Cu,Ba,Hg): has Ca → HIGH
3. O4.180Cu1Ba2Hg1 (O,Cu,Ba,Hg): has Hg, no Tl → HIGH
4. O8Ca0.850Cu2.200Ba2Tl1.800 (O,Ca,Cu,Ba,Tl): has Ca → HIGH
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 (O,Ca,Cu,Sr,Tl,Pb): has Ca → HIGH
6. O8Cu3Y1Ba1.900La0.100 (O,Cu,Y,Ba,La): no Ca, no Hg → LOW
7. O8Cu4Y1Ba1.870La0.130 (O,Cu,Y,Ba,La): no Ca, no Hg → LOW

## Step D - Final Pick
HIGH predicted unmeasurables:
1. O6.160Ca1Cu2Ba2Hg1 - 5 elements
2. O6.270Ca1Cu2Ba2Hg1 - 5 elements
3. O4.180Cu1Ba2Hg1 - 4 elements ← FEWEST
4. O8Ca0.850Cu2.200Ba2Tl1.800 - 5 elements
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 - 6 elements

Final answer: O4.180Cu1Ba2Hg1 (4 elements, fewest among high-predicted)

Budget used: 26 / 34
