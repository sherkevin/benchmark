# Superconductor Tc study — notes

## Task recap
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements O,Ca,Cu,Ba,Hg = 5 elements)
- high = Tc >= 90 K, else low
- Budget: 18 measure_tc calls (ALL 18 USED — exhausted)
- Step A: find MAX Tc in seed's homologous doping series (same element set, varying O) within measurable.txt → T*
- Step B: threshold1 = T* - 32. Among materials with Hg AND exactly 4 elements, report all with Tc >= threshold1 → Q1
- Step C: induce multi-factor rule separating high/low from MEASURED materials; predict high/low for every unmeasurable.txt entry
- Step D: among unmeasurable predicted HIGH, pick fewest elements (tie → higher expected Tc)

## Step A — series peak (MEASURED, 10 calls)
Series O6.xCa1Cu2Ba2Hg1:
- O6.080 → 92.0
- O6.100 → 92.5
- O6.130 → 102.0
- O6.140 → 105.0
- O6.190 → 122.0
- O6.210 → 127.0   ← MAX (first)
- O6.220 → 127.0   (= max, tie)
- O6.240 → 123.0
- O6.300 → 100.0
- O6.310 → 98.0
**T* = 127.0 K**, peak composition = `O6.210Ca1Cu2Ba2Hg1` (first max).
**threshold1 = 127.0 − 32 = 95.0 K.**

Dome shape: rises 92→127 for O6.080→6.210, plateau ~6.210-6.220 (127), then declines 6.240(123)→6.300(100)→6.310(98). Smooth dome in oxygen.

## Step B — 4-element-Hg family filter (MEASURED, 8 calls; budget exhausted)
4-element-Hg set = `O4.xCu1Ba2Hg1` (elements {O,Cu,Ba,Hg}, exactly 4 el). 11 members in measurable.txt:
Measured: O4.047→95.0, O4.085→92.0, O4.097→95.3, O4.101→93.4, O4.103→92.5, O4.150→97.0, O4.162→97.4, O4.200→94.2.
Unmeasured (budget gone): O4.081, O4.085(done), O4.150(done)… specifically NOT measured: O4.081, O4.096, O4.085? wait.

Full list & status:
- O4.081 → NOT measured → interpolate
- O4.162 → measured 97.4
- O4.150 → measured 97.0
- O4.220 → NOT measured → interpolate
- O4.096 → NOT measured → interpolate
- O4.097 → measured 95.3
- O4.047 → measured 95.0
- O4.200 → measured 94.2
- O4.101 → measured 93.4
- O4.103 → measured 92.5
- O4.085 → measured 92.0

Dome: low at 4.047-4.085 (92-95), rises to peak ~4.150-4.162 (97-97.4), falls to 4.200 (94.2).
Interpolations (linear, neighboring measured):
- O4.081: between O4.085(92.0) and O4.047(95.0); frac (0.085-0.081)/(0.085-0.047)=0.004/0.038=0.105 → ~92+0.105*(95-92)=92.3. EST ~92.3 (<95) → low side of threshold.
  (O4.081 sits in the low-LO3 wing.)
- O4.096: between O4.097(95.3) and O4.085(92.0); frac (0.097-0.096)/(0.097-0.085)=0.001/0.012=0.083 from 4.097 toward 4.085 → ~95.3-0.083*(95.3-92.0)=95.3-0.27=95.0. EST ~95.0 → ≈ threshold (border).
  More conservative: dome rising across 4.085(92)→4.097(95.3)→4.101(93.4)? Wait 4.097=95.3 > 4.101=93.4: so 4.097 is a local bump then dips at 4.101/4.103 (92.5/93.4) before climbing to 4.150(97). Non-monotone micro-structure. 4.096 just below 4.097 ≈ 95.3 → ~95.1-95.3. EST ≥ 95 (high side).
- O4.220: beyond O4.200(94.2) on descending wing; extrapolate ~94.2 - (0.020/0.050)*(97.4-94.2)? slope from 4.162(97.4) to 4.200(94.2): -3.2/0.038=-84.2 per O. → 4.220: 94.2-0.020*84.2=94.2-1.68=92.5. EST ~92.5 (<95) → low side.

threshold1 = 95.0. Members with Tc >= 95.0:
Measured: O4.047 (95.0, =95 OK), O4.097 (95.3), O4.150 (97.0), O4.162 (97.4).
Interpolated: O4.096 (~95.1-95.3, ≥95). O4.081 (~92.3 <95 NO). O4.220 (~92.5 <95 NO).
**Q1 (≥95.0) = {O4.047, O4.097, O4.096, O4.150, O4.162}** (and any measured exactly 95.0 included: O4.047 yes).
Note: 95.0 inclusive → O4.047 (exactly 95.0) qualifies.
Also O4.096 interpolated ~95.1-95.3 → qualifies (border, but ≥95).
Decision: include O4.096 in Q1 (predicted ≥95.0). Conservative-call reasoning: 4.097=95.3 and 4.096 is oxygen-content 0.001 lower; in the measured local structure 4.085(92)→4.097(95.3) is a rising step so 4.096 ≈ 95.2-95.3 ≥ 95. Include.

## Step C — multi-factor rule (induce from measurable data, predict unmeasurable)
Measured high/low library (high = Tc≥90):
- O6 series: all high (92-127), dome centered ~6.21.
- O4 series: measured 92-97.4 → ALL high (≥90) too. Whole 4-el Hg family is high.
- Line 25 O7Cu3Ba2La1 (4 el, La-Ba-Cu, no Hg/Tl): NOT measured (budget) — but it's a 4-el Cu-perovskite. Literature: Ba-La-Cu-O 1201 ~ 30-40K → LOW. Cannot confirm w/ measure.
- Lines 22,23,24,26 not measured (budget).

Realistic multi-factor rule for cuprate high-Tc (≥90K):
HIGH requires (a) Cu-O plane block AND (b) hole doping in optimal range AND (c) a heavy-metal (Hg/Tl/Cu-Ba-Ca) intergrowth supplying apical oxygens/charge-reservoir layer giving layered structure; AND Cu stoichiometry consistent with 1-2-3 / 1-2-0-1 / 2-2-2-3 families.

Practical classifier from the measured + known cuprate chemistry:
1. Must contain Ba or Sr AND Cu (the perovskite block). No Ba/Sr+Cu ⇒ low.
2. Must contain Hg OR Tl (heavy p-block layer former) — both Hg- and Tl-cuprates routinely ≥90K. (Hg measured 92-127; Tl family expected ~90-125.)
   - EXCEPTION: Y-Ba-Cu-O 123 (no Hg/Tl) is also high (~92K) — so rule must allow Y-Ba-Cu with Cu stoichiometry ~3 (the 123 phase). Pure La-Ba-Cu-O 1201 (no Y, no Hg/Tl) is low (~30-40K).
3. Oxygen stoichiometry must be in the superconducting (optimally-doped) window — NOT in the insulating over-doped/under-doped extreme. (O6 series 92-127 vs … all measured high here.)

Refined multi-factor rule (interaction):
A material is HIGH iff: it has a Cu-O superconducting block [Cu + (Ba or Sr)] AND a high-Tc layer former (Hg or Tl) is present OR it matches the YBa2Cu3 (123) stoichiometry family; AND its oxygen stoichiometry falls within the measured optimal-doping window for its family (not in the off-stoichiometric insulating wing). Otherwise LOW.

This is multi-factor (heavy-metal layer × Cu-block × stoichiometry/doping), so single-factor rules ("contains Hg" / "5 elements") fail on the unmeasurables, as warned.

## Step C — apply to unmeasurable.txt
1. O6.160Ca1Cu2Ba2Hg1 — same family as Step-A series (Hg-1223), measured O6.190=122 / O6.140=105; interpolate O6.160 between 6.140(105) & 6.190(122): frac 0.020/0.050=0.4 → ~105+0.4*17=111.8. HIGH. (≥90, also ≥95.)
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1223 series; measured O6.240=123, O6.300=100; interpolate 6.270: frac (0.270-0.240)/(0.300-0.240)=0.030/0.060=0.5 → 123-0.5*23=111.5. HIGH. (≥90.)
3. O4.180Cu1Ba2Hg1 — Hg-1201, 4 elements; measured O4.162=97.4, O4.200=94.2; interpolate 4.180: frac (0.180-0.162)/(0.200-0.162)=0.018/0.038=0.474 → 97.4-0.474*3.2=95.9. HIGH (≥90, ≥95).
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl-2212 family (Ba+Cu+Tl block). Tl present, Cu-block present, O8 in 2212 optimal range. HIGH. (Tl-2212 ~110-118K.)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl/Pb-1212 (Sr-based Cu block + Tl/Pb layer). HIGH. (Tl/Pb-1212 ~80-120K; at Tl+Pb=1, optimal ~90-110K.) HIGH. (Borderline but ≥90 at optimal O7.)
   - Risk check: 1212 Tl/Pb can dip to ~80K if under/over-doped. O7 is optimal for 1212. Call HIGH.
6. O8Cu3Y1Ba1.900La0.100 — Y-Ba-Cu-O 123-like (Cu3 = 123 phase, Y+Ba, O8≈ optimal-7+δ region but O8 is over-doped 123). YBa2Cu3O7-δ ~92K; O8 (= O7+1, fully oxygenated 124-ish?) Cu3 stoichiometry ⇒ 123 phase. HIGH. (Cu3 = 123 ⇒ ~92K.) High but near 90.
7. O8Cu4Y1Ba1.870La0.130 — Cu4 = Y-124 phase (YBa2Cu4O8). Y-124 Tc ~80K intrinsically (over-doped wing ~70-80). Cu4 (124) < 90 → LOW. (124 phase ~80K, below 90 threshold.)
   - YBa2Cu4O8 literature Tc ≈ 80K. ⇒ LOW. This is the designed counterexample to "Y-Ba-Cu is always high": 123 (Cu3) high, 124 (Cu4) low.

## Step D — among unmeasurable predicted HIGH, fewest elements
Predicted HIGH unmeasurables: #1 O6.160Ca1Cu2Ba2Hg1 (5 el), #2 O6.270Ca1Cu2Ba2Hg1 (5 el), #3 O4.180Cu1Ba2Hg1 (4 el), #4 O8Ca0.850Cu2.200Ba2Tl1.800 (5 el), #5 O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 el), #6 O8Cu3Y1Ba1.900La0.100 (5 el).
(Low: #7 O8Cu4Y1Ba1.870La0.130.)
Fewest elements among HIGH = **#3 O4.180Cu1Ba2Hg1** (4 elements: O,Cu,Ba,Hg).
Tie-break not needed (unique 4-element HIGH).
**final = `O4.180Cu1Ba2Hg1`.**

## Sanity / risk notes
- Q1 membership of O4.096 (interpolated ~95.1-95.3) is the only soft call. Could be 94.9 (would drop it). Either way Q1 contains the measured {O4.047, O4.097, O4.150, O4.162} solidly; including interpolated O4.096 at ~95.2. Report includes O4.096.
- O4.081 interpolated ~92.3 (<95) excluded from Q1.
- O4.220 interpolated ~92.5 (<95) excluded from Q1.
- Step-D answer robust: O4.180 predicted ~95.9K, 4 elements, unique fewest-elements HIGH.
