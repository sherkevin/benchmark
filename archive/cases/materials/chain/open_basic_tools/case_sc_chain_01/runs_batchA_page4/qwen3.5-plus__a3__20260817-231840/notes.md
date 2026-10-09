# Notes for Superconductor Tc Study

## Seed material
- `O6.080Ca1Cu2Ba2Hg1`

## Step A: Homologous series peak
Series: O-Ca-Cu-Ba-Hg (5 elements, varying oxygen)

Measured Tc values:
- O6.080Ca1Cu2Ba2Hg1: 92.0
- O6.100Ca1Cu2Ba2Hg1: 92.5
- O6.130Ca1Cu2Ba2Hg1: 102.0
- O6.140Ca1Cu2Ba2Hg1: 105.0
- O6.160Ca1Cu2Ba2Hg1: no sample
- O6.190Ca1Cu2Ba2Hg1: 122.0
- O6.210Ca1Cu2Ba2Hg1: 127.0 ← PEAK (T*)
- O6.220Ca1Cu2Ba2Hg1: 127.0 ← also peak
- O6.240Ca1Cu2Ba2Hg1: 123.0
- O6.270Ca1Cu2Ba2Hg1: no sample
- O6.300Ca1Cu2Ba2Hg1: 100.0
- O6.310Ca1Cu2Ba2Hg1: 98.0

**T* = 127.0 K**
**Peak composition: O6.210Ca1Cu2Ba2Hg1**

## Step B: Filter family B (Hg, exactly 4 elements, Tc >= threshold1)
threshold1 = T* - 32 = 127.0 - 32 = 95.0

4-element Hg materials (O, Cu, Ba, Hg):
- O4.047Cu1Ba2Hg1: no sample
- O4.066Cu1Ba2Hg1: no sample
- O4.081Cu1Ba2Hg1: 98.0 >= 95 → QUALIFIES
- O4.085Cu1Ba2Hg1: 92.0 < 95 → NO
- O4.096Cu1Ba2Hg1: 95.6 >= 95 → QUALIFIES
- O4.097Cu1Ba2Hg1: 95.3 >= 95 → QUALIFIES
- O4.101Cu1Ba2Hg1: 93.4 < 95 → NO
- O4.103Cu1Ba2Hg1: 92.5 < 95 → NO
- O4.150Cu1Ba2Hg1: 97.0 >= 95 → QUALIFIES
- O4.160Cu1Ba2Hg1: no sample
- O4.162Cu1Ba2Hg1: 97.4 >= 95 → QUALIFIES
- O4.180Cu1Ba2Hg1: no sample
- O4.200Cu1Ba2Hg1: 94.2 < 95 → NO
- O4.220Cu1Ba2Hg1: 97.0 >= 95 → QUALIFIES

**Q1 (qualifying):**
- O4.081Cu1Ba2Hg1
- O4.096Cu1Ba2Hg1
- O4.097Cu1Ba2Hg1
- O4.150Cu1Ba2Hg1
- O4.162Cu1Ba2Hg1
- O4.220Cu1Ba2Hg1

## Step C: Predictions for unmeasurable materials

Induced rule: **HIGH if (contains Ca) OR (contains Hg AND NOT contains Tl); otherwise LOW**

Unmeasurable materials and predictions:
- O4.047Cu1Ba2Hg1: Hg, no Tl → HIGH
- O4.066Cu1Ba2Hg1: Hg, no Tl → HIGH
- O4.160Cu1Ba2Hg1: Hg, no Tl → HIGH
- O4.180Cu1Ba2Hg1: Hg, no Tl → HIGH
- O6.160Ca1Cu2Ba2Hg1: has Ca → HIGH
- O6.270Ca1Cu2Ba2Hg1: has Ca → HIGH
- O7Ca1Cu2Sr2Tl0.500Pb0.500: has Ca → HIGH
- O8Ca0.850Cu2.200Ba2Tl1.800: has Ca → HIGH
- O8Cu3Y1Ba1.900La0.100: no Ca, no Hg → LOW
- O8Cu4Y1Ba1.870La0.130: no Ca, no Hg → LOW

## Step D: Final pick
Among unmeasurable HIGH predictions, fewest elements = 4:
- O4.047Cu1Ba2Hg1 (4 elem)
- O4.066Cu1Ba2Hg1 (4 elem)
- O4.160Cu1Ba2Hg1 (4 elem)
- O4.180Cu1Ba2Hg1 (4 elem)

Break tie by expected Tc. Based on 4-element Hg series trend:
- O4.160 is closest to peak at O4.162 (97.4)
- Expected highest Tc: O4.160Cu1Ba2Hg1

**Final answer: O4.160Cu1Ba2Hg1**

## Budget used: 41 units (36 search + 25 measure? Let me recount)
Actually: 7 pages Hg search + 9 pages Cu search = 16 search units
Measurements: 12 (step A) + 14 (step B) + 9 (remaining) = 35 measurements
Wait, that's 51 which exceeds 45...

Let me recount from the tool outputs:
- After Hg page 7: budget_used = 7
- After all measurements: budget_used = 41 (last measurement showed 41)

So total budget used: 41 units (within 45 limit)
