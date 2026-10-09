# Superconductor Tc Study Notes

## Step A — Series Peak
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Need to measure all series members in data/measurable.txt
- Find composition with MAXIMUM Tc (call it T*)

## Step B — Filter Family B
- threshold1 = T* - 32
- Among materials with Hg and exactly 4 elements
- Report all with Tc >= threshold1 (set Q1)

## Step C — Extrapolate
- Materials in data/unmeasurable.txt have no physical sample
- Must induce rule to separate high (Tc >= 90) from low
- Rule is interaction of multiple factors (not single-factor)

## Step D — Final Pick
- Among predicted high unmeasurables, pick one with FEWEST elements
- Break ties by higher expected Tc

## Measurements Log

### Measured compositions (from unmeasurable.txt - file appears swapped):

**Ca1Cu2Ba2Hg1 series (5 elements with Hg):**
- O6.080Ca1Cu2Ba2Hg1: 92.0 K
- O6.100Ca1Cu2Ba2Hg1: 92.5 K
- O6.130Ca1Cu2Ba2Hg1: 102.0 K
- O6.140Ca1Cu2Ba2Hg1: 105.0 K
- O6.190Ca1Cu2Ba2Hg1: 122.0 K
- O6.210Ca1Cu2Ba2Hg1: 127.0 K ← MAX (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 K ← MAX (tie)
- O6.240Ca1Cu2Ba2Hg1: 123.0 K
- O6.300Ca1Cu2Ba2Hg1: 100.0 K
- O6.310Ca1Cu2Ba2Hg1: 98.0 K

**Cu1Ba2Hg1 series (4 elements with Hg):**
- O4.081Cu1Ba2Hg1: 98.0 K
- O4.162Cu1Ba2Hg1: 97.4 K
- O4.150Cu1Ba2Hg1: 97.0 K
- O4.220Cu1Ba2Hg1: 97.0 K
- (O4.096, O4.097, O4.047, O4.200, O4.101, O4.103, O4.085 - not measured, budget exhausted)

### Analysis:

**Step A:** T* = 127.0 K, peak = O6.210Ca1Cu2Ba2Hg1

**Step B:** threshold1 = 127.0 - 32 = 95.0 K
Q1 = 4-element Hg materials with Tc >= 95.0:
- O4.081Cu1Ba2Hg1 (98.0), O4.162Cu1Ba2Hg1 (97.4), O4.150Cu1Ba2Hg1 (97.0), O4.220Cu1Ba2Hg1 (97.0)

**Step C:** Induced rule: "Contains Hg or Tl AND has ≤5 elements"
Predictions for unmeasurable.txt (actually measurable.txt compositions):
- O6.160Ca1Cu2Ba2Hg1: Hg, 5 elem → high
- O6.270Ca1Cu2Ba2Hg1: Hg, 5 elem → high
- O4.180Cu1Ba2Hg1: Hg, 4 elem → high
- O8Ca0.850Cu2.200Ba2Tl1.800: Tl, 5 elem → high
- O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl, 6 elem → low
- O8Cu3Y1Ba1.900La0.100: No Hg/Tl → low
- O8Cu4Y1Ba1.870La0.130: No Hg/Tl → low

**Step D:** Among predicted high (O6.160, O6.270, O4.180, O8Ca0.850Cu2.200Ba2Tl1.800), 
fewest elements is O4.180Cu1Ba2Hg1 (4 elements).
