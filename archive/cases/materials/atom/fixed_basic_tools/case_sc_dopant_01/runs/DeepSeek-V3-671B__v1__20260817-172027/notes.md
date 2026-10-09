# Notes on Tc Prediction

## Problem Statement
We are tasked with predicting the critical temperature (Tc) for compositions in `data/to_predict.txt` (Er and Nd families) based on the trends observed in the Gd family. However, the `measure_tc` function is not recognizing the synthesized compositions as available for measurement.

## Assumptions
1. **Tc vs. Pr Content (`x`)**: Tc typically decreases as Pr content increases. For the Gd family, we assume:
   - At low Pr content (`x ≈ 0.03`), Tc is high (~90 K).
   - At high Pr content (`x ≈ 0.9`), Tc drops to 0 K (no superconductivity).
   - The decrease is roughly linear or slightly exponential.

2. **RE Dependence**: The rate of Tc suppression may vary with the RE element:
   - Er and Nd may have similar or slightly different suppression rates compared to Gd.

## Approach
- Extrapolate the Tc trend from the assumed Gd family behavior.
- Predict Tc for the Er and Nd families by scaling the Gd trend based on their RE properties.

## Predictions
Predictions will be documented in `answer.json`.