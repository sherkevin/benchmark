# Notes - Superconductor Tc Study

## Budget: 18 units total (shared between measure_tc and search_db)

## Seed: O6.080Ca1Cu2Ba2Hg1
- Elements: O, Ca, Cu, Ba, Hg (5 elements)
- Homologous series: same element set, varying oxygen

## Step A: Find series peak (max Tc)
Need to find all members of the Hg-Ba-Ca-Cu-O series with varying oxygen.

## Step B: Filter family B (Hg + exactly 4 elements)
threshold1 = T* - 32

## Step C: Predict unmeasurable materials

## Step D: Final pick (fewest elements among predicted high, unmeasurable)

---

## Actions log:

### Step A - Series Peak
Homologous series: O{x}Ca1Cu2Ba2Hg1 (5 elements: O, Ca, Cu, Ba, Hg)

Measurements:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← PEAK
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: UNMEASURABLE (no sample)
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127 K at O6.220Ca1Cu2Ba2Hg1**

### Step B - Filter Family B (Hg + 4 elements)
threshold1 = 127 - 32 = 95 K

4-element Hg materials found (O-Cu-Ba-Hg):
Page 1: O4.047, O4.066, O4.081
Page 2: O4.085, O4.096, O4.097
Page 3: O4.101 (Tc=93.4), O4.103, O4.150 (Tc=97.0)
Page 4: O4.160, O4.162, O4.180

Measured:
- O4.101Cu1Ba2Hg1: 93.4 K (below 95)
- O4.150Cu1Ba2Hg1: 97.0 K (above 95) ✓

Trend: Tc increases with oxygen content in 4-element series.

### Step C - Unmeasurable Materials
Found: O6.270Ca1Cu2Ba2Hg1 (no sample exists)
Prediction: HIGH (based on trend - would be ~105-115 K between O6.240=123 and O6.310=98)

### Step D - Final Pick
O6.270Ca1Cu2Ba2Hg1 (5 elements, predicted high)

---

## Final Answer Summary

**Step A**: Peak at O6.220Ca1Cu2Ba2Hg1 with T* = 127.0 K
- Tied with O6.210Ca1Cu2Ba2Hg1 (also 127.0 K)
- Series shows bell curve: rises from 92K at O6.080, peaks at 127K around O6.21-6.22, drops to 98K at O6.310

**Step B**: threshold1 = 95 K
- Q1 = [O4.150Cu1Ba2Hg1] (Tc=97.0 K)
- O4.101Cu1Ba2Hg1 (Tc=93.4 K) is below threshold
- Other 4-element materials not measured due to budget constraints

**Step C**: Unmeasurable materials
- O6.270Ca1Cu2Ba2Hg1: no sample exists
- Predicted as "high" based on trend (would be ~105-115 K)

**Step D**: Final pick
- O6.270Ca1Cu2Ba2Hg1 (5 elements, predicted high)

## Limitations
- Budget exhausted before measuring all 4-element Hg materials
- Only discovered 1 unmeasurable material (may be more in unsearched pages)
- Rule is primarily based on oxygen content (single-factor), which task warns may be insufficient

