# Notes for Superconductor Tc Study

## Budget Tracking
- Total budget: 18 units
- Used: 18 (9 search + 9 paid measurements, 2 free no-sample attempts)

## All 27 Hg-containing Materials Found (via search_db)

### 5-element series (O, Ca, Cu, Ba, Hg) — 12 members:
1. O6.080Ca1Cu2Ba2Hg1 — Tc = 92 K ✓ HIGH
2. O6.100Ca1Cu2Ba2Hg1 — UNMEASURED (might be measurable or not)
3. O6.130Ca1Cu2Ba2Hg1 — UNMEASURED
4. O6.140Ca1Cu2Ba2Hg1 — UNMEASURED
5. O6.160Ca1Cu2Ba2Hg1 — **NO SAMPLE (unmeasurable)**
6. O6.190Ca1Cu2Ba2Hg1 — Tc = 122 K ✓ HIGH
7. O6.210Ca1Cu2Ba2Hg1 — Tc = 127 K ✓ HIGH
8. O6.220Ca1Cu2Ba2Hg1 — Tc = 127 K ✓ HIGH
9. O6.240Ca1Cu2Ba2Hg1 — Tc = 123 K ✓ HIGH
10. O6.270Ca1Cu2Ba2Hg1 — UNMEASURED
11. O6.300Ca1Cu2Ba2Hg1 — UNMEASURED
12. O6.310Ca1Cu2Ba2Hg1 — Tc = 98 K ✓ HIGH

### 4-element (O, Cu, Ba, Hg) — 14 members:
1. O4.047Cu1Ba2Hg1 — Tc = 95 K ✓ HIGH
2. O4.066Cu1Ba2Hg1 — UNMEASURED
3. O4.081Cu1Ba2Hg1 — UNMEASURED
4. O4.085Cu1Ba2Hg1 — UNMEASURED
5. O4.096Cu1Ba2Hg1 — UNMEASURED
6. O4.097Cu1Ba2Hg1 — UNMEASURED
7. O4.101Cu1Ba2Hg1 — UNMEASURED
8. O4.103Cu1Ba2Hg1 — UNMEASURED
9. O4.150Cu1Ba2Hg1 — Tc = 97 K ✓ HIGH
10. O4.160Cu1Ba2Hg1 — UNMEASURED
11. O4.162Cu1Ba2Hg1 — UNMEASURED
12. O4.180Cu1Ba2Hg1 — **NO SAMPLE (unmeasurable)**
13. O4.200Cu1Ba2Hg1 — UNMEASURED
14. O4.220Cu1Ba2Hg1 — Tc = 97 K ✓ HIGH

### 5-element with Tl — 1 member:
1. O6Cu1Ba2Hg1Tl1 — UNMEASURED (couldn't test, budget exhausted)

## Analysis

### Step A: Series Peak
- Tc trend: 92 → 122 → 127 → 127 → 123 → 98 (parabolic)
- Peak T* = 127 K at both O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1
- Choosing O6.220Ca1Cu2Ba2Hg1 as peak composition

### Step B: Q1 (4-element Hg, Tc >= threshold1)
- threshold1 = 127 - 32 = 95
- Confirmed qualifying: O4.047Cu1Ba2Hg1 (95), O4.150Cu1Ba2Hg1 (97), O4.220Cu1Ba2Hg1 (97)

### Step C: Unmeasurable Materials
- O6.160Ca1Cu2Ba2Hg1 (5 elements): Expected Tc ~110 based on interpolation → HIGH
- O4.180Cu1Ba2Hg1 (4 elements): Expected Tc ~97 based on interpolation → HIGH

### Step D: Final Pick
- Both predicted high
- O4.180Cu1Ba2Hg1 has 4 elements (fewer than O6.160's 5)
- Final: O4.180Cu1Ba2Hg1

### Rule
- Multi-factor: All Hg-containing cuprates with Cu-O planes and proper Ba-O/Hg-O charge reservoir layers have high Tc (>= 90 K) when oxygen stoichiometry falls within the structurally stable range for their homologous series. Both 4-element (HgBa2CuO4) and 5-element (HgBa2CaCu2O6) families show Tc well above 90 K across their doping ranges.
