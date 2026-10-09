# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Budget: 34 units total (shared between measure_tc and search_db)

---

## Step A — Find series peak (homologous series with same elements, varying oxygen)

The seed has elements: O, Ca, Cu, Ba, Hg
Found 12 series members with same elements but varying oxygen.

### Measurements for Step A:
| Composition | Tc (K) |
|-------------|--------|
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

**Peak (T*) = 127.0 K** at `O6.210Ca1Cu2Ba2Hg1` (and O6.220Ca1Cu2Ba2Hg1)

---

## Step B — Filter family B (contains Hg, exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

### 4-element Hg materials (O-Cu-Ba-Hg family):
| Composition | Tc (K) | >= 95? |
|-------------|--------|--------|
| O4.047Cu1Ba2Hg1 | no sample | - |
| O4.066Cu1Ba2Hg1 | no sample | - |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.160Cu1Ba2Hg1 | no sample | - |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | no sample | - |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.220Cu1Ba2Hg1 | not measured | - |

**Q1 (qualifying):** O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1

---

## Step C — Extrapolate unmeasurable materials

### Unmeasurable materials found:
| Composition | Elements | Predicted | Reasoning |
|-------------|----------|-----------|-----------|
| O4.047Cu1Ba2Hg1 | 4 | low | Extreme low oxygen, far from optimal |
| O4.066Cu1Ba2Hg1 | 4 | high | Near 4.081 peak region |
| O4.160Cu1Ba2Hg1 | 4 | high | At 4.16 peak region (~97.3 expected) |
| O4.180Cu1Ba2Hg1 | 4 | high | Between peak and decline (~95.9 expected) |
| O6.160Ca1Cu2Ba2Hg1 | 5 | high | In rising region (~112 expected) |
| O6.270Ca1Cu2Ba2Hg1 | 5 | high | In falling region but still high (~111 expected) |

**Rule induced:** Tc follows a dome-shaped relationship with oxygen content; materials near family-specific optimal oxygen levels are high, while underdoped or overdoped materials are low.

---

## Step D — Final pick

Among predicted high unmeasurables:
- 4-element: O4.066, O4.160, O4.180
- 5-element: O6.160, O6.270

**Fewest elements:** 4 (tie among O4.066, O4.160, O4.180)

**Highest expected Tc among 4-element:**
- O4.066: ~95.6
- O4.160: ~97.3 (highest)
- O4.180: ~95.9

**Final answer: O4.160Cu1Ba2Hg1**

---

## Budget tracking:
- search_db calls: 14 (Hg pages 1-9, Ca pages 1-5)
- measure_tc calls: 20
- Total used: 34 / 34 (exhausted)

---

## Final answer.json written
