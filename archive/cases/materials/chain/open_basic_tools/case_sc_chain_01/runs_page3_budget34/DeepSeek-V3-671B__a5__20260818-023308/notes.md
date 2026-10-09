# Notes on Superconductor Tc Study

## Step A: Series Peak
- Searched for materials containing `Hg` to identify the homologous series.
- Measured Tc for all synthesized materials in the series.
- **Peak Tc**: 127.0 K (`O6.210Ca1Cu2Ba2Hg1` and `O6.220Ca1Cu2Ba2Hg1`).

## Step B: Filter Family B
- Materials containing `Hg` and exactly 4 elements:
  - `O4.081Cu1Ba2Hg1`: Tc = 98.0 K (qualifies for Q1).
  - Others were unmeasurable.
- **Threshold**: 127.0 - 32 = 95.0 K.

## Step C: Extrapolate to Unmeasurable Materials
- **Rule**: A material is predicted as high if it contains exactly 4 elements (`O`, `Cu`, `Ba`, `Hg` or `O`, `Ca`, `Cu`, `Ba`, `Hg`) and its oxygen count is between 4.0 and 6.3.
- Predicted high for all unmeasurable materials in the series.

## Step D: Final Pick
- Chose `O4.180Cu1Ba2Hg1` (fewest elements among predicted high materials).

## Budget Usage
- Total budget: 34 units.
- Used: 31 units (search_db and measure_tc calls).