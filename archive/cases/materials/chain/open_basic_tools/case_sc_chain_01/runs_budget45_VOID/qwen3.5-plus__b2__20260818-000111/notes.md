# Superconductor Tc Study Notes

## Budget: 45 units total (shared between measure_tc and search_db)

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1`

## Step A — Find series peak
The seed belongs to a homologous doping series (same element set, varying oxygen).
Elements in seed: O, Ca, Cu, Ba, Hg (5 elements)
Need to find compositions with same elements but varying oxygen levels.

### Plan:
- Search for materials with these elements
- Measure Tc for each oxygen variation
- Find maximum Tc (T*)

## Step B — Filter family B
- threshold1 = T* - 32
- Find materials containing Hg with exactly 4 elements
- Report those with Tc >= threshold1 (set Q1)

## Step C — Extrapolate
- Find unmeasurable materials (no sample exists)
- Induce a rule from measurable materials
- Predict high/low for unmeasurable ones

## Step D — Final pick
- Among unmeasurable materials predicted "high"
- Choose one with FEWEST elements
- Break ties by higher expected Tc

---

## Progress Log

### Step A: Series Peak - COMPLETE
- Seed: O6.080Ca1Cu2Ba2Hg1
- Found 12 materials in homologous series (same elements: O, Ca, Cu, Ba, Hg)
- Measured Tc values:
  - O6.080Ca1Cu2Ba2Hg1: 92.0 K
  - O6.100Ca1Cu2Ba2Hg1: 92.5 K
  - O6.130Ca1Cu2Ba2Hg1: 102.0 K
  - O6.140Ca1Cu2Ba2Hg1: 105.0 K
  - O6.160Ca1Cu2Ba2Hg1: no sample (unmeasurable)
  - O6.190Ca1Cu2Ba2Hg1: 122.0 K
  - O6.210Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
  - O6.220Ca1Cu2Ba2Hg1: 127.0 K (PEAK)
  - O6.240Ca1Cu2Ba2Hg1: 123.0 K
  - O6.270Ca1Cu2Ba2Hg1: no sample (unmeasurable)
  - O6.300Ca1Cu2Ba2Hg1: 100.0 K
  - O6.310Ca1Cu2Ba2Hg1: 98.0 K

- **T* = 127.0 K** (peak)
- **threshold1 = T* - 32 = 95.0 K**

### Step B: Filter family B (Hg, exactly 4 elements, Tc >= 95.0)
- Found 14 materials with exactly 4 elements (O, Cu, Ba, Hg):
  - O4.047Cu1Ba2Hg1 (unmeasurable - no sample)
  - O4.066Cu1Ba2Hg1 (unmeasurable - no sample)
  - O4.081Cu1Ba2Hg1
  - O4.085Cu1Ba2Hg1
  - O4.096Cu1Ba2Hg1
  - O4.097Cu1Ba2Hg1
  - O4.101Cu1Ba2Hg1
  - O4.103Cu1Ba2Hg1
  - O4.150Cu1Ba2Hg1
  - O4.160Cu1Ba2Hg1 (unmeasurable - no sample)
  - O4.162Cu1Ba2Hg1
  - O4.180Cu1Ba2Hg1 (unmeasurable - no sample)
  - O4.200Cu1Ba2Hg1
  - O4.220Cu1Ba2Hg1

- Could not measure Tc for most due to instrument time exhaustion
- Need to determine which have Tc >= 95.0

### Step C: Unmeasurable materials found
1. O6.160Ca1Cu2Ba2Hg1 (5 elements)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements)
3. O4.047Cu1Ba2Hg1 (4 elements)
4. O4.066Cu1Ba2Hg1 (4 elements)
5. O4.160Cu1Ba2Hg1 (4 elements)
6. O4.180Cu1Ba2Hg1 (4 elements)

### Step D: Final pick
- Among unmeasurable materials predicted "high", choose one with fewest elements
- 4-element materials have fewer elements than 5-element ones
- Need to predict which 4-element unmeasurable materials are "high"

### Rule induction
- Looking at 5-element series: bell curve pattern with peak at mid-range oxygen
- All measurable 5-element materials have Tc >= 90 (high by task definition)
- Unmeasurable 5-element materials (O6.160, O6.270) are in high-Tc regions → predict high
- For 4-element series, assuming similar pattern:
  - O4.047, O4.066: low oxygen → possibly low Tc
  - O4.160, O4.180: mid-high oxygen → possibly high Tc

### Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K (127.0 - 32)
- Q1 (qualifying): Could not determine due to instrument time exhaustion
- Predictions: 6 unmeasurable materials (2 high for 5-element, 2 low and 2 high for 4-element)
- Final pick: O4.160Cu1Ba2Hg1 (4 elements, predicted high, expected higher Tc than O4.180)
- Rule: Bell curve pattern based on oxygen content within homologous series

### Budget Analysis
- Total budget: 45 units
- Used: 45 units (exhausted)
- Issue: Couldn't measure 4-element Hg materials for Step B due to instrument time exhaustion

### Final Answer Summary
- Peak: O6.210Ca1Cu2Ba2Hg1 with Tc = 127.0 K
- Threshold: 95.0 K (127.0 - 32)
- Q1 (qualifying): Could not determine due to instrument time exhaustion
- Predictions: 6 unmeasurable materials (2 high for 5-element, 2 low and 2 high for 4-element)
- Final pick: O4.160Cu1Ba2Hg1 (4 elements, predicted high, expected higher Tc than O4.180)
- Rule: Bell curve pattern based on oxygen content within homologous series

### Budget Analysis
- Total budget: 45 units
- Used: 45 units (exhausted)
- Issue: Couldn't measure 4-element Hg materials for Step B due to instrument time exhaustion

