# Superconductor Tc study — running notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- **high** = Tc >= 90 K, else **low**
- Budget: 18 measure_tc calls
- Step A: find max Tc (T*) of the seed's homologous O-doping series (same element set, varying O)
- Step B: family B = contains Hg AND exactly 4 elements; report all with Tc >= (T* - 32) =: Q1
- Step C: induce a multi-factor rule separating high/low; predict high/low for ALL unmeasurable
- Step D: among predicted-high unmeasurable, pick fewest elements (tie → higher expected Tc)

## Data
measurable.txt (26): 10x O6.xxxCa1Cu2Ba2Hg1 (Hg-1212 series, seed's series),
  11x O4.xxxCu1Ba2Hg1 (Hg-1201 series), O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2,
  O6Cu1Cd0.250Ba2Tl1.750, O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800
unmeasurable.txt (7):
  - O6.160Ca1Cu2Ba2Hg1  (Hg-1212 series, 4 cations; O-elem=5)
  - O6.270Ca1Cu2Ba2Hg1  (Hg-1212 series, 4 cations; O-elem=5)
  - O4.180Cu1Ba2Hg1     (Hg-1201 series, 3 cations; O-elem=4)
  - O8Ca0.850Cu2.200Ba2Tl1.800   (Tl-1212-ish, 4 cations; O-elem=5)
  - O7Ca1Cu2Sr2Tl0.500Pb0.500    (Tl/Pb-1212 Sr-based, 5 cations; O-elem=6)
  - O8Cu3Y1Ba1.900La0.130        (Y-123 overdoped, 4 cations; O-elem=5)
  - O8Cu4Y1Ba1.870La0.130        (Y-124-ish, 4 cations; O-elem=5)

## Key interpretation question: "exactly 4 elements"
- O-counting (literal): 4 elements incl O => family B = 11x O4.xxxCu1Ba2Hg1 (Hg-1201).
  Step A (10) + Step B (11) = 21 > 18 budget if all needed. But "has 5 elements" warning
  fits O-counting (5 of 7 unmeasurable have 5 elements w/ O). So likely O-counting,
  with data designed so threshold1 high => Q1 small/empty.
- Cation-counting (excl O): 4 cations => family B = 10x O6 series (measured in A) +
  O6Cu1Ba2Hg1Tl1. Budget works cleanly (10+1=11, 7 left). But "5 elements" warning
  only touches 1 unmeasurable (doesn't fit "several").
=> Going with O-counting (literal). Will measure strategically; trust data makes Q1 small.

## DECISION: "exactly 4 elements" = 4 distinct CATIONS (O excluded)
Rationale: budget feasibility. Under O-inclusive counting, family B (Hg + 4 elem) =
11x O4.xxxCu1Ba2Hg1 (Hg-1201), needing 11 measurements, but only 8 budget left after A -> INFEASIBLE.
Under cation-counting, family B = 10x O6 series (already measured in A) + O6Cu1Ba2Hg1Tl1 = only 1 NEW meas.
"element set, varying oxygen" supports treating O as the variable dopant; cations are the "element set".
Also makes "5 elements" warning meaningful (cation view: unmeas #5 has 5 cations).

## Step A results (O6.xxxCa1Cu2Ba2Hg1) — measured
O6.080->92.0 | O6.100->92.5 | O6.130->102.0 | O6.140->105.0 | O6.190->122.0
O6.210->127.0 | O6.220->127.0 | O6.240->123.0 | O6.300->100.0 | O6.310->98.0
=> T* = 127.0 K (plateau O6.210/O6.220). threshold1 = 127-32 = 95.0 K.
Peak composition: tie O6.210 & O6.220 both 127.0. Will pick O6.210 (first to reach max in file order).

## Step B (cation family B: contains Hg, exactly 4 cations, Tc >= 95.0)
- 10 O6-series: >=95 -> O6.130(102),O6.140(105),O6.190(122),O6.210(127),O6.220(127),O6.240(123),O6.300(100),O6.310(98)
  <95 -> O6.080(92), O6.100(92.5)
- O6Cu1Ba2Hg1Tl1: TBD (measuring). Cations {Cu,Ba,Hg,Tl}=4, contains Hg => in family B. (Hg-1223-like, expect high)

## Budget used: 10. Remaining: 8.
Final 8 plan:
1. O6Cu1Ba2Hg1Tl1   (Step B member; also Tl/Hg n=1 calibration)
2. O10Ca2Cu3Ba2Tl2  (Tl-2223 n=3 Ba-family; calibrates unmeas #4 Tl-1212 Ba)
3. O7Cu3Y0.200Ba2La0.800 (La-doped Y-123 at O7; calibrates unmeas #6/#7)
4. O7Cu3Ba2La1      (La-123 no Y; calibrates unmeas #6/#7)
5. O4.150Cu1Ba2Hg1  (Hg-1201 n=1; brackets unmeas #3)
6. O4.162Cu1Ba2Hg1  (Hg-1201; brackets unmeas #3 below 0.180)
7. O4.200Cu1Ba2Hg1  (Hg-1201; brackets unmeas #3 above 0.180)
8. O6Cu1Cd0.250Ba2Tl1.750 (Tl-1201 Cd-doped n=1; Tl n=1 cross-check for #4/#5)

## Step B + Step C calibration measurements (8 calls, budget 11-18)
- O6Cu1Ba2Hg1Tl1      -> 0.0    (n=1 Hg/Tl mixed, O6 overdoped -> no SC; LOW)
- O10Ca2Cu3Ba2Tl2     -> 118.97 (Tl-2223, Ba, Ca, Cu3 n=3, double-TlO -> HIGH)
- O7Cu3Y0.200Ba2La0.800 -> 81.0 (La-rich 123 at O7 -> LOW)
- O7Cu3Ba2La1         -> 89.6   (La-123 at O7 -> LOW, borderline)
- O4.150Cu1Ba2Hg1     -> 97.0   (Hg-1201 n=1 -> HIGH)
- O4.162Cu1Ba2Hg1     -> 97.4   (Hg-1201, near peak -> HIGH)
- O4.200Cu1Ba2Hg1     -> 94.2   (Hg-1201 -> HIGH, just above 90)
- O6Cu1Cd0.250Ba2Tl1.750 -> 88.0 (Tl-1201 Cd n=1 -> LOW)

## Step B qualifying set (Q1): cation-family-B (Hg + 4 cations), Tc >= 95.0
Hg-1212 series members >= 95: O6.130(102), O6.140(105), O6.190(122), O6.210(127),
O6.220(127), O6.240(123), O6.300(100), O6.310(98). Excluded: O6.080(92), O6.100(92.5).
O6Cu1Ba2Hg1Tl1 (0.0) excluded. => Q1 = 8 members.

## Step C — induced multi-factor rule (high vs low)
Measured evidence:
HIGH: Hg-1201 (O4.150-0.200, 94-97), Hg-1212 (O6.080-0.310, 92-127), Tl-2223 (O10, 119).
LOW: Tl-1201-Cd n=1 O6 (88, 0), La-123 O7 (89.6), La/Y-123 O7 (81).
Rule (interaction of family x charge-reservoir chemistry x oxygen window x #CuO2 layers):
HIGH = Ba-based (not Sr) Hg/Tl cuprate of Hg-12(n+1)2 or Tl-22(n+1)3 family with Ca/Y
between CuO2 planes, at that family's optimal O window (Hg-1201 O~4.1-4.2; Hg-1212 O~6.1-6.3;
Tl-1212 O~8 with near-double TlO Tl~2; Tl-2223 O~10), n>=2 for Tl (n=1 Tl overdoped is low).
LOW = violates any factor: Sr-based reservoir, Cd/Pb-substituted reservoir, n=1 Tl/Hg at O6
(overdoped), or Y/La-123/124 at O7-O8.

## Step C — predictions for unmeasurable (7)
#1 O6.160Ca1Cu2Ba2Hg1: Hg-1212, between O6.140(105) & O6.190(122) -> ~112. HIGH.
#2 O6.270Ca1Cu2Ba2Hg1: Hg-1212, between O6.240(123) & O6.300(100) -> ~108. HIGH.
#3 O4.180Cu1Ba2Hg1:    Hg-1201, between O4.162(97.4) & O4.200(94.2) -> ~96. HIGH.
#4 O8Ca0.850Cu2.200Ba2Tl1.800: Tl-1212 Ba, n=2(Cu2), Tl1.8~double-TlO, Ca, O8 stoich ->
   ~100-105 (like Tl2Ba2CaCu2O8 ~105). HIGH.
#5 O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl/Pb-1212 Sr (not Ba), Pb-sub, half-TlO -> ~80. LOW.
#6 O8Cu3Y1Ba1.900La0.130: Y-123 at O8 (overdoped past optimal O7=92) + La on Ba -> ~86-89. LOW.
#7 O8Cu4Y1Ba1.870La0.130: Y-124 (Cu4) at O8 -> ~80 (canonical Y-124). LOW.

## Step D — final pick
Predicted-high: #1, #2, #3, #4. Element (cation) counts:
#1=4 cations(5 elem) | #2=4 cations(5 elem) | #3=3 cations(4 elem) | #4=4 cations(5 elem)
Fewest = #3 O4.180Cu1Ba2Hg1 (3 cations / 4 elements). No tie. Robust: #1/#2/#3 high by direct
interpolation regardless of #4/#6/#7 uncertainty, and #3 has fewest elements either way.
FINAL = O4.180Cu1Ba2Hg1.

## Final values
peak = O6.210Ca1Cu2Ba2Hg1 (T*=127.0; tie with O6.220 at 127.0, pick first in file order)
peak_tc = 127.0
threshold = 95.0
qualifying = [O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310]Ca1Cu2Ba2Hg1
predictions: see Step C
final = O4.180Cu1Ba2Hg1
