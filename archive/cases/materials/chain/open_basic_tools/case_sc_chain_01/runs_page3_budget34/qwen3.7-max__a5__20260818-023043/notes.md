# Notes - Superconductor Tc Study

## Task Overview
- Budget: 34 units (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High Tc: >= 90 K, Low: < 90 K

## Step A: Find homologous series peak
- Seed: O6.080Ca1Cu2Ba2Hg1 (O-Ca-Cu-Ba-Hg system, varying oxygen)

## Step B: Filter family B (Hg, 4 elements, Tc >= T* - 32)

## Step C: Predict unmeasurable materials

## Step D: Final pick (fewest elements among predicted high, unmeasurable)

---

## Actions & Results

### Searches performed (budget: 14 units)
- Hg search: 9 pages, 27 materials found
- Ca search: 5 pages, 15 materials found

### Step A: Homologous Series (O-Ca-Cu-Ba-Hg, varying oxygen)
| Composition | Tc (K) | Sample? |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | yes |
| O6.160Ca1Cu2Ba2Hg1 | — | NO SAMPLE |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | yes ← PEAK |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | yes ← PEAK (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | yes |
| O6.270Ca1Cu2Ba2Hg1 | — | NO SAMPLE |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | yes |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1)

### Step B: 4-element Hg materials (O-Cu-Ba-Hg)
| Composition | Tc (K) | >= 95? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.066Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | — | NO SAMPLE |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

threshold1 = 127.0 - 32 = 95.0
**Q1 = 6 materials** (measured Tc >= 95)

### Step C: All unmeasurable materials found
1. O4.047Cu1Ba2Hg1 (4 elem, Hg, very low O=4.047)
2. O4.066Cu1Ba2Hg1 (4 elem, Hg, low O=4.066)
3. O4.160Cu1Ba2Hg1 (4 elem, Hg, mid O=4.160)
4. O4.180Cu1Ba2Hg1 (4 elem, Hg, mid O=4.180)
5. O6.160Ca1Cu2Ba2Hg1 (5 elem, Hg, mid O=6.160)
6. O6.270Ca1Cu2Ba2Hg1 (5 elem, Hg, mid O=6.270)
7. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, no Hg)
8. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, no Hg)

### Rule Induction
- All 20 measurable materials have Tc >= 90 K (all HIGH)
- Single-factor rules fail: "contains Hg = HIGH" wrong for O4.047/O4.066 (very low O); "high O = HIGH" wrong for non-Hg materials
- **Multi-factor rule**: HIGH if (contains Hg) AND (oxygen coefficient in formula is above the gap threshold ~4.08 for the 4-elem series, or more generally, oxygen is within the interpolable range of its series). LOW if no Hg, or if Hg present but oxygen is too far below the measurable range of its series.

### Predictions
| Composition | Prediction | Reasoning |
|---|---|---|
| O4.047Cu1Ba2Hg1 | LOW | Hg present but O=4.047 well below measurable range (4.081+) |
| O4.066Cu1Ba2Hg1 | LOW | Hg present but O=4.066 below measurable range |
| O4.160Cu1Ba2Hg1 | HIGH | Hg present, O=4.160 within measurable range (neighbors: 97.0, 97.4) |
| O4.180Cu1Ba2Hg1 | HIGH | Hg present, O=4.180 within measurable range |
| O6.160Ca1Cu2Ba2Hg1 | HIGH | Hg present, O=6.160 within series range |
| O6.270Ca1Cu2Ba2Hg1 | HIGH | Hg present, O=6.270 within series range |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | LOW | No Hg, 6 elements, different chemistry |
| O8Ca0.850Cu2.200Ba2Tl1.800 | LOW | No Hg, different chemistry |

### Step D: Final pick
HIGH unmeasurable: O4.160 (4 elem), O4.180 (4 elem), O6.160 (5 elem), O6.270 (5 elem)
Fewest elements = 4: tie between O4.160 and O4.180
Expected Tc: O4.160 ~97.2 (between 4.150@97.0 and 4.162@97.4); O4.180 ~95.8 (between 4.162@97.4 and 4.200@94.2)
**Final: O4.160Cu1Ba2Hg1** (higher expected Tc)
