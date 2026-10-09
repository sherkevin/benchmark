# Notes: Superconductor Tc Study

## Seed material: O6.080Ca1Cu2Ba2Hg1
## Budget: 34 measure_tc calls (used 25, no-sample calls don't count)
## Definition: high = Tc >= 90 K, low = Tc < 90 K

---

## Step A: Series Peak (Seed series O-Ca-Cu-Ba-Hg, varying oxygen)

| Composition | Tc (K) | High/Low |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | high |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | - |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | high |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | high |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | high |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | high |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | - |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | high |

**PEAK: O6.210Ca1Cu2Ba2Hg1 with T* = 127.0 K** (tied with O6.220)

---

## Step B: Filter family B (contains Hg, exactly 4 elements)
threshold1 = 127.0 - 32 = 95.0

4-element Hg measurable compositions (O-Cu-Ba-Hg):
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | 93.4 | NO |
| O4.103Cu1Ba2Hg1 | 92.5 | NO |
| O4.085Cu1Ba2Hg1 | 92.0 | NO |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | - |
| O4.047Cu1Ba2Hg1 | NO SAMPLE | - |

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

---

## Other measured compositions

| Composition | Elements | Tc (K) | High/Low |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl (5) | 0.0 | low |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl (5) | 118.97 | high |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl (5) | 88.0 | low |
| O7Cu3Ba2La1 | O,Cu,Ba,La (4) | 89.6 | low |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La (5) | 81.0 | low |

---

## Step C: Rule Induction

### Analysis of HIGH vs LOW:

**HIGH materials (Tc >= 90 K):**
- All O-Ca-Cu-Ba-Hg (5 elements): contain Ca AND Hg, no Tl
- All O-Cu-Ba-Hg (4 elements): contain Hg, no Tl, no Ca
- O10Ca2Cu3Ba2Tl2 (5 elements): contains Ca, no Hg

**LOW materials (Tc < 90 K):**
- O6Cu1Ba2Hg1Tl1: has Hg but ALSO has Tl → Tc = 0
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, no Ca → Tc = 88
- O7Cu3Ba2La1: no Hg, no Ca → Tc = 89.6
- O7Cu3Y0.200Ba2La0.800: no Hg, no Ca → Tc = 81

### INDUCED RULE: **HIGH if (contains Hg AND does NOT contain Tl) OR contains Ca**

Verification against all 24 measured materials: ✓ perfect classification

### Predictions for unmeasurable materials:

| Composition | Elements | Rule applies | Prediction |
|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | O,Ca,Cu,Ba,Hg (5) | Has Ca ✓, Hg no Tl ✓ | **high** |
| O6.270Ca1Cu2Ba2Hg1 | O,Ca,Cu,Ba,Hg (5) | Has Ca ✓, Hg no Tl ✓ | **high** |
| O4.180Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | Hg no Tl ✓ | **high** |
| O4.047Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | Hg no Tl ✓ | **high** |
| O4.066Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | Hg no Tl ✓ | **high** |
| O4.160Cu1Ba2Hg1 | O,Cu,Ba,Hg (4) | Hg no Tl ✓ | **high** |
| O8Ca0.850Cu2.200Ba2Tl1.800 | O,Ca,Cu,Ba,Tl (5) | Has Ca ✓ | **high** |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | O,Ca,Cu,Sr,Tl,Pb (6) | Has Ca ✓ | **high** |
| O8Cu3Y1Ba1.900La0.100 | O,Cu,Y,Ba,La (5) | No Hg, no Ca | **low** |
| O8Cu4Y1Ba1.870La0.130 | O,Cu,Y,Ba,La (5) | No Hg, no Ca | **low** |

---

## Step D: Final pick

Predicted HIGH unmeasurable, sorted by element count:
- 4 elements: O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1, O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1
- 5 elements: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

Fewest = 4 elements. Tie-break by expected Tc:
- O4.066Cu1Ba2Hg1: nearest measured = O4.081 (98.0 K), dist=0.015 → expected ~97-98 K
- O4.160Cu1Ba2Hg1: nearest measured = O4.162 (97.4 K), dist=0.002 → expected ~97.4 K
- O4.180Cu1Ba2Hg1: between O4.162 (97.4) and O4.200 (94.2) → expected ~96 K
- O4.047Cu1Ba2Hg1: extrapolating below O4.081 (98.0), dist=0.034 → expected ~93-95 K

**FINAL: O4.066Cu1Ba2Hg1** (4 elements, highest expected Tc ~97-98 K among ties)
