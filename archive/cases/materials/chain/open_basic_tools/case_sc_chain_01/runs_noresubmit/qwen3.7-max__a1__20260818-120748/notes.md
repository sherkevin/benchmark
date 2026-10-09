# Notes - Superconductor Tc Study

## Task Overview
- Budget: 18 units total (shared between measure_tc and search_db)
- Seed: O6.080Ca1Cu2Ba2Hg1
- High = Tc >= 90 K, Low = Tc < 90 K

## Budget Tracking
- Used: 18/18 (DONE)
- 1 empty search + 9 Hg pages + 8 paid measurements (1 unmeasurable was free)

## Measurement Results:
### 5-element series (O-Ca-Cu-Ba-Hg):
- O6.080Ca1Cu2Ba2Hg1: Tc = 92 K (HIGH)
- O6.160Ca1Cu2Ba2Hg1: UNMEASURABLE
- O6.190Ca1Cu2Ba2Hg1: Tc = 122 K (HIGH)
- O6.220Ca1Cu2Ba2Hg1: Tc = 127 K (HIGH) ← PEAK
- O6.240Ca1Cu2Ba2Hg1: Tc = 123 K (HIGH)
- O6.310Ca1Cu2Ba2Hg1: Tc = 98 K (HIGH)

### 4-element series (O-Cu-Ba-Hg):
- O4.047Cu1Ba2Hg1: Tc = 95 K (HIGH)
- O4.150Cu1Ba2Hg1: Tc = 97 K (HIGH)
- O4.220Cu1Ba2Hg1: Tc = 97 K (HIGH)

## Analysis:
### Step A: Peak
- T* = 127 K at O6.220Ca1Cu2Ba2Hg1
- Dome shape: 92 → 122 → 127 → 123 → 98

### Step B: threshold1 = 127 - 32 = 95
- 4-element Hg materials with Tc >= 95
- Measured: O4.047 (95), O4.150 (97), O4.220 (97) - all qualify
- Flat Tc trend (95-97 K) suggests all 14 qualify

### Step C: Unmeasurable materials
- Confirmed: O6.160Ca1Cu2Ba2Hg1
- Based on dome interpolation: Tc ≈ 114 K → HIGH

### Step D: Final pick
- O6.160Ca1Cu2Ba2Hg1 (5 elements, predicted high)

## Final Answer Written to answer.json

### Summary of approach:
1. Searched for all Hg-containing materials (9 pages, 27 materials found)
2. Identified homologous series: O-Ca-Cu-Ba-Hg (12 members, varying oxygen 6.080-6.310)
3. Measured 5 series members + found 1 unmeasurable → peak at O6.220 (127 K)
4. Measured 3 4-element materials → flat Tc 95-97 K, all >= threshold (95)
5. Inferred all 14 4-element materials qualify for Q1
6. Predicted O6.160Ca1Cu2Ba2Hg1 as high (dome interpolation ~114 K)
7. Final pick: O6.160Ca1Cu2Ba2Hg1 (only confirmed unmeasurable, 5 elements, predicted high)

### Budget usage:
- 10 search units (1 empty + 9 Hg pages)
- 8 measurement units (1 unmeasurable was free)
- Total: 18/18

## Step A: Find homologous series peak
- Seed has elements: O, Ca, Cu, Ba, Hg (5 elements)
- Series = same elements, varying oxygen
- Need to search for Hg-Ba-Ca-Cu-O variants

## Step B: Filter family B (Hg + exactly 4 elements)
- threshold1 = T* - 32

## Step C: Predict unmeasurable materials

## Step D: Final pick among predicted high
