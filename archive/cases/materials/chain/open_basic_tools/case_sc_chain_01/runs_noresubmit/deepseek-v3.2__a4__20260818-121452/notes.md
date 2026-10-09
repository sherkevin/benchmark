# Superconductor Tc Study Notes

## Budget Usage Summary
Total budget: 18 units
Used: 18 units
Remaining: 0 units

### Searches (6 units):
- "Hg" page 1: 1 unit
- "Ca" pages 1-5: 5 units

### Measurements (12 units):
Series measurements (O-Ca-Cu-Ba-Hg materials):
1. O6.080Ca1Cu2Ba2Hg1: 92.0 K ✓
2. O6.100Ca1Cu2Ba2Hg1: 92.5 K ✓
3. O6.130Ca1Cu2Ba2Hg1: 102.0 K ✓
4. O6.140Ca1Cu2Ba2Hg1: 105.0 K ✓
5. O6.160Ca1Cu2Ba2Hg1: No sample (unmeasurable)
6. O6.190Ca1Cu2Ba2Hg1: 122.0 K ✓
7. O6.210Ca1Cu2Ba2Hg1: 127.0 K ✓
8. O6.220Ca1Cu2Ba2Hg1: 127.0 K ✓
9. O6.240Ca1Cu2Ba2Hg1: 123.0 K ✓
10. O6.270Ca1Cu2Ba2Hg1: No sample (unmeasurable)
11. O6.300Ca1Cu2Ba2Hg1: 100.0 K ✓
12. O6.310Ca1Cu2Ba2Hg1: 98.0 K ✓

Additional measurement:
13. O4.047Cu1Ba2Hg1: 95.0 K ✓ (Hg-containing, 4 elements)

## Step A: Series Peak Analysis
**Series members found**: 12 materials with elements O, Ca, Cu, Ba, Hg, varying oxygen content
**Maximum Tc**: 127.0 K
**Peak compositions**: O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (both 127.0 K)
**Selected peak**: O6.210Ca1Cu2Ba2Hg1
**T***: 127.0 K
**Threshold1**: T* - 32 = 95.0 K

## Step B: Filter Family B (Hg-containing, exactly 4 elements)
**Materials found** (from Hg searches):
- O4.047Cu1Ba2Hg1 (measured: 95.0 K = threshold1) ✓ QUALIFIES
- O4.066Cu1Ba2Hg1 (not measured)
- O4.081Cu1Ba2Hg1 (not measured)
- O4.085Cu1Ba2Hg1 (not measured)
- O4.096Cu1Ba2Hg1 (not measured)
- O4.097Cu1Ba2Hg1 (not measured)

**Budget constraint**: Out of budget after measuring O4.047Cu1Ba2Hg1
**Q1 set**: {O4.047Cu1Ba2Hg1} (only confirmed qualifying material)

## Step C: Rule Induction for Unmeasurable Materials
**Unmeasurable materials found** (from discovery.json):
1. O6.160Ca1Cu2Ba2Hg1 - confirmed unmeasurable (Hg-containing cuprate)
2. O6.270Ca1Cu2Ba2Hg1 - confirmed unmeasurable (Hg-containing cuprate)
3. O7Ca1Cu2Sr2Tl0.500Pb0.500 - unmeasurable (contains Tl, complex fractions)
4. O8Ca0.850Cu2.200Ba2Tl1.800 - unmeasurable (contains Tl, complex fractions)

**Pattern analysis**:
All measurable Hg-containing materials have high Tc (≥90 K):
- O-Ca-Cu-Ba-Hg series: all ≥ 92 K
- O4.047Cu1Ba2Hg1: 95 K

Materials with Tl and complex fractional stoichiometries (O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Ca0.850Cu2.200Ba2Tl1.800) are unmeasurable and likely have low Tc due to instability.

**Rule induction**: 
- Hg-containing cuprates with oxygen content above 4.0 have high Tc (≥90 K)
- Materials containing Tl with complex fractional stoichiometries have low Tc

**Predictions**:
- O6.160Ca1Cu2Ba2Hg1: high (Hg-containing, oxygen > 4.0)
- O6.270Ca1Cu2Ba2Hg1: high (Hg-containing, oxygen > 4.0)
- O7Ca1Cu2Sr2Tl0.500Pb0.500: low (contains Tl, complex fractions)
- O8Ca0.850Cu2.200Ba2Tl1.800: low (contains Tl, complex fractions)

## Step D: Final Pick
**Unmeasurable materials predicted HIGH**:
1. O6.160Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~115 K)
2. O6.270Ca1Cu2Ba2Hg1 (5 elements, expected Tc ~110 K)

**Unmeasurable materials predicted LOW**:
1. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6+ elements, complex)
2. O8Ca0.850Cu2.200Ba2Tl1.800 (5+ elements, complex)

**Fewest elements among HIGH predictions**: Both have 5 elements (tie)
**Higher expected Tc**: O6.160Ca1Cu2Ba2Hg1 has higher expected Tc based on oxygen-Tc curve
**Final selection**: O6.160Ca1Cu2Ba2Hg1