# Superconductor Tc Study Notes

## Budget: 45 units total (used all 45)

## Step A — Find series peak (homologous doping series with same elements as seed)

Seed: `O6.080Ca1Cu2Ba2Hg1`
Elements: O, Ca, Cu, Ba, Hg (5 elements)

### Series members and Tc measurements:
| Composition | Tc |
|-------------|-----|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | no sample |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | no sample |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**Peak Tc (T*) = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (and tied at O6.220Ca1Cu2Ba2Hg1)

**Step A Answer: peak = O6.210Ca1Cu2Ba2Hg1, peak_tc = 127.0**

---

## Step B — Filter family B (contains Hg, exactly 4 elements)

threshold1 = T* - 32 = 127.0 - 32 = **95.0**

4-element Hg-containing materials (O4.xxxCu1Ba2Hg1 series) and their Tc:
| Composition | Tc | >= 95.0? |
|-------------|-----|----------|
| O4.047Cu1Ba2Hg1 | no sample | - |
| O4.066Cu1Ba2Hg1 | no sample | - |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | no sample | - |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | no sample | - |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

**Q1 (measurable materials with Tc >= 95.0):**
["O4.081Cu1Ba2Hg1", "O4.096Cu1Ba2Hg1", "O4.097Cu1Ba2Hg1", "O4.150Cu1Ba2Hg1", "O4.162Cu1Ba2Hg1", "O4.220Cu1Ba2Hg1"]

---

## Step C — Extrapolate for unmeasurable materials

**All unmeasurable materials found:**
1. O4.047Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
2. O4.066Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
3. O4.160Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
4. O4.180Cu1Ba2Hg1 (4 elements: O, Cu, Ba, Hg)
5. O6.160Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
6. O6.270Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elements: O, Ca, Cu, Sr, Tl, Pb)
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elements: O, Ca, Cu, Ba, Tl)
9. O8Cu3Y1Ba1.900La0.100 (5 elements: O, Cu, Y, Ba, La)
10. O8Cu4Y1Ba1.870La0.130 (5 elements: O, Cu, Y, Ba, La)

**Induced Rule:** 
A material is HIGH (Tc >= 90) if it does NOT contain La, Y, Cd, or the combination of both Hg and Tl together. Otherwise it is LOW.

Equivalently: HIGH if no (La OR Y OR Cd OR (Hg AND Tl))

**Verification against measurable materials:**
- All O4.xxxCu1Ba2Hg1 and O6.xxxCa1Cu2Ba2Hg1: No La/Y/Cd, Hg without Tl → HIGH ✓
- O10Ca2Cu3Ba2Tl2: No La/Y/Cd, Tl without Hg → HIGH (118.97) ✓
- O6Cu1Ba2Hg1Tl1: Has both Hg AND Tl → LOW (0.0) ✓
- O6Cu1Cd0.250Ba2Tl1.750: Has Cd → LOW (88.0) ✓
- O7Cu3Ba2La1: Has La → LOW (89.6) ✓
- O7Cu3Y0.200Ba2La0.800: Has Y and La → LOW (81.0) ✓

**Predictions for unmeasurable materials:**
| Composition | Contains | Prediction |
|-------------|----------|------------|
| O4.047Cu1Ba2Hg1 | Hg only | HIGH |
| O4.066Cu1Ba2Hg1 | Hg only | HIGH |
| O4.160Cu1Ba2Hg1 | Hg only | HIGH |
| O4.180Cu1Ba2Hg1 | Hg only | HIGH |
| O6.160Ca1Cu2Ba2Hg1 | Hg only | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | Hg only | HIGH |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | Tl only (no Hg) | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | Tl only (no Hg) | HIGH |
| O8Cu3Y1Ba1.900La0.100 | Y and La | LOW |
| O8Cu4Y1Ba1.870La0.130 | Y and La | LOW |

---

## Step D — Final pick

Among unmeasurable materials predicted HIGH, choose the one with FEWEST elements.

HIGH predictions with element counts:
- O4.047Cu1Ba2Hg1: 4 elements
- O4.066Cu1Ba2Hg1: 4 elements
- O4.160Cu1Ba2Hg1: 4 elements
- O4.180Cu1Ba2Hg1: 4 elements
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O7Ca1Cu2Sr2Tl0.500Pb0.500: 6 elements
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

Minimum element count = 4. Four materials tie:
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1

Tie-breaker: higher expected Tc.

Looking at O4.xxxCu1Ba2Hg1 series measured values:
- 081: 98.0 (peak)
- 162: 97.4 (second peak)

For unmeasurable:
- O4.066: Near 081 (98.0), expected high
- O4.160: Between 150 (97.0) and 162 (97.4), expected ~97.3

O4.066 is closest to the highest peak (98.0 at 081), so it should have the highest expected Tc.

**Final Answer: O4.066Cu1Ba2Hg1**

Wait - let me reconsider. O4.047 is further from 081 than O4.066 is. O4.066 is closer to the 98.0 peak.

Actually, looking at the distances:
- O4.047 to O4.081: 0.034 oxygen difference
- O4.066 to O4.081: 0.015 oxygen difference

O4.066 is closer to the 98.0 peak, so it should have higher expected Tc than O4.047.

For O4.160 vs O4.066:
- O4.066 near 98.0 peak
- O4.160 near 97.4 peak

98.0 > 97.4, so O4.066 should have higher expected Tc.

**Final Answer: O4.066Cu1Ba2Hg1**
