# Superconductor Tc study — running notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 family: O,Ca,Cu,Ba,Hg = 5 elements)
- Budget: 34 `measure_tc` calls
- High = Tc >= 90 K; Low = Tc < 90 K
- Step A: find MAX Tc (T*) across the homologous O-varying series sharing the seed's element set (O,Ca,Cu,Ba,Hg).
- Step B: threshold1 = T* - 32. Q1 = all materials containing Hg AND exactly 4 elements, with Tc >= threshold1.
- Step C: induce multi-factor rule separating high/low from measurables; predict every unmeasurable material.
- Step D: among predicted-high unmeasurables, pick FEWEST elements (tie-break: higher expected Tc).

## Measurable compositions (data/measurable.txt)
- Series A (Hg-1212, O,Ca,Cu,Ba,Hg, 5 elem): O6.080, O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310 (12)
- Series B (Hg-1201, O,Cu,Ba,Hg, 4 elem): O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.180, O4.047, O4.200, O4.101, O4.103, O4.085 (12)
- Others (rule-induction): O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800 (5)

## Unmeasurable compositions (data/unmeasurable.txt)
1. O6.160Ca1Cu2Ba2Hg1  (= measurable A-series! Hg-1212, 5 elem)
2. O6.270Ca1Cu2Ba2Hg1  (= measurable A-series! Hg-1212, 5 elem)
3. O4.180Cu1Ba2Hg1    (= measurable B-series! Hg-1201, 4 elem)
4. O4.047Cu1Ba2Hg1    (= measurable B-series! Hg-1201, 4 elem)
5. O4.066Cu1Ba2Hg1    (Hg-1201, 4 elem) — NOT in measurable
6. O4.160Cu1Ba2Hg1    (Hg-1201, 4 elem) — NOT in measurable (B has 4.162)
7. O8Ca0.850Cu2.200Ba2Tl1.800 (Tl-2212 type, 5 elem)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 (Tl/Pb-Sr-1212 type, 6 elem)
9. O8Cu3Y1Ba1.900La0.100 (Y-123 type, 5 elem)
10. O8Cu4Y1Ba1.870La0.130 (Y-124 type, 5 elem)

Note: unmeasurables 1-4 are exact duplicates of measurable compositions → can infer high/low from measured twin without violating "no measured number for unmeasurable" (only predict high/low).

## Budget plan
- Step A: 12 calls (A-series)
- Step B: 12 calls (B-series, all 4-elem Hg candidates)
- Step C rule base: 5 calls (other measurables)
- Total: 29 / 34. 5 spare.

## KEY FINDING: unmeasurable.txt entries have NO sample
- O6.160 and O6.270 are listed in BOTH files but returned "no sample exists" when measured.
- => unmeasurable.txt = unsynthesized (no physical sample). Don't waste budget on them.
- For Step B, SKIP O4.180 & O4.047 (they're in unmeasurable.txt → no sample).

## Step A measurements (A-series, Hg-1212, 5 elem)
| composition | Tc | note |
|-------------|----|------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | unmeasurable twin |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | **PEAK (tied)** |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | **PEAK (tied)** |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | unmeasurable twin |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | |

Dome shape confirmed: rises 92→127 (O6.21-O6.22 plateau), falls to 98.
Unmeasured O6.160 is between 105 (O6.140) and 122 (O6.190) → on rising edge, <127.
Unmeasured O6.270 is between 123 (O6.240) and 100 (O6.300) → on falling edge, <127.
**T* = 127.0 K, peak = O6.210Ca1Cu2Ba2Hg1** (first occurrence of max).

## Step B
threshold1 = T* - 32 = 127 - 32 = 95.0 K
Q1 = measurable materials with (contains Hg) AND (exactly 4 elements) AND Tc >= 95.
Candidates: B-series Hg-1201 (O,Cu,Ba,Hg = 4 elem, has Hg). Skip O4.180/O4.047 (no sample).

## Step B measurements (B-series, Hg-1201, 4 elem)
| composition | Tc | >=95? |
|-------------|----|------|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
(O4.180, O4.047 skipped — no sample)

threshold1 = 95.0. Q1 (sorted): O4.081, O4.096, O4.097, O4.150, O4.162, O4.220.

## Step C — rule induction
Goal: predict high (>=90) vs low (<90) for 10 unmeasurables. Multi-factor rule.

## Rule-base measurements (other measurables)
| composition | Tc | elements | family | spacer |
|-------------|----|----------|--------|--------|
| O6Cu1Ba2Hg1Tl1 | 0.0 | O,Cu,Ba,Hg,Tl (5) | 1201 mixed | Hg+Tl MIXED |
| O10Ca2Cu3Ba2Tl2 | 118.97 | O,Ca,Cu,Ba,Tl (5) | Tl-2223 | Tl2 pure |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | O,Cu,Cd,Ba,Tl (5) | 1201 mixed | Tl+Cd MIXED |
| O7Cu3Ba2La1 | 89.6 | O,Cu,Ba,La (4) | La-123 | La (non-Hg/Tl) |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | O,Cu,Y,Ba,La (5) | Y/La-123 | Y+La MIXED |

All measured points summary (22 total):
- HIGH (>=90): all 17 Hg-1201/Hg-1212 (Hg unmixed spacer, Ba), + Tl-2223 (Tl unmixed spacer, Ba, Ca).
- LOW (<90): Tl/Hg-1201 (Hg+Tl mixed), Tl/Cd-1201 (Tl+Cd mixed), La-123 (La non-Hg/Tl spacer), Y/La-123 (Y+La mixed + non-Hg/Tl).

## Step C — induced multi-factor RULE
HIGH (Tc>=90) IFF the cuprate has a SINGLE UNMIXED Hg-or-Tl spacer-cation layer
(i.e. a clean Hg-12(n-1)n or Tl-22(n-1)n family member WITH Ba).
=> LOW when: (a) the spacer layer is mixed (two different spacer cations: Hg+Tl, Tl+Cd, Tl+Pb, Y+La),
OR (b) the spacer is a non-Hg/non-Tl cation (La, Y-only), OR (c) Ba is absent (Sr instead of Ba).
Why multi-factor: "contains Hg" fails (Tl/Hg-1201 has Hg yet Tc=0); "has 5 elem" fails (Hg-1201 has 4 yet HIGH);
"contains Tl" fails (Tl/Cd-1201 has Tl yet Tc=88). Identity+mixing+Ba together fit ALL 22 points.

## Step C — predictions for unmeasurable.txt
1. O6.160Ca1Cu2Ba2Hg1  -> Hg unmixed + Ba -> HIGH (also: in A-series dome, between 105@4.140-neigh & 122)
2. O6.270Ca1Cu2Ba2Hg1  -> Hg unmixed + Ba -> HIGH (A-series falling edge, between 123 & 100)
3. O4.180Cu1Ba2Hg1     -> Hg unmixed + Ba -> HIGH (B-series, ~95-96)
4. O4.047Cu1Ba2Hg1     -> Hg unmixed + Ba -> HIGH (B-series edge, ~93-95)
5. O4.066Cu1Ba2Hg1     -> Hg unmixed + Ba -> HIGH (B-series, ~95-97)
6. O4.160Cu1Ba2Hg1     -> Hg unmixed + Ba -> HIGH (B-series, bracketed by 97.0@4.150 & 97.4@4.162 ~97)
7. O8Ca0.850Cu2.200Ba2Tl1.800 -> Tl unmixed (Tl1.8) + Ba + Ca, like Tl-2223 -> HIGH
8. O7Ca1Cu2Sr2Tl0.500Pb0.500  -> Tl+Pb MIXED spacer + Sr(no Ba) -> LOW
9. O8Cu3Y1Ba1.900La0.100  -> Y spacer (non-Hg/Tl) + Ba-site La mixing -> LOW
10. O8Cu4Y1Ba1.870La0.130 -> Y spacer (non-Hg/Tl) -> LOW (Y-124-type, ~80)

HIGH predicted: 1,2,3,4,5,6,7.  LOW predicted: 8,9,10.

## Step D — final pick
Predicted-HIGH unmeasurables by element count:
- 5 elem: #1(O6.160...), #2(O6.270...), #7(O8...Tl1.800)
- 4 elem: #3(O4.180), #4(O4.047), #5(O4.066), #6(O4.160)  <- FEWEST
Tie-break (higher expected Tc) among the 4-elem Hg-1201 set:
  O4.160 ~97.2 (best-bracketed by 4.150=97.0 & 4.162=97.4)  <- HIGHEST expected
  O4.066 ~95-97 (sparse, near 4.081=98 / 4.085=92 edge)
  O4.180 ~95-96 (between 97.4 & 94.2, declining)
  O4.047 ~93-95 (far edge, uncertain)
FINAL: O4.160Cu1Ba2Hg1

## Budget: 25/34 used. Stopping — rule fully supported by 5 "other" points; unmeasurables cannot be measured.

