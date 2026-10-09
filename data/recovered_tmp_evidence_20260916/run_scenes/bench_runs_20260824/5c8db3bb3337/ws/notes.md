# Notes on Predicting Tc for Gd Series

## Measured Tc Values

### Er Series
- `O7Cu3Ba2Pr0.030Er0.970`: 93.9 K
- `O7Cu3Ba2Pr0.076Er0.924`: 91.1 K
- `O7Cu3Ba2Pr0.150Er0.850`: 82.9 K
- `O7Cu3Ba2Pr0.200Er0.800`: 76.4 K
- `O7Cu3Ba2Pr0.300Er0.700`: 61.1 K
- `O7Cu3Ba2Pr0.400Er0.600`: 45.8 K
- `O7Cu3Ba2Pr0.500Er0.500`: 28.6 K
- `O7Cu3Ba2Pr0.800Er0.200`: 0.0 K
- `O7Cu3Ba2Pr0.900Er0.100`: 0.0 K

### Nd Series
- `O7Cu3Ba2Pr0.030Nd0.970`: 85.1 K
- `O7Cu3Ba2Pr0.080Nd0.920`: 68.2 K
- `O7Cu3Ba2Pr0.150Nd0.850`: 40.7 K
- `O7Cu3Ba2Pr0.200Nd0.800`: 31.4 K
- `O7Cu3Ba2Pr0.300Nd0.700`: 5.6 K
- `O7Cu3Ba2Pr0.500Nd0.500`: 0.0 K
- `O7Cu3Ba2Pr0.600Nd0.400`: 0.0 K
- `O7Cu3Ba2Pr0.700Nd0.300`: 0.0 K
- `O7Cu3Ba2Pr0.800Nd0.200`: 0.0 K
- `O7Cu3Ba2Pr0.900Nd0.100`: 0.0 K

## Observations
1. The Tc suppression is more pronounced in the Nd series compared to the Er series.
2. For both series, Tc decreases as the Pr content increases, eventually reaching 0 K (non-superconducting).
3. The Gd series is expected to follow a similar trend but may lie between the Er and Nd series in terms of suppression.

## Prediction Strategy
- Assume the Gd series will exhibit Tc values intermediate between the Er and Nd series for the same Pr content.
- Use linear interpolation between the Er and Nd series Tc values for each Pr content to estimate the Gd series Tc.

Next steps: Calculate the predicted Tc for each Gd composition.