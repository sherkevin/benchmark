# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc >= 90 K, Low Tc < 90 K

## Budget Tracking
- Used: 34 / 34 (14 searches + 20 measurements)

---

## Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)

| Composition | Tc (K) | Sample? | High/Low |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes | high |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes | high |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes | high |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes | high |
| O6.160Ca1Cu2Ba2Hg1 | ??? | NO | - |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes | high |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes | high |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes | high |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes | high |
| O6.270Ca1Cu2Ba2Hg1 | ??? | NO | - |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes | high |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes | high |

**Peak T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (also O6.220 ties at 127.0)
Dome-shaped Tc vs oxygen content, peaking at O6.210-O6.220

---

## Step B: 4-element Hg materials (O-Cu-Ba-Hg), threshold1 = 127 - 32 = 95.0 K

| Composition | Tc (K) | Sample? | >= 95? |
|---|---|---|---|
| O4.047Cu1Ba2Hg1 | ??? | NO | - |
| O4.066Cu1Ba2Hg1 | ? | unmeasured | - |
| O4.081Cu1Ba2Hg1 | ? | unmeasured | - |
| O4.085Cu1Ba2Hg1 | ? | unmeasured | - |
| O4.096Cu1Ba2Hg1 | 95.6 | yes | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | yes | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | yes | no |
| O4.103Cu1Ba2Hg1 | 92.5 | yes | no |
| O4.150Cu1Ba2Hg1 | 97.0 | yes | YES |
| O4.160Cu1Ba2Hg1 | ??? | NO | - |
| O4.162Cu1Ba2Hg1 | 97.4 | yes | YES |
| O4.180Cu1Ba2Hg1 | ??? | NO | - |
| O4.200Cu1Ba2Hg1 | 94.2 | yes | no |
| O4.220Cu1Ba2Hg1 | 97.0 | yes | YES |

**Q1 (Tc >= 95):** O4.096, O4.097, O4.150, O4.162, O4.220

---

## Other materials found

| Composition | Elements | Tc | High/Low |
|---|---|---|---|
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl (5) | 118.97 | high |
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl (5) | 0.0 | LOW |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | O,Ca,Cu,Sr,Tl,Pb (6) | unmeasured | - |
| O8Ca0.850Cu2.200Ba2Tl1.800 | O,Ca,Cu,Ba,Tl (5) | unmeasured | - |

---

## Step C: Rule Induction

**Key observation:** O6Cu1Ba2Hg1Tl1 is the ONLY low material (Tc = 0). It contains BOTH Hg and Tl.
- All materials with Hg but no Tl → high
- All materials with Tl but no Hg → high (O10Ca2Cu3Ba2Tl2 = 118.97)
- Material with BOTH Hg AND Tl → low (O6Cu1Ba2Hg1Tl1 = 0.0)

**Rule: Materials containing both Hg and Tl are low (non-superconducting); all others are high (Tc >= 90 K).**

This is a multi-factor rule (requires checking two elements), consistent with the warning that single-factor rules fail.

### Confirmed unmeasurable materials (5):
1. O6.160Ca1Cu2Ba2Hg1 → HIGH (Hg, no Tl)
2. O6.270Ca1Cu2Ba2Hg1 → HIGH (Hg, no Tl)
3. O4.047Cu1Ba2Hg1 → HIGH (Hg, no Tl)
4. O4.160Cu1Ba2Hg1 → HIGH (Hg, no Tl)
5. O4.180Cu1Ba2Hg1 → HIGH (Hg, no Tl)

---

## Step D: Final Pick

Among unmeasurable predicted HIGH, fewest elements:
- O4.047Cu1Ba2Hg1 (4 elements) - expected Tc uncertain (low-O end)
- O4.160Cu1Ba2Hg1 (4 elements) - expected Tc ~97.2 (between O4.150=97.0 and O4.162=97.4)
- O4.180Cu1Ba2Hg1 (4 elements) - expected Tc ~96 (between O4.162=97.4 and O4.200=94.2)
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

Three tie at 4 elements. Tie-break by higher expected Tc:
**O4.160Cu1Ba2Hg1** (expected ~97.2 K) wins.
