# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 34 measurements
- Definition: High Tc ≥ 90K, Low Tc < 90K

## Step A: Series Peak
- Measured materials from measurable.txt that belong to the same homologous doping series as the seed
- Seed series: Ca1Cu2Ba2Hg1 with varying oxygen
- Also measured: Cu1Ba2Hg1 with varying oxygen
- Found composition with MAXIMUM Tc
- Call that maximum T*

### Measurements:
- O6.080Ca1Cu2Ba2Hg1: Tc = 92.0 K
- O6.100Ca1Cu2Ba2Hg1: Tc = 92.5 K
- O6.130Ca1Cu2Ba2Hg1: Tc = 102.0 K
- O6.140Ca1Cu2Ba2Hg1: Tc = 105.0 K
- O6.160Ca1Cu2Ba2Hg1: no sample available
- O6.190Ca1Cu2Ba2Hg1: Tc = 122.0 K
- O6.210Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX!)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127.0 K (MAX!)
- O6.240Ca1Cu2Ba2Hg1: Tc = 123.0 K
- O6.270Ca1Cu2Ba2Hg1: no sample available
- O6.300Ca1Cu2Ba2Hg1: Tc = 100.0 K
- O6.310Ca1Cu2Ba2Hg1: Tc = 98.0 K
- O4.081Cu1Ba2Hg1: Tc = 98.0 K
- O4.162Cu1Ba2Hg1: Tc = 97.4 K
- O4.150Cu1Ba2Hg1: Tc = 97.0 K
- O4.220Cu1Ba2Hg1: Tc = 97.0 K
- O4.096Cu1Ba2Hg1: Tc = 95.6 K
- O4.097Cu1Ba2Hg1: Tc = 95.3 K
- O4.180Cu1Ba2Hg1: no sample available
- O4.047Cu1Ba2Hg1: no sample available
- O4.200Cu1Ba2Hg1: Tc = 94.2 K
- O4.101Cu1Ba2Hg1: Tc = 93.4 K
- O4.103Cu1Ba2Hg1: Tc = 92.5 K
- O4.085Cu1Ba2Hg1: Tc = 92.0 K
- O6Cu1Ba2Hg1Tl1: Tc = 0.0 K
- O10Ca2Cu3Ba2Tl2: Tc = 118.97 K
- O6Cu1Cd0.250Ba2Tl1.750: Tc = 88.0 K
- O7Cu3Ba2La1: Tc = 89.6 K
- O7Cu3Y0.200Ba2La0.800: Tc = 81.0 K

### Result:
- T* (max Tc) = 127.0 K
- Peak composition: O6.210Ca1Cu2Ba2Hg1 or O6.220Ca1Cu2Ba2Hg1

## Step B: Filter Family B
- Materials containing Hg and having exactly 4 elements
- threshold1 = T* - 32 = 127.0 - 32 = 95.0
- Report all materials with Tc ≥ threshold1
- Call this set Q1

### 4-element materials containing Hg:
- O4.081Cu1Ba2Hg1: Tc = 98.0 (≥ 95.0) ✓
- O4.162Cu1Ba2Hg1: Tc = 97.4 (≥ 95.0) ✓
- O4.150Cu1Ba2Hg1: Tc = 97.0 (≥ 95.0) ✓
- O4.220Cu1Ba2Hg1: Tc = 97.0 (≥ 95.0) ✓
- O4.096Cu1Ba2Hg1: Tc = 95.6 (≥ 95.0) ✓
- O4.097Cu1Ba2Hg1: Tc = 95.3 (≥ 95.0) ✓
- O4.200Cu1Ba2Hg1: Tc = 94.2 (< 95.0) ✗
- O4.101Cu1Ba2Hg1: Tc = 93.4 (< 95.0) ✗
- O4.103Cu1Ba2Hg1: Tc = 92.5 (< 95.0) ✗
- O4.085Cu1Ba2Hg1: Tc = 92.0 (< 95.0) ✗
- O4.180Cu1Ba2Hg1: no sample available
- O4.047Cu1Ba2Hg1: no sample available

### Result:
- Q1 = {O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1}

## Step C: Extrapolate
- Predict high/low for unmeasurable materials
- Rule must consider interactions between multiple factors
- Not single-factor rules

## Step D: Final Pick
- Among unmeasurable materials predicted as high in step C
- Return the one with fewest elements (break ties by higher expected Tc)