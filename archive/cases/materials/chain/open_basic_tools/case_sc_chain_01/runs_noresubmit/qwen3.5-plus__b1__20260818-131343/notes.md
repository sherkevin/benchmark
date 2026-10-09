# Superconductor Tc Study Notes - Final

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Budget: 18 units total - EXHAUSTED
- Used: 9 for search_db (9 pages of Hg search, 27 total results)
- Used: 9 for measure_tc (including 2 failed attempts on unmeasurable materials)
- Remaining: 0

---

## Step A — Series peak (homologous series: O, Ca, Cu, Ba, Hg)

Measured compositions and Tc values:
| Composition | Tc (K) | Sample Available |
|-------------|--------|------------------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | - | No (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | - | No (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | ? | Not measured (budget exhausted) |

**Peak Tc (T*) = 127.0 K** (at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (first to reach peak)

**threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

---

## Step B — Filter family B (contains Hg, exactly 4 elements, Tc >= 95.0)

4-element Hg materials (O, Cu, Ba, Hg series) - NOT MEASURED (no budget):
- O4.047Cu1Ba2Hg1
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

**qualifying**: UNKNOWN - no budget to measure. Assuming empty set.

---

## Step C — Unmeasurable materials found and predictions

Unmeasurable materials (no sample exists):
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)

**Prediction rule**: Tc in the O-Ca-Cu-Ba-Hg series follows a bell curve with oxygen content:
- Rises from ~92K at O6.08 to peak 127K at O6.21-6.22
- Falls to ~100K at O6.30
- All compositions in range O6.08-O6.31 have Tc >= 90K (HIGH)

**Predictions**:
- O6.160Ca1Cu2Ba2Hg1: HIGH (interpolated ~112K, between 105K and 122K)
- O6.270Ca1Cu2Ba2Hg1: HIGH (interpolated ~112K, between 123K and 100K)

---

## Step D — Final pick

Among unmeasurable predicted HIGH:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~112K
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~112K

Both have 5 elements (tie). Tie-break by higher expected Tc.
- O6.160: interpolated ~111.8K
- O6.270: interpolated ~111.5K

**Final answer**: O6.160Ca1Cu2Ba2Hg1 (slightly higher expected Tc)

---

## Final Answer Summary

```json
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": [],
  "predictions": {
    "O6.160Ca1Cu2Ba2Hg1": "high",
    "O6.270Ca1Cu2Ba2Hg1": "high"
  },
  "final": "O6.160Ca1Cu2Ba2Hg1",
  "rule": "Tc follows bell curve with oxygen content in O-Ca-Cu-Ba-Hg series, peaking at 127K. All series members have Tc >= 90K (high)."
}
```

**Note**: Step B qualifying set is empty because budget was exhausted before measuring 4-element Hg materials. This may be incorrect if some 4-element materials have Tc >= 95K.
