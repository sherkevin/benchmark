# Superconductor Tc Study Notes

## Initial Information
- Seed material: `O6.080Ca1Cu2Ba2Hg1`
- Definition: high Tc >= 90 K, low Tc < 90 K
- Budget: 34 units total (for both measure_tc and search_db)
- Both tools cost 1 budget unit each

## Step A: Series Peak
- Need to find materials in the same homologous doping series (same elements, varying oxygen)
- Find the composition with maximum Tc (T*)

## Step B: Filter Family B
- Materials containing Hg with exactly 4 elements
- Threshold = T* - 32
- Find all materials with Tc >= threshold

## Step C: Extrapolate
- Some materials cannot be measured ("no sample exists")
- Need to induce a rule from measurable materials
- Predict high/low for unmeasurable ones

## Step D: Final Pick
- From unmeasurable materials predicted as "high"
- Choose the one with fewest elements
- Break ties by higher expected Tc