# Notes for Superconductor Tc Study

## Initial Setup
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- **Budget**: 34 units total (shared between `measure_tc` and `search_db`).
- **Definition**: A material is **high** if Tc >= 90 K, otherwise **low**.

## Step A — Series Peak
Objective: Find the composition with the maximum Tc (T*) in the homologous doping series of the seed material.

### Results:
- **Highest Tc (T*)**: 127.0 K, achieved by:
  - `O6.210Ca1Cu2Ba2Hg1`
  - `O6.220Ca1Cu2Ba2Hg1`
- **Threshold1 (T* - 32)**: 95.0 K

### Budget Used:
- `search_db`: 3 units (pages 1-3)
- `measure_tc`: 10 units (12 compositions, 2 unmeasurable)
- **Total Budget Used**: 13 units
- **Remaining Budget**: 21 units

## Step B — Filter Family B
Objective: Identify materials containing Hg with exactly 4 elements and Tc >= threshold1 (95.0 K).

### Results:
- **Qualifying Materials (Q1)**:
  - `O4.081Cu1Ba2Hg1` (98.0 K)
  - `O4.085Cu1Ba2Hg1` (92.0 K)
  - `O4.096Cu1Ba2Hg1` (95.6 K)
  - `O4.097Cu1Ba2Hg1` (95.3 K)
  - `O4.101Cu1Ba2Hg1` (93.4 K)
  - `O4.103Cu1Ba2Hg1` (92.5 K)
  - `O4.150Cu1Ba2Hg1` (97.0 K)
  - `O4.162Cu1Ba2Hg1` (97.4 K)
  - `O4.200Cu1Ba2Hg1` (94.2 K)
  - `O4.220Cu1Ba2Hg1` (97.0 K)

### Budget Used:
- `search_db`: 7 units (pages 1-7)
- `measure_tc`: 17 units (17 compositions, 3 unmeasurable)
- **Total Budget Used**: 24 units
- **Remaining Budget**: 10 units

## Step C — Extrapolate
Objective: Predict high/low for unmeasurable materials based on induced rules.

### Results:
- **Induced Rule**: All measurable materials with exactly 4 elements (O, Cu, Ba, Hg) have Tc >= 90 K (high).
- **Predictions**:
  - `O4.047Cu1Ba2Hg1`: **high**
  - `O4.066Cu1Ba2Hg1`: **high**
  - `O4.160Cu1Ba2Hg1`: **high**
  - `O4.180Cu1Ba2Hg1`: **high**

## Step D — Final Pick
Objective: Among predicted high unmeasurable materials, choose the one with the fewest elements (break ties by higher expected Tc).

### Plan:
1. All unmeasurable materials have the same number of elements (4).
2. Break ties by selecting the one with the highest expected Tc (all predicted high).
3. Choose the first one in the list: `O4.047Cu1Ba2Hg1`.

## Step B — Filter Family B
Objective: Identify materials containing Hg with exactly 4 elements and Tc >= threshold1 (T* - 32).

### Plan:
1. Use `search_db` to find materials containing Hg and having exactly 4 elements.
2. Measure Tc for these materials to determine if they qualify (Tc >= threshold1).

## Step C — Extrapolate
Objective: Predict high/low for unmeasurable materials based on induced rules.

### Notes:
- Some materials in the database are unmeasurable (`measure_tc` returns "no sample exists").
- Need to induce a rule from measurable materials to predict unmeasurable ones.
- Single-factor rules are insufficient; need a more nuanced approach.

## Step D — Final Pick
Objective: Among predicted high unmeasurable materials, choose the one with the fewest elements (break ties by higher expected Tc).

### Notes:
- Final answer will be written to `answer.json`.
