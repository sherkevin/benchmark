# Notes for Superconductor Tc Study

## Budget: 34 units total (shared between measure_tc and search_db)

## Step A: Find series peak (T*)
Seed: O6.080Ca1Cu2Ba2Hg1
This is a homologous doping series with same element set, varying oxygen.
Elements: O, Ca, Cu, Ba, Hg

Need to find compositions with different oxygen levels.

### Measurements so far:
- 

### Search results:
- 

## Step B: Filter family B (contains Hg, exactly 4 elements)
threshold1 = T* - 32
Find all materials with Hg, exactly 4 elements, and Tc >= threshold1

## Step C: Extrapolate for unmeasurable materials
Find materials where measure_tc returns "no sample exists"
Induce a rule from measurable materials to predict high/low

## Step D: Final pick
Among unmeasurable materials predicted "high", choose one with FEWEST elements
Tie-break by higher expected Tc

## Progress tracker:
- [ ] Step A complete
- [ ] Step B complete
- [ ] Step C complete
- [ ] Step D complete
- [ ] answer.json written
