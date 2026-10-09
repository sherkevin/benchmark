# Superconductor Tc study — notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements {O, Ca, Cu, Ba, Hg} = 5 elements)
- "high" = Tc >= 90 K, "low" = Tc < 90 K
- Budget: 34 measure_tc calls

## Data inventory
### measurable.txt (26 entries)
Series A (seed family, {O,Ca,Cu,Ba,Hg}, 5 elem) — lines 1-10:
  O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310 Ca1Cu2Ba2Hg1
Family B (Hg, 4 elem {O,Cu,Ba,Hg}) — lines 11-21:
  O4.047, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.162, O4.200, O4.220 Cu1Ba2Hg1
Others (training for rule induction) — lines 22-26:
  O6Cu1Ba2Hg1Tl1  (5 elem, Hg+Tl)
  O10Ca2Cu3Ba2Tl2 (5 elem {O,Ca,Cu,Ba,Tl}) — analog of unmeas #4
  O6Cu1Cd0.250Ba2Tl1.750 (5 elem)
  O7Cu3Ba2La1 (4 elem {O,Cu,Ba,La})
  O7Cu3Y0.200Ba2La0.800 (5 elem {O,Cu,Y,Ba,La}) — analog of unmeas #6,#7

### unmeasurable.txt (7 entries)
1. O6.160Ca1Cu2Ba2Hg1   — in seed series! neighbors O6.140/O6.190 (interpolatable)
2. O6.270Ca1Cu2Ba2Hg1   — in seed series! neighbors O6.240/O6.300 (interpolatable)
3. O4.180Cu1Ba2Hg1      — in B family! neighbors O4.162/O4.200 (interpolatable)
4. O8Ca0.850Cu2.200Ba2Tl1.800 — {O,Ca,Cu,Ba,Tl}; analog line23 O10Ca2Cu3Ba2Tl2
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — {O,Ca,Cu,Sr,Tl,Pb}; NO measurable analog (Sr,Pb unique)
6. O8Cu3Y1Ba1.900La0.100 — {O,Cu,Y,Ba,La}; analog line26
7. O8Cu4Y1Ba1.870La0.130 — {O,Cu,Y,Ba,La}; analog line26

## Budget plan
- Step A: 10 calls (lines 1-10)
- Step B: 11 calls (lines 11-21)
- Training: 5 calls (lines 22-26)
Total = 26. 8 spare. Within 34.

## Measurements

### Step A — series O6.xCa1Cu2Ba2Hg1 (5 elem {O,Ca,Cu,Ba,Hg})
| O | 6.080 | 6.100 | 6.130 | 6.140 | 6.190 | 6.210 | 6.220 | 6.240 | 6.300 | 6.310 |
|Tc| 92.0 | 92.5 | 102.0 | 105.0 | 122.0 | **127.0** | **127.0** | 123.0 | 100.0 | 98.0 |
=> Peak T* = 127.0 K (tie: O6.210 & O6.220; pick first = O6.210Ca1Cu2Ba2Hg1). All HIGH.

### Step B — family O4.xCu1Ba2Hg1 (4 elem {O,Cu,Ba,Hg})
| O | 4.047 | 4.081 | 4.085 | 4.096 | 4.097 | 4.101 | 4.103 | 4.150 | 4.162 | 4.200 | 4.220 |
|Tc| 95.0 | 98.0 | 92.0 | 95.6 | 95.3 | 93.4 | 92.5 | 97.0 | 97.4 | 94.2 | 97.0 |
threshold1 = T* - 32 = 127.0 - 32 = 95.0. Filter: Hg ∧ 4 elem ∧ Tc>=95.0.
Q1 = {O4.047(95.0), O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0), O4.162(97.4), O4.220(97.0)}
(excluded: O4.085=92.0, O4.101=93.4, O4.103=92.5, O4.200=94.2 — below 95.0)

### Step C training (lines 22-26)
| comp | elements | #el | Ca? | Hg? | Tl? | Tc | H/L |
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl | 5 | N | Y | Y | 0.0 | LOW |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl | 5 | Y | N | Y | 118.97 | HIGH |
| O6Cu1Cd0.25Ba2Tl1.75 | O,Cu,Cd,Ba,Tl | 5 | N | N | Y | 88.0 | LOW |
| O7Cu3Ba2La1 | O,Cu,Ba,La | 4 | N | N | N | 89.6 | LOW |
| O7Cu3Y0.2Ba2La0.8 | O,Cu,Y,Ba,La | 5 | N | N | N | 81.0 | LOW |

## Induced rule (fits all 26 measured perfectly)
HIGH ⟺ (Ca present) OR (Hg present AND Tl absent)
  i.e. LOW ⟺ (Ca absent) AND (Hg absent OR Tl present)
Multi-factor interaction (Ca/Hg/Tl). Single-factor rules fail:
- "contains Hg": #23 (no Hg) HIGH, #22 (has Hg) LOW — wrong on both.
- "has 5 elements": #22/#24 (5el) LOW, B-family (4el) HIGH — wrong.
- "has Ca": actually perfect on measured, but fails conceptually + the rule needs the Hg/Tl clause for Ca-free cases.

Verification vs all measured:
- A-series (Ca,Hg,¬Tl): Ca → HIGH ✓ (10/10)
- B-family (¬Ca,Hg,¬Tl): Hg∧¬Tl → HIGH ✓ (11/11)
- #22 (¬Ca,Hg,Tl): ¬Ca ∧ Tl → LOW ✓
- #23 (Ca,Tl,¬Hg): Ca → HIGH ✓
- #24 (¬Ca,¬Hg,Tl): ¬Ca∧¬Hg → LOW ✓
- #25 (¬Ca,¬Hg,¬Tl): ¬Ca∧¬Hg → LOW ✓
- #26 (¬Ca,¬Hg): LOW ✓
26/26 correct.

## Step C predictions (apply rule to unmeasurable)
1. O6.160Ca1Cu2Ba2Hg1  — Ca present → HIGH (in A-series, ~113K between 6.14=105 & 6.19=122)
2. O6.270Ca1Cu2Ba2Hg1  — Ca present → HIGH (in A-series, ~111K between 6.24=123 & 6.30=100)
3. O4.180Cu1Ba2Hg1     — ¬Ca, Hg, ¬Tl → HIGH (in B-family, ~96K between 4.162=97.4 & 4.200=94.2)
4. O8Ca0.85Cu2.2Ba2Tl1.8 — Ca present → HIGH
5. O7Ca1Cu2Sr2Tl0.5Pb0.5 — Ca present → HIGH (6 elem)
6. O8Cu3Y1Ba1.9La0.1   — ¬Ca, ¬Hg → LOW (measured analogs #25/#26 both LOW)
7. O8Cu4Y1Ba1.87La0.13 — ¬Ca, ¬Hg → LOW (measured analog #26 LOW)

## Step D — final pick
HIGH predictions: #1(5 el), #2(5 el), #3(4 el), #4(5 el), #5(6 el).
Fewest elements = #3 (4 elements, unique). Final = O4.180Cu1Ba2Hg1.
(Tiebreak by higher expected Tc not needed — #3 uniquely fewest; expected Tc ~96 K.)
Note: final is robust — #3 is the only 4-element HIGH candidate under any plausible rule variant.

Budget: 26/34 used. 8 spare (nothing left to measure; unmeasurables have no sample).
