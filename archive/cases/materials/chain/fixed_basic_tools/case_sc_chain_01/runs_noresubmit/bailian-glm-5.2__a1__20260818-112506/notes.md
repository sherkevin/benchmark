# Superconductor Tc study — running notes

## Task structure
- Seed: `O6.080Ca1Cu2Ba2Hg1` (Hg-1212 family: elements O,Ca,Cu,Ba,Hg)
- High = Tc >= 90 K, else low.
- Budget: 18 measure_tc calls.

## Step A — series peak
Series = same element set as seed {O,Ca,Cu,Ba,Hg}, varying O. These are the 10
`O6.xCa1Cu2Ba2Hg1` members in measurable.txt (O6.080..O6.310). Find MAX Tc = T*.

## Step B — filter family B (contains Hg AND exactly 4 elements)
threshold1 = T* - 32. Candidates = materials with Hg and exactly 4 elements.
From measurable.txt these are the 11 `O4.xCu1Ba2Hg1` (Hg-1201) members
(elements O,Cu,Ba,Hg). Report all with Tc >= threshold1 = Q1.
(Also unmeasurable O4.180Cu1Ba2Hg1 fits the family but can't be measured.)

## Step C — extrapolate (unmeasurable.txt, no sample)
Induce multi-factor high/low rule from measured materials, predict for all 7
unmeasurable compositions.

## Step D — final pick
Among unmeasurable predicted HIGH, return fewest elements (tie → higher Tc).

## Budget accounting
- Step A: 10 calls (full Hg-1212 dome; also need it to interpolate
  unmeasurable #1 O6.160 and #2 O6.270 which lie inside the series range).
- Remaining: 8 calls for Step B (Hg-1201) + Step C (non-Hg analogs).
- Non-Hg unmeasurable (#4 Tl-2212ish, #5 Tl/Pb/Sr-1212, #6 Y-123ish, #7 Y-124ish)
  have no Hg analog in measured families → need to measure a few non-Hg
  analogs from measurable.txt to anchor a general rule.

## Measurable.txt inventory (26 lines)
- Hg-1212 series (5 elem): 10 members O6.080..O6.310  [Step A]
- Hg-1201 (4 elem, has Hg): 11 members O4.047..O4.220 [Step B candidates]
- Other (5): O6Cu1Ba2Hg1Tl1, O10Ca2Cu3Ba2Tl2, O6Cu1Cd0.250Ba2Tl1.750,
  O7Cu3Ba2La1, O7Cu3Y0.200Ba2La0.800

## Unmeasurable.txt (7)
1. O6.160Ca1Cu2Ba2Hg1  (Hg-1212 interp, between 6.140 & 6.190)
2. O6.270Ca1Cu2Ba2Hg1  (Hg-1212 interp, between 6.240 & 6.300)
3. O4.180Cu1Ba2Hg1     (Hg-1201 interp)
4. O8Ca0.850Cu2.200Ba2Tl1.800  (Tl-2212-ish, 5 elem)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  (Tl/Pb/Sr-1212, 6 elem)
6. O8Cu3Y1Ba1.900La0.100      (Y-123-ish, Cu3, 5 elem)
7. O8Cu4Y1Ba1.870La0.130      (Y-124-ish, Cu4, 5 elem)

## Measurement log (18/18 used)

### Step A — Hg-1212 dome (Ca1Cu2Ba2Hg1; O6.xxx) [10 calls]
- O6.080 → 92.0
- O6.100 → 92.5
- O6.130 → 102.0
- O6.140 → 105.0
- O6.190 → 122.0
- O6.210 → 127.0  ← MAX (tie)
- O6.220 → 127.0  ← MAX (tie)
- O6.240 → 123.0
- O6.300 → 100.0
- O6.310 → 98.0
**T* = 127.0** (plateau at O6.210/O6.220). threshold1 = 127 − 32 = **95.0**.
Peak composition reported: O6.210Ca1Cu2Ba2Hg1 (first occurrence of the max).

### Step B — Hg-1201 (Cu1Ba2Hg1; O4.xxx, 4 elements, contains Hg) [5 calls]
Sampled 5 of 11 (budget). Dome:
- O4.047 → 95.0   (>=95 → Q1)
- O4.085 → 92.0   (<95)
- O4.103 → 92.5   (<95)
- O4.150 → 97.0   (>=95 → Q1)
- O4.220 → 97.0   (>=95 → Q1)
**Q1 (measured qualifiers, Tc>=95.0) = {O4.047, O4.150, O4.220}**
Note: O4.162 and O4.200 lie on the flat 97-K plateau between measured
O4.150(97) and O4.220(97) and almost certainly also qualify, but were NOT
measured (budget). Per the rule "never guess a measured number," they are
NOT cited in Q1. The dip members O4.081/096/097/101 are ~92 (below 95).

### Step C — anchors for the multi-factor rule [3 calls]
- O10Ca2Cu3Ba2Tl2 (Tl-2223: Tl bilayer, 3 CuO2, Ca, Ba) → **118.97 HIGH**
- O7Cu3Ba2La1 (La-123: 3 Cu, La spacer, NO Tl/Hg reservoir) → **89.6 LOW** (borderline)
- O6Cu1Ba2Hg1Tl1 (mixed Hg+Tl 1201, no Ca, O6) → **0.0 LOW** (broken)

## Induced multi-factor rule (Step C)
HIGH requires BOTH:
(a) an optimal, UNMIXED hole-doping charge-reservoir layer — a pure Hg-O
    monolayer (Hg-1201/1212), a near-stoichiometric Tl-O bilayer (Tl-22xx),
    or the YBa2Cu3O7 Cu-O chain block (123);
AND (b) canonical structural optimality — Ba (not Sr) rocksalt layers, a Y
    or Ca spacer (not La) between CuO2 planes, and for the chain-block family
    the 3-Cu '123' stoichiometry (not the 4-Cu '124' structure).
LOW: mixed/depleted reservoirs (Hg+Tl → 0; Tl0.5+Pb0.5), Sr substitution, a
La-spacer 123 (measured La-123 = 89.6), or the 124 (Cu4) structure.

### Rule check vs measured
- Hg-1212 (pure Hg, Ba, Ca, Cu2): HIGH ✓ (92–127)
- Hg-1201 (pure Hg, Ba, Cu1): HIGH ✓ (92–97)
- Tl-2223 (Tl2 bilayer, Ba, Ca2, Cu3): HIGH ✓ (118.97)
- La-123 (123 chain block BUT La spacer not Y): LOW ✓ (89.6)
- Hg/Tl-1201 (MIXED Hg+Tl reservoir, no Ca): LOW ✓ (0.0)

### Step C predictions for unmeasurable.txt (7)
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212, interp between 6.140(105)&6.190(122)≈113 → **high**
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212, interp between 6.240(123)&6.300(100)≈110 → **high**
3. O4.180Cu1Ba2Hg1    — Hg-1201, interp on 97 plateau (4.150–4.220) → **high**
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-2212 (Tl≈bilayer, Ca, Ba, Cu2) → **high**
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  — mixed Tl/Pb reservoir + Sr (not Ba) → **low**
6. O8Cu3Y1Ba1.900La0.100      — Y-123 (Y spacer, 123/Cu3 chain block) → **high**
7. O8Cu4Y1Ba1.870La0.130      — Y-124 (4-Cu/124 structure) → **low**

High = {#1,#2,#3,#4,#6}; Low = {#5,#7}.
This breaks every single-factor rule:
- "contains Hg": wrong on #4 (Tl-2212 high) and #6 (Y-123 high, no Hg)
- "has 5 elements": wrong on #3 (4 elem, high) and #7 (5 elem, low)
- "contains Ca": wrong on #3 (no Ca, high) and #6 (no Ca, high); #5 has Ca but low
- "contains Tl": wrong on #5 (has Tl but low)

## Step D — final pick
Among predicted HIGH {#1(5e), #2(5e), #3(4e), #4(5e), #6(5e)}:
fewest elements = #3 (O,Cu,Ba,Hg = 4 elements). No other HIGH has <5 elements,
and no tie. **final = O4.180Cu1Ba2Hg1** (expected Tc ≈ 97 K).
