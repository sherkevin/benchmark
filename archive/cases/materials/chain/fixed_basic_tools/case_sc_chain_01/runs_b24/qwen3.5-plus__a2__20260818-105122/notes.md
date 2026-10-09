# Superconductor Tc Study - Notes

## Step A: Find peak Tc in the homologous series

Seed material: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg)
The homologous series has the same element set with varying oxygen.

### Measurements from the series (O*Ca1Cu2Ba2Hg1):
| Composition | Tc (K) |
|-------------|--------|
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

**Peak Tc (T*) = 127.0 K** at `O6.210Ca1Cu2Ba2Hg1`

### Other measurements:
| Composition | Tc (K) | Elements | Has Hg | Has Ca | Has Tl |
|-------------|--------|----------|--------|--------|--------|
| O4.081Cu1Ba2Hg1 | 98.0 | O,Cu,Ba,Hg | Y | N | N |
| O4.162Cu1Ba2Hg1 | 97.4 | O,Cu,Ba,Hg | Y | N | N |
| O4.150Cu1Ba2Hg1 | 97.0 | O,Cu,Ba,Hg | Y | N | N |
| O4.220Cu1Ba2Hg1 | 97.0 | O,Cu,Ba,Hg | Y | N | N |
| O4.096Cu1Ba2Hg1 | 95.6 | O,Cu,Ba,Hg | Y | N | N |
| O4.097Cu1Ba2Hg1 | 95.3 | O,Cu,Ba,Hg | Y | N | N |
| O4.047Cu1Ba2Hg1 | 95.0 | O,Cu,Ba,Hg | Y | N | N |
| O4.200Cu1Ba2Hg1 | 94.2 | O,Cu,Ba,Hg | Y | N | N |
| O4.101Cu1Ba2Hg1 | 93.4 | O,Cu,Ba,Hg | Y | N | N |
| O4.103Cu1Ba2Hg1 | 92.5 | O,Cu,Ba,Hg | Y | N | N |
| O4.085Cu1Ba2Hg1 | 92.0 | O,Cu,Ba,Hg | Y | N | N |
| O6Cu1Ba2Hg1Tl1 | 0.0 | O,Cu,Ba,Hg,Tl | Y | N | Y |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O,Ca,Cu,Ba,Tl | N | Y | Y |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | O,Cu,Cd,Ba,Tl | N | N | Y |

## Step B: Filter family B (contains Hg, exactly 4 elements, Tc >= threshold1)

threshold1 = T* - 32 = 127.0 - 32 = 95.0

Materials with Hg, exactly 4 elements, Tc >= 95.0 (Q1):
- O4.081Cu1Ba2Hg1 (98.0) ✓
- O4.162Cu1Ba2Hg1 (97.4) ✓
- O4.150Cu1Ba2Hg1 (97.0) ✓
- O4.220Cu1Ba2Hg1 (97.0) ✓
- O4.096Cu1Ba2Hg1 (95.6) ✓
- O4.097Cu1Ba2Hg1 (95.3) ✓
- O4.047Cu1Ba2Hg1 (95.0) ✓

## Step C: Induce rule for high/low prediction

**Rule: High if (contains Hg AND NOT contains Tl) OR (contains Ca AND contains Tl)**

Verification against all measured data:
- Hg=Y, Tl=N -> High (all O*Ca1Cu2Ba2Hg1 and O*Cu1Ba2Hg1) ✓
- Hg=Y, Tl=Y, Ca=N -> Low (O6Cu1Ba2Hg1Tl1: 0.0) ✓
- Hg=N, Tl=Y, Ca=Y -> High (O10Ca2Cu3Ba2Tl2: 118.97) ✓
- Hg=N, Tl=Y, Ca=N -> Low (O6Cu1Cd0.250Ba2Tl1.750: 88.0) ✓
- Hg=N, Tl=N, Ca=N -> Low (O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800) ✓

## Step D: Final pick

Among unmeasurable materials predicted **high**, find the one with FEWEST elements.
Tie-break by higher expected Tc.

Predicted high materials by element count:
- 4 elements: O4.081Cu1Ba2Hg1 (98.0), O4.162Cu1Ba2Hg1 (97.4), O4.150Cu1Ba2Hg1 (97.0), O4.220Cu1Ba2Hg1 (97.0), O4.096Cu1Ba2Hg1 (95.6), O4.097Cu1Ba2Hg1 (95.3), O4.047Cu1Ba2Hg1 (95.0), O4.200Cu1Ba2Hg1 (94.2), O4.101Cu1Ba2Hg1 (93.4), O4.103Cu1Ba2Hg1 (92.5), O4.085Cu1Ba2Hg1 (92.0)
- 5 elements: All O*Ca1Cu2Ba2Hg1 series, O10Ca2Cu3Ba2Tl2

Fewest elements = 4. Among 4-element predicted-high materials, highest Tc is O4.081Cu1Ba2Hg1 (98.0 K).

**Final answer: O4.081Cu1Ba2Hg1**
