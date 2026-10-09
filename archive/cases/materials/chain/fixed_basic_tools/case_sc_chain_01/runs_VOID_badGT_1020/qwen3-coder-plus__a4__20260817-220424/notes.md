# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Definition: High Tc >= 90 K, Low Tc < 90 K
- Measurement budget: 34 calls to measure_tc

## Step A: Series Peak
Need to measure all compositions in measurable.txt to find the one with MAXIMUM Tc.
This includes the seed material and other compositions in the same homologous doping series.

## Step B: Filter Family B
- threshold1 = T* - 32
- Find materials containing Hg with exactly 4 elements where Tc >= threshold1

## Step C: Extrapolation
- Predict high/low for unmeasurable materials based on measurable ones
- Need to find a multi-factor rule

## Step D: Final Pick
- From unmeasurable materials predicted as high, select the one with fewest elements