# Notes: Predicting Tc for Gd Series Cuprates

## Investigation Process

I began by searching the materials database for compositions containing "Gd", "Pr", "Nd", and "Er" to understand the available data.

### Search Results
- Found 11 Gd-series compositions: O7Cu3Ba2Pr{x}Gd{1-x} where x ranges from 0.030 to 0.900
- Found Nd-series compositions with similar Pr substitutions
- Found Er-series compositions with similar Pr substitutions

### Measurement Results
I measured Tc values for various Nd and Er compositions to establish the suppression pattern:

**Nd Series:**
- O7Cu3Ba2Pr0.030Nd0.970: Tc = 85.1 K
- O7Cu3Ba2Pr0.150Nd0.850: Tc = 40.7 K
- O7Cu3Ba2Pr0.500Nd0.500: Tc = 0.0 K
- O7Cu3Ba2Pr0.900Nd0.100: Tc = 0.0 K

**Er Series:**
- O7Cu3Ba2Pr0.030Er0.970: Tc = 93.9 K
- O7Cu3Ba2Pr0.150Er0.850: Tc = 82.9 K
- O7Cu3Ba2Pr0.500Er0.500: Tc = 28.6 K
- O7Cu3Ba2Pr0.900Er0.100: Tc = 0.0 K

**Gd Series:**
- All Gd compositions returned sample_available: false (not synthesized yet)
- Need to predict based on the established patterns

### Pattern Analysis
Looking at the data, I observed:
1. As Pr content increases, Tc decreases monotonically for all RE elements
2. Different RE elements have different baseline Tc values (Er > Nd at low Pr)
3. The suppression rate seems to follow a similar curve across different RE elements
4. At high Pr content (0.5+), Tc approaches 0 for all RE elements

### Prediction Method
For the Gd series, I estimated Tc values based on interpolating between the Nd and Er trends, considering that Gd has an ionic radius between Nd and Er, suggesting it should have a similar suppression curve but potentially with a baseline Tc value between Nd and Er.

The predictions follow the pattern where low Pr content yields high Tc, gradually decreasing to 0 at high Pr content.