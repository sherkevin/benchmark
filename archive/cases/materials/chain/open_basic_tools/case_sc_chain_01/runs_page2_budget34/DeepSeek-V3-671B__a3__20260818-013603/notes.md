# Notes for Superconductor Tc Study

## Initial Setup
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- **Budget**: 34 units total (shared between `measure_tc` and `search_db`).
- **Definition**: A material is **high** if Tc >= 90 K, otherwise **low**.

## Step A — Series Peak
Goal: Find the composition with MAXIMUM Tc (T*) in the homologous doping series (same element set, varying oxygen).

### Findings:
- Searches for materials containing `Ca`, `Cu`, `Ba`, `Hg` or `Hg`, `Cu` returned no results.
- Measured Tc of seed material `O6.080Ca1Cu2Ba2Hg1`: **92.0 K** (high).
- Assuming seed is the peak of its series (T* = 92.0 K).

### Budget:
- Used: 3 units (2 searches, 1 measurement).
- Remaining: 31 units.

## Step B — Filter Family B
Goal: Identify materials containing `Hg` and exactly 4 elements with Tc >= threshold1 (T* - 32 = 60.0 K).

### Findings:
- Searched for materials containing `Hg` and exactly 4 elements.
- Measured Tc for:
  - `O4.081Cu1Ba2Hg1`: **98.0 K** (qualifies for Q1).
  - `O4.085Cu1Ba2Hg1`: **92.0 K** (qualifies for Q1).
  - `O4.047Cu1Ba2Hg1`, `O4.066Cu1Ba2Hg1`: Unmeasurable (no synthesized samples).
- **Qualifying Materials (Q1)**:
  - `O4.081Cu1Ba2Hg1`
  - `O4.085Cu1Ba2Hg1`

### Budget:
- Used: 7 units (3 searches, 4 measurements).
- Remaining: 27 units.

## Step C — Extrapolate
Goal: Predict high/low for unmeasurable materials (those without synthesized samples).

### Findings:
- **Measurable Materials**:
  - `O6.080Ca1Cu2Ba2Hg1`: 92.0 K (high).
  - `O4.081Cu1Ba2Hg1`: 98.0 K (high).
  - `O4.085Cu1Ba2Hg1`: 92.0 K (high).
- **Unmeasurable Materials**:
  - `O4.047Cu1Ba2Hg1`
  - `O4.066Cu1Ba2Hg1`

### Rule Induced**:
Materials containing `Hg`, `Cu`, `Ba`, and `O` are likely high-Tc (>= 90 K).

### Predictions**:
- `O4.047Cu1Ba2Hg1`: high.
- `O4.066Cu1Ba2Hg1`: high.

### Budget:
- Used: 7 units (3 searches, 4 measurements).
- Remaining: 27 units.

## Step D — Final Pick
Goal: Choose the unmeasurable material predicted **high** with the fewest elements (break ties by higher expected Tc).

### Plan:
1. Select the best candidate from `predictions`.

---

**Budget Tracking**:
- Initial: 34 units
- Used: 0 units
- Remaining: 34 units
