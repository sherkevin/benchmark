# Superconductor Tc study — running notes

## Setup
Seed `O6.080Ca1Cu2Ba2Hg1`. HIGH = Tc>=90. Budget 18 measure_tc calls.

## Step A — O6.0xx Ca1Cu2Ba2Hg1 series (Hg-1223) — 10 calls (budget 1-10)
| comp | Tc |
|---|---|
| O6.080 | 92.0 |
| O6.100 | 92.5 |
| O6.130 | 102.0 |
| O6.140 | 105.0 |
| O6.190 | 122.0 |
| O6.210 | 127.0 |  <-- MAX (tied) |
| O6.220 | 127.0 |  <-- MAX (tied) |
| O6.240 | 123.0 |
| O6.300 | 100.0 |
| O6.310 | 98.0 |

T* = 127.0 K (peak composition O6.210Ca1Cu2Ba2Hg1; O6.220 ties). All 10 are HIGH.
Dome: rises 92->127 from O6.08 to O6.21, flat top 6.21-6.22, falls to 98 at 6.31.

## Step B — threshold1 = T* - 32 = 127 - 32 = 95.0
Family B = contains Hg AND exactly 4 elements.
Check all measurable:
- O6.0xx Ca1Cu2Ba2Hg1 -> {O,Ca,Cu,Ba,Hg}=5 el -> excluded
- O4.xxx Cu1Ba2Hg1 -> {O,Cu,Ba,Hg}=4 el + Hg -> INCLUDED (11 members)
- O6Cu1Ba2Hg1Tl1 -> {O,Cu,Ba,Hg,Tl}=5 -> excluded
- O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.25Ba2Tl1.75, O7Cu3Ba2La1, O7Cu3Y0.2Ba2La0.8 -> no Hg -> excluded
So family B = the 11 O4.xxxCu1Ba2Hg1 members (Hg-1201).

Measured 8 of 11 (budget 11-18):
| comp | Tc | >=95? |
|---|---|---|
| O4.096 | 95.6 | YES |
| O4.097 | 95.3 | YES |
| O4.101 | 93.4 | no |
| O4.103 | 92.5 | no |
| O4.150 | 97.0 | YES |
| O4.162 | 97.4 | YES |
| O4.200 | 94.2 | no |
| O4.220 | 97.0 | YES |
UNMEASURED (budget exhausted): O4.047, O4.081, O4.085.
  - Underdoped side (low O) trends lower; O4.096/097 are the lowest-O measured (95.6/95.3).
  - O4.047 (very underdoped) -> expected ~85-92, <95. O4.081 -> ~92-94, <95. O4.085 -> ~94-95.5, borderline but expected <95.
  - Cannot cite measured Tc for these (budget spent); exclude from Q1 (no measured value to confirm >=95).

Q1 (Tc>=95, measured-confirmed) = {O4.096, O4.097, O4.150, O4.162, O4.220} (all Cu1Ba2Hg1).

## Step C — induce rule, predict 7 unmeasurable
Unmeasurable.txt:
1. O6.160Ca1Cu2Ba2Hg1  -> Hg-1223; interpolate O6 series between .140(105)/.190(122) -> ~113 K -> HIGH (measured-series interpolation, confident)
2. O6.270Ca1Cu2Ba2Hg1  -> Hg-1223; between .240(123)/.300(100) -> ~115-120 -> HIGH (confident)
3. O4.180Cu1Ba2Hg1     -> Hg-1201; between .162(97.4)/.200(94.2) -> ~96 -> HIGH (confident)
4. O8Ca0.850Cu2.200Ba2Tl1.800 -> Tl-12(n3) family: Tl+Ba+Ca+2-3 CuO2 -> ~100-127 -> HIGH (domain)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500   -> Tl-1212 with Sr2 (NOT Ba2) -> ~80 K -> LOW (domain; Sr compresses lattice)
6. O8Cu3Y1Ba1.900La0.100       -> Y-123 (Cu3, Y, Ba) ~90 K, minor La -> HIGH (domain; Cu3=123 motif)
7. O8Cu4Y1Ba1.870La0.130       -> Y-124 (Cu4 double-chain) ~80 K -> LOW (domain; Cu4=124 motif)

NOTE on data gap: I spent all 18 calls on Step A (10) + Step B O4 family (8). I did NOT measure
the 5 "misc" measurable materials (O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.25Ba2Tl1.75,
O7Cu3Ba2La1, O7Cu3Y0.2Ba2La0.8) which would have calibrated the non-Hg part of the rule.
Predictions #1-3 rest directly on measured series interpolation (strong). #4-7 rest on cuprate
domain knowledge keyed off the composition formulas (medium). The multi-factor rule below is
induced from the measured Hg-1201/Hg-1223 data (confirming Hg+Ba+Cu motif -> high) plus
structural/chemical reasoning for the Tl- and Y-based families.

Multi-factor rule (interaction of factors; single-factor rules fail):
 HIGH (Tc>=90) requires the JOINT presence of:
   (i) a heavy p-block charge-reservoir cation Hg or Tl in the rocksalt block,
   (ii) Ba (not the smaller Sr) as the large alkaline earth,
   (iii) a 1-3 CuO2-plane block in the standard layered motif:
        Hg-1201 (Cu1), Hg/Tl-1223 or 2223 (Cu2-3 with Ca), or YBCO-123 (Cu3 with Y).
 Any single deviation drops Tc below 90:
   - no reservoir cation -> La-Ba-Cu-O (LBCO ~40K) = LOW
   - Sr replacing Ba       -> Tl-1212-Sr (~80K) = LOW
   - Y-124 double chain    -> Cu4 motif (~80K) = LOW
   - La replacing Y on Y-site -> La-123 (~50K) = LOW
 Confirmed by measured Hg-1201 (O4 family 92.5-97.4K, all high) and Hg-1223 (O6 series 92-127K, all high).

## Step D — among predicted HIGH, fewest elements (tie-break higher expected Tc)
HIGH predictions: #1(5el), #2(5el), #3(4el), #4(5el), #6(5el).
Fewest elements = #3 O4.180Cu1Ba2Hg1 ({O,Cu,Ba,Hg}=4). No tie.
final = O4.180Cu1Ba2Hg1 (expected Tc ~96 K, interpolated from O4.162=97.4 / O4.200=94.2).
ROBUST: #3 is HIGH regardless of how #4/#6 resolve (its 4 elements is uniquely minimal among the high set).

## Budget
18/18 used: 10 (Step A) + 8 (Step B O4 family). 0 remaining.
