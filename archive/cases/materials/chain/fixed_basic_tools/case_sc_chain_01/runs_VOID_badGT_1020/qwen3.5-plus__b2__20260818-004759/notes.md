# Notes: Superconductor Tc Study

## Task Overview
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- High Tc definition: >= 90 K
- Measurement budget: 34 calls

## Step A — Series Peak
Goal: Find composition with MAXIMUM Tc among homologous series (same element set, varying oxygen)
- Need to measure series members in data/measurable.txt
- Find T* (maximum Tc)

## Step B — Filter Family B
- threshold1 = T* - 32
- Filter: materials that contain Hg AND have exactly 4 elements
- Report ALL with Tc >= threshold1 as Q1

## Step C — Extrapolate
- Materials in data/unmeasurable.txt have no physical sample
- Must induce a rule to separate high/low Tc
- Rule involves interaction of multiple factors (not single-factor)

## Step D — Final Pick
- Among predicted "high" unmeasurable materials, pick one with FEWEST elements
- Break ties by higher expected Tc

---

## Progress Log

### Step A: Finding T*

The seed is `O6.080Ca1Cu2Ba2Hg1` with elements O, Ca, Cu, Ba, Hg (5 elements).
The homologous series is `O6.xxxCa1Cu2Ba2Hg1`.

Measurements from the series:
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K
- O6.220Ca1Cu2Ba2Hg1: 127.0 K
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**T* = 127.0 K** (maximum, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)

For the peak composition, I'll use **O6.210Ca1Cu2Ba2Hg1** (first one with max Tc).

**threshold1 = T* - 32 = 127.0 - 32 = 95.0 K**

### Step B: Filter Family B

Criteria: contains Hg AND has exactly 4 elements, Tc >= 95.0 K

Materials with Hg and 4 elements: O4.xxxCu1Ba2Hg1 series

Q1 (Tc >= 95.0 K):
- O4.081Cu1Ba2Hg1: 98.0 K ✓
- O4.162Cu1Ba2Hg1: 97.4 K ✓
- O4.150Cu1Ba2Hg1: 97.0 K ✓
- O4.220Cu1Ba2Hg1: 97.0 K ✓
- O4.096Cu1Ba2Hg1: 95.6 K ✓
- O4.097Cu1Ba2Hg1: 95.3 K ✓

Excluded (Tc < 95.0 K):
- O4.200Cu1Ba2Hg1: 94.2 K
- O4.101Cu1Ba2Hg1: 93.4 K
- O4.103Cu1Ba2Hg1: 92.5 K
- O4.085Cu1Ba2Hg1: 92.0 K

No sample (can't verify):
- O4.180Cu1Ba2Hg1
- O4.047Cu1Ba2Hg1

**Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}**

### Step C: Extrapolate Rule

Induced rule: **HIGH if (contains Hg AND NOT contains Tl) OR (contains Ca AND contains Tl). Otherwise LOW.**

Equivalently: HIGH if the material contains Hg without Tl, or contains both Ca and Tl.

Predictions for unmeasurable.txt:
- O6.xxxCa1Cu2Ba2Hg1 (Hg=yes, Tl=no, Ca=yes): HIGH
- O4.xxxCu1Ba2Hg1 (Hg=yes, Tl=no, Ca=no): HIGH
- O6Cu1Ba2Hg1Tl1 (Hg=yes, Tl=yes, Ca=no): LOW
- O10Ca2Cu3Ba2Tl2 (Hg=no, Tl=yes, Ca=yes): HIGH
- O6Cu1Cd0.250Ba2Tl1.750 (Hg=no, Tl=yes, Ca=no): LOW
- O7Cu3Ba2La1 (Hg=no, Tl=no, Ca=no): LOW
- O7Cu3Y0.200Ba2La0.800 (Hg=no, Tl=no, Ca=no): LOW

### Step D: Final Pick

Among predicted HIGH, find the one with FEWEST elements.

Predicted HIGH with 4 elements: O4.xxxCu1Ba2Hg1 series
Predicted HIGH with 5 elements: O6.xxxCa1Cu2Ba2Hg1 series, O10Ca2Cu3Ba2Tl2

Minimum elements = 4. Among 4-element compositions, pick the one with highest expected Tc.

Based on measurements, O4.081Cu1Ba2Hg1 has the highest Tc (98.0 K).

**Final answer: O4.081Cu1Ba2Hg1**
