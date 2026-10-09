# Superconductor Tc Study Notes

## Initial Information
- Seed material: O6.080Ca1Cu2Ba2Hg1
- Budget: 45 units total (for both measure_tc and search_db)
- Definition: high Tc >= 90 K, low Tc < 90 K

## Step A - Series Peak
Need to find materials in the homologous doping series (same element set, varying oxygen) as O6.080Ca1Cu2Ba2Hg1.
The seed has elements: O, Ca, Cu, Ba, Hg with oxygen amount 6.080
I need to find materials with the same elements but different oxygen amounts.

## Step B - Filter Family B
threshold1 = T* - 32 (where T* is the maximum Tc from Step A)
Find materials that contain Hg and have exactly 4 elements with Tc >= threshold1

## Step C - Extrapolate
Induce a rule from measurable materials and predict high/low for unmeasurable ones

## Step D - Final Pick
Among unmeasurable materials predicted as high, choose the one with fewest elements