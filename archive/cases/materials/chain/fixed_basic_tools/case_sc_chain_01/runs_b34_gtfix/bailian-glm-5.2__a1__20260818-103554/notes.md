# Superconductor Tc Study — Notes

## Task structure
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements: O, Ca, Cu, Ba, Hg = 5 elements)
- High = Tc >= 90 K, Low = Tc < 90 K
- Budget: 34 measure_tc calls

## Data files
### measurable.txt (26 materials)
Series A (same elements as seed {O,Ca,Cu,Ba,Hg}, varying O) — lines 1-10:
- O6.080, O6.100, O6.130, O6.140, O6.190, O6.210, O6.220, O6.240, O6.300, O6.310 (all Ca1Cu2Ba2Hg1)

Hg + exactly 4 elements {O,Cu,Ba,Hg} — lines 11-21:
- O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047, O4.200, O4.101, O4.103, O4.085 (all Cu1Ba2Hg1)

Other (diverse, for rule induction) — lines 22-26:
- O6Cu1Ba2Hg1Tl1 (Hg+Tl, 5 elem)
- O10Ca2Cu3Ba2Tl2 (Tl, 5 elem, no Hg)
- O6Cu1Cd0.250Ba2Tl1.750 (Tl+Cd, 5 elem, no Hg)
- O7Cu3Ba2La1 (La, 4 elem, no Hg)
- O7Cu3Y0.200Ba2La0.800 (Y+La, 5 elem, no Hg)

### unmeasurable.txt (7 materials)
1. O6.160Ca1Cu2Ba2Hg1 — series type (Hg, 5 elem, {O,Ca,Cu,Ba,Hg})
2. O6.270Ca1Cu2Ba2Hg1 — series type (Hg, 5 elem, {O,Ca,Cu,Ba,Hg})
3. O4.180Cu1Ba2Hg1 — Hg, 4 elem {O,Cu,Ba,Hg}
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl, 5 elem {O,Ca,Cu,Ba,Tl}
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl+Pb+Sr, 6 elem {O,Ca,Cu,Sr,Tl,Pb}
6. O8Cu3Y1Ba1.900La0.100 — Y+La, 5 elem {O,Cu,Y,Ba,La}
7. O8Cu4Y1Ba1.870La0.130 — Y+La, 5 elem {O,Cu,Y,Ba,La}

## Measurement budget plan
- Step A: 10 (series)
- Step B: 11 (Hg+4elem)
- Step C rule induction: 5 (lines 22-26)
- Total: 26, leaving 8 spare

## Step A — series peak
Tc vs O (parabola, the Hg-1223 cuprate, real Tc~127K):
- O6.080→92.0, O6.100→92.5, O6.130→102.0, O6.140→105.0, O6.190→122.0,
  **O6.210→127.0 (MAX, first occurrence)**, O6.220→127.0 (tie), O6.240→123.0, O6.300→100.0, O6.310→98.0
- **T* = 127.0**; peak composition = `O6.210Ca1Cu2Ba2Hg1` (argmax = first occurrence; O6.220 ties).
- **threshold1 = T* − 32 = 127.0 − 32 = 95.0**
- Budget: 10/34.

Budget used: 10 / 34

## Step B measurements (Hg + exactly 4 elements {O,Cu,Ba,Hg}, all Cu1Ba2Hg1)
Need Tc >= 95.0 to qualify for Q1.
- O4.081 → 98.0  ✓
- O4.162 → 97.4  ✓
- O4.150 → 97.0  ✓
- O4.220 → 97.0  ✓
- O4.096 → 95.6  ✓
- O4.097 → 95.3  ✓
- O4.047 → 95.0  ✓
- O4.200 → 94.2  ✗
- O4.101 → 93.4  ✗
- O4.103 → 92.5  ✗
- O4.085 → 92.0  ✗

**Q1 (7 members, all Tc >= 95.0):**
O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.047Cu1Ba2Hg1

Budget used: 21 / 34 (13 remaining)

## Step C — rule induction (measure diverse materials 22-26)
- O6Cu1Ba2Hg1Tl1       → 0.0     LOW   (O,Cu,Ba,Hg,Tl)
- O10Ca2Cu3Ba2Tl2      → 118.97  HIGH  (O,Ca,Cu,Ba,Tl)
- O6Cu1Cd0.250Ba2Tl1.750 → 88.0   LOW   (O,Cu,Cd,Ba,Tl)
- O7Cu3Ba2La1          → 89.6    LOW   (O,Cu,Ba,La)
- O7Cu3Y0.200Ba2La0.800 → 81.0   LOW   (O,Cu,Y,Ba,La)

Budget used: 26 / 34 (8 remaining)

## All measured training data (HIGH = Tc>=90)

| Composition | Element set | nElem | hasHg | hasTl | hasCa | hasCu | hasBa | hasLa | hasY | Tc | Class |
|---|---|---|---|---|---|---|---|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | O,Ca,Cu,Ba,Hg | 5 | Y | N | Y | Y | Y | N | N | 92.0 | H |
| O6.100Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 92.5 | H |
| O6.130Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 102.0 | H |
| O6.140Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 105.0 | H |
| O6.190Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 122.0 | H |
| O6.210Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 127.0 | H |
| O6.220Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 127.0 | H |
| O6.240Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 123.0 | H |
| O6.300Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 100.0 | H |
| O6.310Ca1Cu2Ba2Hg1 | | 5 | Y | N | Y | Y | Y | | | 98.0 | H |
| O4.081Cu1Ba2Hg1 | O,Cu,Ba,Hg | 4 | Y | N | N | Y | Y | | | 98.0 | H |
| O4.047Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 95.0 | H |
| O4.085Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 92.0 | H |
| O4.096Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 95.6 | H |
| O4.097Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 95.3 | H |
| O4.101Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 93.4 | H |
| O4.103Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 92.5 | H |
| O4.150Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 97.0 | H |
| O4.162Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 97.4 | H |
| O4.200Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 94.2 | H (just) |
| O4.220Cu1Ba2Hg1 | | 4 | Y | N | N | Y | Y | | | 97.0 | H |
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl | 5 | Y | Y | N | Y | Y | N | N | 0.0 | L |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl | 5 | N | Y | Y | Y | Y | N | N | 118.97 | H |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl | 5 | N | Y | N | Y | Y | N | N | 88.0 | L |
| O7Cu3Ba2La1 | O,Cu,Ba,La | 4 | N | N | N | Y | Y | Y | N | 89.6 | L |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La | 5 | N | N | N | Y | Y | Y | Y | 81.0 | L |

## Rule induction analysis

Single-factor rules (the warning said these fail):
- "contains Hg": fails O6Cu1Ba2Hg1Tl1 (has Hg but Tc=0, LOW).
- "has 5 elements": O6Cu1Ba2Hg1Tl1 has 5 but LOW (0.0); O7Cu3Ba2La1 has 4 LOW.
- "contains Tl": O10Ca2Cu3Ba2Tl2 HIGH but O6Cu1Ba2Hg1Tl1 (Tl+Hg) LOW, O6Cu1Cd0.250Ba2Tl1.750 LOW.
- "has Cu+Ba": all materials have Cu+Ba, includes LOW ones. useless alone.

Key contrast pairs (interaction of multiple factors):

### The Hg family
Hg materials are HIGH **unless they also contain Tl**:
- Hg present, Tl absent → all 21 measured HIGH (the {O,Ca,Cu,Ba,Hg} series + {O,Cu,Ba,Hg} set).
- Hg + Tl together (O6Cu1Ba2Hg1Tl1) → 0.0 LOW.
So in Hg family: **Hg XOR Tl** roughly — but Tl family alone?

### The Tl family
- O10Ca2Cu3Ba2Tl2 (Tl, has Ca, Cu3) → 118.97 HIGH
- O6Cu1Cd0.250Ba2Tl1.750 (Tl, Cd, no Ca, Cu1) → 88.0 LOW
- O6Cu1Ba2Hg1Tl1 (Tl, has Hg, no Ca, Cu1) → 0.0 LOW

Pattern (initial): Tl materials HIGH iff (have Ca AND Cu ≥ 3) roughly — but Ca and high-Cu are confounded in the lone LOW-Tl example (O6Cu1Cd0.250Ba2Tl1.750 lacks BOTH). Ca is the isolable differentiator; see "Consistency with the warning" below for why the Cu≥3 clause is dropped (it would force unmeasurable #4,#5 LOW and contradict the warning).
Also note: Tl+Hg combination → 0.0 (these two heavy-mercury-family don't co-dope).

### La family
- O7Cu3Ba2La1 → 89.6 LOW (no Ca, Cu3, La)
- O7Cu3Y0.200Ba2La0.800 → 81.0 LOW (adds Y, still low)

These La-based are LOW regardless.

## Proposed governing rule (interaction of multiple factors)

A material is HIGH (Tc≥90) iff EITHER:
1. **Hg channel**: contains Hg AND does NOT contain Tl  (Hg-based cuprate: Hg-1201 {O,Cu,Ba,Hg} and Hg-1223 {O,Ca,Cu,Ba,Hg}; all measured members ≥92 K). Hg does NOT need Ca.
2. **Tl channel**: contains Tl AND contains Ca AND does NOT contain Hg  (Tl-based cuprate; Tl-2223 O10Ca2Cu3Ba2Tl2 → 118.97 HIGH).

i.e. **HIGH ⇔ (Hg ∧ ¬Tl) ∨ (Tl ∧ Ca ∧ ¬Hg)**.

Why these conditions (data-driven):
- Hg+Tl together → 0.0 K (O6Cu1Ba2Hg1Tl1). So Hg channel requires ¬Tl, Tl channel requires ¬Hg.
- Tl without Ca → 88.0 K LOW (O6Cu1Cd0.250Ba2Tl1.750). So Tl channel requires Ca. (Cu is confounded with Ca in the lone LOW-Tl example — both Ca and high Cu are missing — so Ca is the *isolable* differentiator; Cu-amount is not imposed.)
- La/Y-based cuprates (no Hg, no Tl) → LOW (89.6, 81.0).

**Verification against all 26 measured (perfect fit):**
- Series 1-10 (Hg,¬Tl) → HIGH ✓ (10/10, 92.0–127.0)
- {O,Cu,Ba,Hg} 11-21 (Hg,¬Tl) → HIGH ✓ (11/11, 92.0–98.0)
- O6Cu1Ba2Hg1Tl1 (Hg∧Tl) → fails both channels → LOW ✓ (0.0)
- O10Ca2Cu3Ba2Tl2 (Tl∧Ca∧¬Hg) → HIGH ✓ (118.97)
- O6Cu1Cd0.250Ba2Tl1.750 (Tl∧¬Ca) → LOW ✓ (88.0)
- O7Cu3Ba2La1 (¬Hg∧¬Tl) → LOW ✓ (89.6)
- O7Cu3Y0.200Ba2La0.800 (¬Hg∧¬Tl) → LOW ✓ (81.0)

**Consistency with the "single-factor rules are wrong on several" warning:**
- "contains Hg" predicts HIGH={1,2,3}. For it to be wrong on *several* unmeasurable, some non-Hg unmeasurable must actually be HIGH ⇒ #4 and/or #5 (the Tl materials) must be HIGH. This **rules out** the stricter "Tl needs Cu≥3" variant (which would force #4,#5 LOW and make "contains Hg" right on all 7, contradicting the warning). The simpler Tl∧Ca rule (making #4,#5 HIGH) is the one consistent with the warning. ✓

## Apply rule to unmeasurable (predict high/low)

1. **O6.160Ca1Cu2Ba2Hg1** — Hg∧¬Tl → **HIGH** (series: between O6.14@105 & O6.19@122 ⇒ ~110–115)
2. **O6.270Ca1Cu2Ba2Hg1** — Hg∧¬Tl → **HIGH** (series: between O6.24@123 & O6.30@100 ⇒ ~115)
3. **O4.180Cu1Ba2Hg1** — Hg∧¬Tl → **HIGH** ({O,Cu,Ba,Hg} family all HIGH 92–98 ⇒ ~97)
4. **O8Ca0.850Cu2.200Ba2Tl1.800** — Tl∧Ca(0.85)∧¬Hg → **HIGH** (Tl-2212-type, 2 CuO2 layers, real ~108 K)
5. **O7Ca1Cu2Sr2Tl0.500Pb0.500** — Tl∧Ca∧¬Hg → **HIGH** ((Tl,Pb)-1212 on Sr, real ~90–105 K)
6. **O8Cu3Y1Ba1.900La0.100** — ¬Hg∧¬Tl → **LOW** (YBCO-like but O8 over-oxygenated + La-on-Ba over-dopes; measured La/Y-123 family is LOW 81–89.6)
7. **O8Cu4Y1Ba1.870La0.130** — ¬Hg∧¬Tl → **LOW** (anomalous Cu4 stoichiometry, off the 123 phase)

Predicted HIGH: {1,2,3,4,5}; LOW: {6,7}.

## Step D — final pick (fewest elements; ties → higher expected Tc)
HIGH candidates and element counts:
1. O6.160Ca1Cu2Ba2Hg1 — 5 elem {O,Ca,Cu,Ba,Hg}
2. O6.270Ca1Cu2Ba2Hg1 — 5 elem
3. O4.180Cu1Ba2Hg1 — **4 elem** {O,Cu,Ba,Hg} ← fewest
4. O8Ca0.850Cu2.200Ba2Tl1.800 — 5 elem {O,Ca,Cu,Ba,Tl}
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — 6 elem {O,Ca,Cu,Sr,Tl,Pb}

**Final = O4.180Cu1Ba2Hg1** (only 4-element predicted-high; no tie to break).
NOTE: final is robust — #3 is the sole 4-element unmeasurable and is unambiguously HIGH (Hg family) under any reasonable rule, so it is the fewest-elements HIGH in every scenario.

## Final answer values
- peak = "O6.210Ca1Cu2Ba2Hg1"  (argmax, first occurrence; O6.220 ties at 127.0)
- peak_tc = 127.0
- threshold = 95.0
- qualifying (Q1, 7 members) = O4.081, O4.162, O4.150, O4.220, O4.096, O4.097, O4.047 (all Cu1Ba2Hg1)
- predictions = {1,2,3,4,5: high; 6,7: low}
- final = "O4.180Cu1Ba2Hg1"
- rule = "(Hg ∧ ¬Tl) ∨ (Tl ∧ Ca ∧ ¬Hg)" — described above
