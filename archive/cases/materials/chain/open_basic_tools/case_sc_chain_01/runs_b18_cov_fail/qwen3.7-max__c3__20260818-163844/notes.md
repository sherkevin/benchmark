# Notes - Superconductor Tc Study

## Budget tracking
Total: 18 units (search_db + measure_tc combined) — ALL USED
- 10 search pages (9 Hg + 1 Cu)
- 8 measurements (7 successful + 1 error that didn't count)
- 2 measurements were free (no sample: O6.160, O6.270)

## All Hg materials found (27 total from search_db):

### 4-element series O{x}Cu1Ba2Hg1 (elements: O, Cu, Ba, Hg):
1. O4.047Cu1Ba2Hg1 — measurable (instrument exhausted)
2. O4.066Cu1Ba2Hg1 — "unknown composition" error
3. O4.081Cu1Ba2Hg1 — MEASURED: Tc = 98.0 (HIGH)
4. O4.085Cu1Ba2Hg1 — measurable (instrument exhausted)
5. O4.096Cu1Ba2Hg1 — measurable (instrument exhausted)
6. O4.097Cu1Ba2Hg1 — measurable (instrument exhausted)
7. O4.101Cu1Ba2Hg1 — measurable (instrument exhausted)
8. O4.103Cu1Ba2Hg1 — measurable (instrument exhausted)
9. O4.150Cu1Ba2Hg1 — measurable (instrument exhausted)
10. O4.160Cu1Ba2Hg1 — "unknown composition" error
11. O4.162Cu1Ba2Hg1 — measurable (instrument exhausted)
12. O4.180Cu1Ba2Hg1 — UNMEASURABLE (no sample)
13. O4.200Cu1Ba2Hg1 — measurable (instrument exhausted)
14. O4.220Cu1Ba2Hg1 — measurable (instrument exhausted)

### 5-element series O{x}Ca1Cu2Ba2Hg1 (seed's homologous series):
15. O6.080Ca1Cu2Ba2Hg1 — MEASURED: Tc = 92.0 (HIGH)
16. O6.100Ca1Cu2Ba2Hg1 — MEASURED: Tc = 92.5 (HIGH)
17. O6.130Ca1Cu2Ba2Hg1 — measurable (instrument exhausted)
18. O6.140Ca1Cu2Ba2Hg1 — measurable (instrument exhausted)
19. O6.160Ca1Cu2Ba2Hg1 — UNMEASURABLE (no sample)
20. O6.190Ca1Cu2Ba2Hg1 — MEASURED: Tc = 122.0 (HIGH)
21. O6.210Ca1Cu2Ba2Hg1 — MEASURED: Tc = 127.0 (HIGH)
22. O6.220Ca1Cu2Ba2Hg1 — MEASURED: Tc = 127.0 (HIGH)
23. O6.240Ca1Cu2Ba2Hg1 — MEASURED: Tc = 123.0 (HIGH)
24. O6.270Ca1Cu2Ba2Hg1 — UNMEASURABLE (no sample)
25. O6.300Ca1Cu2Ba2Hg1 — measurable (instrument exhausted)
26. O6.310Ca1Cu2Ba2Hg1 — MEASURED: Tc = 98.0 (HIGH)

### Other:
27. O6Cu1Ba2Hg1Tl1 — 5 elements (O, Cu, Ba, Hg, Tl), measurable (instrument exhausted)

### From Cu search (page 1 of ~12, 35 total):
28. O10Ca2Cu3Ba2Tl2 — 5 elements (O, Ca, Cu, Ba, Tl), no Hg, measurable (instrument exhausted)

## Step A Analysis
5-element series Tc vs oxygen:
O6.080→92.0, O6.100→92.5, O6.190→122.0, O6.210→127.0, O6.220→127.0, O6.240→123.0, O6.310→98.0
Peak T* = 127.0 at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220)

## Step B Analysis
threshold1 = 127.0 - 32 = 95.0
4-element Hg materials: all O{x}Cu1Ba2Hg1
Only measured: O4.081Cu1Ba2Hg1 at Tc=98.0 (≥ 95, qualifies)
Q1 = ["O4.081Cu1Ba2Hg1"]

## Step C Analysis
Unmeasurable materials (no sample):
1. O6.160Ca1Cu2Ba2Hg1 — 5 elements, O=6.160
   Between O6.100(92.5) and O6.190(122.0). Expected Tc ~110. PREDICT: HIGH
2. O6.270Ca1Cu2Ba2Hg1 — 5 elements, O=6.270
   Between O6.240(123.0) and O6.310(98.0). Expected Tc ~110. PREDICT: HIGH
3. O4.180Cu1Ba2Hg1 — 4 elements, O=4.180
   Only 4-element measurement O4.081→98. O4.180 is far from optimal doping.
   Using parabolic model: Tc ≈ 98*(1-20*(0.180-0.081)²) ≈ 79. PREDICT: LOW

Rule: "Tc depends on both the structural family (element set determining Tc_max) and oxygen doping level. 5-element Ca-containing series (Tc_max≈127K) stays high for all doping levels. 4-element series (Tc_max≈98K) drops below 90K when oxygen deviates significantly from optimal (~4.08). Single-factor rules fail because they ignore either the family or the doping."

## Step D Analysis
Predicted high unmeasurables:
- O6.160Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~110
- O6.270Ca1Cu2Ba2Hg1: 5 elements, expected Tc ~110

Both have 5 elements (tie). O6.160 is closer to optimal O (Δ=0.050 vs Δ=0.060), so higher expected Tc.
Final pick: O6.160Ca1Cu2Ba2Hg1
