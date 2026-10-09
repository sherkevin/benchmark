# Superconductor Tc Study — Notes

## Task summary
- Budget: 18 units total (shared by measure_tc + search_db)
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg — 5 elements)
- "high" = Tc >= 90 K, "low" = Tc < 90 K
- Step A: find series (same element set, varying O), measure, find max Tc = T*
- Step B: threshold1 = T* - 32. Among Hg materials w/ exactly 4 elements, report all Tc >= threshold1 = Q1
- Step C: unmeasurable materials (no sample) → induce rule, predict high/low for ALL found
- Step D: among predicted-high unmeasurables, pick fewest elements (tie: higher expected Tc)

## Budget: 18/18 EXHAUSTED

## All 27 Hg-containing materials (from search_db "Hg" pages 1-9)
4-element (O,Cu,Ba,Hg) Hg-1201 series — 14 members (Step B candidates):
- O4.047Cu1Ba2Hg1   [MEAS 95.0]
- O4.066Cu1Ba2Hg1
- O4.081Cu1Ba2Hg1
- O4.085Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.101Cu1Ba2Hg1
- O4.103Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1   [MEAS 97.0]
- O4.160Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.180Cu1Ba2Hg1
- O4.200Cu1Ba2Hg1   [MEAS 94.2]
- O4.220Cu1Ba2Hg1

5-element (O,Ca,Cu,Ba,Hg) Hg-1212 series — 12 members (Step A series):
- O6.080Ca1Cu2Ba2Hg1 [seed, MEAS 92.0]
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1   [UNMEASURABLE — no sample]
- O6.190Ca1Cu2Ba2Hg1 [MEAS 122.0]
- O6.210Ca1Cu2Ba2Hg1 [MEAS 127.0]  ← peak plateau
- O6.220Ca1Cu2Ba2Hg1 [MEAS 127.0]  ← peak plateau
- O6.240Ca1Cu2Ba2Hg1 [MEAS 123.0]
- O6.270Ca1Cu2Ba2Hg1   [UNMEASURABLE — no sample]
- O6.300Ca1Cu2Ba2Hg1 [MEAS 100.0]
- O6.310Ca1Cu2Ba2Hg1

1 other (5-element, O,Cu,Ba,Hg,Tl):
- O6Cu1Ba2Hg1Tl1        (not 4-element, not in series)

## Step A — series peak
Measured Tc vs O content (Hg-1212, O6.x):
- O6.080 → 92.0
- O6.160 → UNMEASURABLE (no sample)
- O6.190 → 122.0
- O6.210 → 127.0  (peak plateau)
- O6.220 → 127.0  (peak plateau)
- O6.240 → 123.0
- O6.270 → UNMEASURABLE (no sample)
- O6.300 → 100.0
Dome shape: rises 92→122→127 (plateau O6.210-220)→ falls 123→100.
T* = MAXIMUM measured Tc = 127.0 K
Peak composition = O6.220Ca1Cu2Ba2Hg1 (and O6.210Ca1Cu2Ba2Hg1 tied at 127.0)

Decision: peak = O6.220Ca1Cu2Ba2Hg1, T* = 127.0

## Step B — filter family B (Hg + exactly 4 elements)
threshold1 = T* - 32 = 127.0 - 32.0 = 95.0
Candidates = the 14 O4.xxxCu1Ba2Hg1 (all contain Hg, exactly 4 elements: O,Cu,Ba,Hg).
Measured: O4.047=95.0, O4.150=97.0, O4.200=94.2.
Need Tc >= 95.0 for the rest. Must PREDICT via rule (budget exhausted).

## Step C — rule induction (predict high/low for unmeasurables)
Unmeasurable materials discovered via search_db:
1. O6.160Ca1Cu2Ba2Hg1  (in series, between 122 and 127 → predict)
2. O6.270Ca1Cu2Ba2Hg1  (in series, between 123 and 100 → predict)
NOTE: also some O4.0xx materials may be unmeasurable — but I could not measure them (budget). 
Task: predictions must contain ALL unmeasurable materials discovered. I discovered all 27 via 
search_db. The UNMEASURABLE ones (no sample) I confirmed: O6.160, O6.270. 
For O4.x I only tested 3 (all measurable). The rest are UNKNOWN — but "unmeasurable" 
means measure_tc returned "no sample exists". I never confirmed the unmeasured O4 ones 
are unmeasurable. They MIGHT be measurable (I just lacked budget). 

=> Only CONFIRMED unmeasurables: O6.160Ca1Cu2Ba2Hg1 and O6.270Ca1Cu2Ba2Hg1.
predictions = {these two}.

Rule induced: For the Hg-1212 series, Tc follows a doping dome vs oxygen content.
Interpolate within the dome:
- O6.160 sits between O6.100(unmeas,unknown) region and measured O6.190=122. Actually 
  between O6.140(unmeas) and O6.190=122 on rising side. On rising edge 92(080)→122(190), 
  O6.160 ~ ~118-122 region. Predict HIGH (>=90).
- O6.270 sits between O6.240=123 and O6.300=100 on falling side → ~110. Predict HIGH.

Both predicted HIGH (>=90 K). 
Warning says single-factor rules are wrong on several. But these two series members 
interpolate cleanly within measured dome → both high.

## Step D — final pick
Predicted-high unmeasurables: O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1.
Both have 5 elements (O,Ca,Cu,Ba,Hg). Tie on element count (both 5).
Break ties by higher expected Tc:
- O6.160 (rising side, near plateau) expected ~118-122
- O6.270 (falling side between 123 and 100) expected ~110-115
=> O6.160Ca1Cu2Ba2Hg1 has higher expected Tc.

FINAL PICK = O6.160Ca1Cu2Ba2Hg1
