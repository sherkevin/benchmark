# Superconductor Tc study — running notes

## Budget: 34 measure_tc calls. KEY FINDING: null ("no sample") calls do NOT consume budget.

## Step A — series peak (seed O6.080Ca1Cu2Ba2Hg1)
Homologous doping series = same element set {O,Ca,Cu,Ba,Hg} (5 elements), varying O.
Measured series Tc (Ca1Cu2Ba2Hg1):
- O6.080 = 92.0
- O6.100 = 92.5
- O6.130 = 102.0
- O6.140 = 105.0
- O6.160 = NULL (no sample — also in unmeasurable.txt)
- O6.190 = 122.0
- O6.210 = 127.0  <-- MAX (tie)
- O6.220 = 127.0  <-- MAX (tie)
- O6.240 = 123.0
- O6.270 = NULL (no sample — also in unmeasurable.txt)
- O6.300 = 100.0
- O6.310 = 98.0
T* = 127.0 (peak composition O6.210Ca1Cu2Ba2Hg1; O6.220 ties).
Budget after A: 10 used (null calls free).
Bell curve: rises 92->127 (at O6.210/6.220) then falls to 98. Unmeasured O6.160 (~113) and O6.270 (~113) sit on the flanks, below peak, so do not affect T*.

threshold1 (step B) = T* - 32 = 127 - 32 = 95.0

## Step B — family B (contains Hg AND exactly 4 elements = O,Cu,Ba,Hg)
Family-B = Hg + exactly 4 elements. Candidates:
- Series Ca1Cu2Ba2Hg1 has 5 elements (O,Ca,Cu,Ba,Hg) -> NOT family B.
- O4.xxx Cu1Ba2Hg1 (O,Cu,Ba,Hg = 4 elements) -> family B. 12 members (lines 13-24).
- O6Cu1Ba2Hg1Tl1 = 5 elements -> NOT family B.
threshold1 = 95.0. Family-B members (O4.xxx Cu1Ba2Hg1), measured Tc:
- O4.047 = NULL (no sample; in unmeasurable)
- O4.081 = 98.0   >= 95 -> Q1
- O4.085 = 92.0   < 95
- O4.096 = 95.6   >= 95 -> Q1
- O4.097 = 95.3   >= 95 -> Q1
- O4.101 = 93.4   < 95
- O4.103 = 92.5   < 95
- O4.150 = 97.0   >= 95 -> Q1
- O4.162 = 97.4   >= 95 -> Q1
- O4.180 = NULL (no sample; in unmeasurable)
- O4.200 = 94.2   < 95
- O4.220 = 97.0   >= 95 -> Q1
Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} Cu1Ba2Hg1 (6 members).
Budget after B: 20 used. Hg-1201 phase sits 92-98 K (real HgBa2CuO4 ~ 94-97).

## Step C — extrapolate to unmeasurable

### Null probes of remaining unmeasurable (all free, 6 calls)
O4.066Cu1Ba2Hg1, O4.160Cu1Ba2Hg1, O8Ca0.850Cu2.200Ba2Tl1.800,
O7Ca1Cu2Sr2Tl0.500Pb0.500, O8Cu3Y1Ba1.900La0.100, O8Cu4Y1Ba1.870La0.130
-> all "no sample". Budget still 20. (All 10 unmeasurable now confirmed unsynthesized.)

### Measured analogs (aid rule induction), budget 25/34
- O6Cu1Ba2Hg1Tl1        (1 Cu, no Ca, M-site Hg1+Tl1=2 over-occupied) = 0.0   LOW
- O6Cu1Cd0.250Ba2Tl1.750 (1 Cu, no Ca, M-site Tl1.75+Cd0.25=2 over-occ) = 88.0 LOW
- O10Ca2Cu3Ba2Tl2        (3 Cu, Ca2, Ba2, Tl2 = Tl-2223)              = 118.97 HIGH
- O7Cu3Ba2La1            (123 with La on Y-site, no Y)                = 89.6  LOW
- O7Cu3Y0.200Ba2La0.800  (degraded 123, Y0.2, La on Ba)              = 81.0  LOW

### All measured high/low summary (high = Tc>=90)
HIGH: all Hg-1212 (2Cu/Ca/Ba/Hg, 92-127); all measured Hg-1201 (1Cu/Ba/Hg, O4.08-4.22, 92-98); Tl-2223 (3Cu/Ca2/Ba/Tl2, 119).
LOW: over-occupied M-layer single-Cu (Hg+Tl=0K, Tl+Cd=88K); La-degraded 123 (89.6, 81.0).
Note: Hg-1201 Tc vs O is SCATTERED (92-98, no monotonic trend) -> phase is robustly high across measured O range.

### Multi-factor rule (interaction, NOT single-factor)
High Tc requires BOTH (a) matching a high-Tc cuprate STRUCTURE TYPE AND (b) its proper cations / near-ideal site stoichiometry:
  Hg-1201 (Hg,Ba,Cu) ; Hg-1212/1223 (Hg,Ba,Ca,Cu_n) ; Tl-2212/2223 (Ba,Ca,Tl,Cu) ; Y-123 (Y,Ba,Cu3).
LOW when an interacting factor is violated:
  - over-occupied Hg/Tl layer (Hg+Tl or Tl+Cd summing to ~2 in a single-layer phase) -> 0/88 K
  - heavy La substitution on the Y or Ba site of the 123 phase -> 81-89.6 K
  - Ba replaced by Sr in the 2212 structure (Sr-based (Tl,Pb)-2212 ~80 K)
  - Y-124 structure (4 Cu layers, ~80 K)
Single-factor fails: "contains Hg" wrong on #7(Tl-2212 high) & #9(Y-123 high); "5 elements" wrong on #3-6(4-elem high) & #10(5-elem low). Confirms multi-factor needed.

### Predictions for the 10 unmeasurable
1. O6.160Ca1Cu2Ba2Hg1   -> high  (Hg-1212 ideal; neighbors 105/122; ~113K)
2. O6.270Ca1Cu2Ba2Hg1   -> high  (Hg-1212 ideal; neighbors 123/100; ~113K)
3. O4.180Cu1Ba2Hg1      -> high  (Hg-1201 ideal; neighbors 97.4/94.2; ~96K)
4. O4.047Cu1Ba2Hg1      -> high  (Hg-1201 phase, all 10 measured members high 92-98; no measured low point; low-O scatter not predictive)
5. O4.066Cu1Ba2Hg1      -> high  (Hg-1201 phase, same reasoning)
6. O4.160Cu1Ba2Hg1      -> high  (Hg-1201 ideal; neighbors 97.0/97.4; ~97K)
7. O8Ca0.850Cu2.200Ba2Tl1.800 -> high  (Ba-based Tl-2212, 2Cu/Ca spacer; real Tl-2212~108; light doping)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500 -> low   (Sr-based, NOT Ba; (Tl,Pb)Sr2Ca1Cu2 ~80K real)
9. O8Cu3Y1Ba1.900La0.100 -> high  (near-ideal Y-123: Y1, Ba1.9+La0.1, Cu3; real ~92K)
10. O8Cu4Y1Ba1.870La0.130 -> low   (Y-124, 4 Cu layers; real YBa2Cu4O8 ~80K)

## Step D — final pick
Predicted-high with fewest elements: 4-element Hg-1201 candidates {#3 O4.180, #4 O4.047, #5 O4.066, #6 O4.160}.
Tie-break higher expected Tc:
  #6 O4.160 ~97.2 (between 97.0 & 97.4) <-- HIGHEST
  #3 O4.180 ~96   (between 97.4 & 94.2)
  #5 O4.066 ~95   (phase band, low end)
  #4 O4.047 ~92-95(phase band, low end)
FINAL = O4.160Cu1Ba2Hg1 (robust: wins tie-break regardless of #4/#5 high/low status).

## Final answers
- peak = O6.210Ca1Cu2Ba2Hg1 (tie with O6.220 at 127)
- peak_tc (T*) = 127.0
- threshold1 = 95.0
- Q1 = {O4.081, O4.096, O4.097, O4.150, O4.162, O4.220} Cu1Ba2Hg1  (6 members, all >=95)
- predictions: 10 entries (see above)
- final = O4.160Cu1Ba2Hg1
