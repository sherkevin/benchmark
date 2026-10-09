# Notes for Superconductor Tc Study

## Initial Setup
- **Seed Material**: `O6.080Ca1Cu2Ba2Hg1`
- **Budget**: 18 units total (shared between `measure_tc` and `search_db`)
- **Definition of High Tc**: Tc >= 90 K

## Plan
1. **Step A**: Identify the homologous doping series (varying oxygen) and find the composition with maximum Tc (T*).
2. **Step B**: Filter materials containing Hg with exactly 4 elements and Tc >= (T* - 32).
3. **Step C**: Induce a rule to predict high/low Tc for unmeasurable materials.
4. **Step D**: Select the unmeasurable material predicted as high with the fewest elements.

## Budget Tracking
- **Initial Budget**: 18 units
- **Spent So Far**: 18 units
- **Remaining Budget**: 0 units

## Results
- **Peak Composition**: `O4.162Cu1Ba2Hg1`
- **Peak Tc (T*)**: 97.4 K
- **Threshold**: 65.4 K (T* - 32)

## Qualifying Materials (Q1)
- `O4.047Cu1Ba2Hg1` (95.0 K)
- `O4.081Cu1Ba2Hg1` (98.0 K)
- `O4.085Cu1Ba2Hg1` (92.0 K)
- `O4.096Cu1Ba2Hg1` (95.6 K)
- `O4.097Cu1Ba2Hg1` (95.3 K)
- `O4.101Cu1Ba2Hg1` (93.4 K)
- `O4.103Cu1Ba2Hg1` (92.5 K)
- `O4.150Cu1Ba2Hg1` (97.0 K)
- `O4.162Cu1Ba2Hg1` (97.4 K)

## Unmeasurable Materials
- None found yet (all materials measured were synthesizable).

## Next Steps
- Induce a rule for predicting Tc based on measurable materials.
- Identify unmeasurable materials and predict their Tc.