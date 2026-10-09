# Superconductor Tc Study Notes

## Step C: Induce a rule to predict high/low Tc for unmeasurable materials

### Results:
- **Rule**: "Materials containing Hg and exactly 4 elements (O, Cu, Ba, Hg) are high-Tc (≥ 90 K). Materials with additional elements (e.g., Ca) may also be high-Tc if they follow similar doping patterns."
- **Predictions**:
  ```json
  {
    "O4.180Cu1Ba2Hg1": "high",
    "O6.100Ca1Cu2Ba2Hg1": "high",
    "O6.130Ca1Cu2Ba2Hg1": "high",
    "O6.140Ca1Cu2Ba2Hg1": "high"
  }
  ```

## Step D: Final pick among unmeasurable materials predicted "high"

### Plan:
1. Among unmeasurable materials predicted "high", choose the one with the fewest elements.
2. Break ties by higher expected Tc (based on oxygen content trends).

### Candidates:
1. `O4.180Cu1Ba2Hg1`: 4 elements (O, Cu, Ba, Hg)
2. `O6.100Ca1Cu2Ba2Hg1`: 5 elements (O, Ca, Cu, Ba, Hg)
3. `O6.130Ca1Cu2Ba2Hg1`: 5 elements
4. `O6.140Ca1Cu2Ba2Hg1`: 5 elements

### Selection:
- `O4.180Cu1Ba2Hg1` has the fewest elements (4).