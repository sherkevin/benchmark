# Notes — superconductor Tc study

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- high = Tc >= 90 K, low otherwise
- Budget: 45 get_property calls; used 25, 20 remaining.
- measurable.txt: 29 compositions; unmeasurable.txt: 10 compositions.
- Note: 4 items appear in BOTH lists (overlap trap): O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1, O4.047Cu1Ba2Hg1 — get_property(tc) returns null for these (genuinely unmeasurable).

## Step A — series peak (Ca1Cu2Ba2Hg1, varying O)
| composition | Tc |
|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | NULL (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** ← peak (tied) |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** ← peak (tied) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | NULL (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T\* = 127.0 K**, peak at O6.210Ca1Cu2Ba2Hg1 (= O6.220). I'll report peak = O6.210Ca1Cu2Ba2Hg1 (first occurrence of the max).

## Step B — filter family B (Hg-containing, exactly 4 elements, Tc >= threshold1)
threshold1 = T* - 32 = 127 - 32 = **95.0 K**

The O4.xx Cu1Ba2Hg1 series: elements = {O, Cu, Ba, Hg} = exactly 4 elements, all contain Hg. Measured Tc:
| composition | Tc | >= 95? |
|---|---|---|
| O4.081Cu1Ba2Hg1 | 98.0 | ✓ |
| O4.162Cu1Ba2Hg1 | 97.4 | ✓ |
| O4.150Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.220Cu1Ba2Hg1 | 97.0 | ✓ |
| O4.096Cu1Ba2Hg1 | 95.6 | ✓ |
| O4.097Cu1Ba2Hg1 | 95.3 | ✓ |
| O4.180Cu1Ba2Hg1 | NULL (unmeasurable) | — (can't include; no measured value) |
| O4.047Cu1Ba2Hg1 | NULL (unmeasurable) | — |
| O4.200Cu1Ba2Hg1 | 94.2 | ✗ |
| O4.101Cu1Ba2Hg1 | 93.4 | ✗ |
| O4.103Cu1Ba2Hg1 | 92.5 | ✗ |
| O4.085Cu1Ba2Hg1 | 92.0 | ✗ |

Q1 (measured Tc >= 95.0, Hg + 4 elements):
- O4.081Cu1Ba2Hg1 (98.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.220Cu1Ba2Hg1 (97.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)

(Note: O4.180 and O4.047 are unmeasurable in this run — no measured Tc, so they can't satisfy the measured-threshold condition for Q1.)

## Other measurable materials (for inducing the rule)
| composition | Tc | high? |
|---|---|---|
| O6Cu1Ba2Hg1Tl1 | 0.0 | low |
| O10Ca2Cu3Ba2Tl2 | 118.97 | high |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | low |
| O7Cu3Ba2La1 | 89.6 | low |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | low |

## Step C — induce the rule (high vs low)

### All measured data table (high/low label, composition features)
Let me tabulate every measured point with element count, presence of Hg, presence of Tl, presence of Cu, presence of Ba, and the Cu count.

| composition | Tc | high? | #elem | Hg? | Tl? | Cu? | Cu# | O# |
|---|---|---|---|---|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | H | 5 | Y | N | Y | 2 | 6.08 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | H | 5 | Y | N | Y | 2 | 6.10 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | H | 5 | Y | N | Y | 2 | 6.13 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | H | 5 | Y | N | Y | 2 | 6.14 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | H | 5 | Y | N | Y | 2 | 6.19 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | H | 5 | Y | N | Y | 2 | 6.21 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | H | 5 | Y | N | Y | 2 | 6.22 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | H | 5 | Y | N | Y | 2 | 6.24 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | H | 5 | Y | N | Y | 2 | 6.30 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | H | 5 | Y | N | Y | 2 | 6.31 |
| O4.081Cu1Ba2Hg1 | 98.0 | H | 4 | Y | N | Y | 1 | 4.081 |
| O4.162Cu1Ba2Hg1 | 97.4 | H | 4 | Y | N | Y | 1 | 4.162 |
| O4.150Cu1Ba2Hg1 | 97.0 | H | 4 | Y | N | Y | 1 | 4.150 |
| O4.220Cu1Ba2Hg1 | 97.0 | H | 4 | Y | N | Y | 1 | 4.220 |
| O4.096Cu1Ba2Hg1 | 95.6 | H | 4 | Y | N | Y | 1 | 4.096 |
| O4.097Cu1Ba2Hg1 | 95.3 | H | 4 | Y | N | Y | 1 | 4.097 |
| O4.200Cu1Ba2Hg1 | 94.2 | L | 4 | Y | N | Y | 1 | 4.200 |
| O4.101Cu1Ba2Hg1 | 93.4 | L | 4 | Y | N | Y | 1 | 4.101 |
| O4.103Cu1Ba2Ba2Hg1 (typo) | 92.5 | L | 4 | Y | N | Y | 1 | 4.103 |
| O4.085Cu1Ba2Hg1 | 92.0 | L | 4 | Y | N | Y | 1 | 4.085 |
| O6Cu1Ba2Hg1Tl1 | 0.0 | L | 5 | Y | Y | Y | 1 | 6 |
| O10Ca2Cu3Ba2Tl2 | 118.97 | H | 5 | N | Y | Y | 3 | 10 |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | L | 5 | N | Y | Y | 1 | 6 |
| O7Cu3Ba2La1 | 89.6 | L | 4 | N | N | Y | 3 | 7 |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | L | 5 | N | N | Y | 3 | 7 |

### Testing candidate one-factor rules (task says these are WRONG)
- "contains Hg": WRONG — O6Cu1Ba2Hg1Tl1 has Hg but Tc=0 (low).
- "has 5 elements": WRONG — O10Ca2Cu3Ba2Tl2 has 5 elements and is high (119), but O7Cu3Y0.200Ba2La0.800 has 5 elements and is low (81). Also O4.xx (4 elem) are high. So element count alone fails.
- "contains Tl": WRONG — O6Cu1Ba2Hg1Tl1 has Tl but is low (0); O10Ca2Cu3Ba2Tl2 has Tl and is high (119).
- "contains Cu": every material contains Cu — not discriminative.
- "contains Ba": nearly all have Ba. O6Cu1Cd0.250Ba2Tl1.750 has Ba, low (88). Not discriminative enough.

### Looking for an interaction rule

The clearest signal: among Hg-bearing cuprates the high ones are HgBa2CuO4-type and the Ca-doped Hg-1212 series. The unmeasurable set needs a structural rule.

Let me focus on the contrast that the task hint emphasizes ("the governing rule is an interaction"). The two unmeasurable sub-series:
- O4.xx Cu1Ba2Hg1 with 4 elements: measured points show a **monotonic dependence on O content** — Tc falls as O rises above ~4.10. At O4.180 (NULL) and O4.047 (NULL) we must extrapolate.
  - O4.047 is BELOW the measured range (lowest measured = 4.081→92.0; 4.047 < 4.081). Trend: lower O → lower Tc near the low-O tail? Actually the measured O4 series: 4.081→98.0, 4.085→92.0, 4.096→95.6, 4.097→95.3, 4.101→93.4, 4.103→92.5, 4.150→97.0, 4.162→97.4, 4.180→NULL, 4.200→94.2, 4.220→97.0. This O4 series is noisy/non-monotonic — hard to call O4.180 and O4.047 individually from O alone.

This is getting complicated; the rule must be a clean interaction that labels the 10 unmeasurables. Let me think structurally.

### Structural (cuprate-layer) reasoning — the real physics

These are all cuprate superconductors. The well-known structural fact: high-Tc cuprates require **CuO2 planes**, and the canonical "HgBa2CuO4+δ" (Hg-1201) and "HgBa2CaCu2O6+δ" (Hg-1212) families are high-Tc. The key interaction is **Hg + Ba + Cu together** (the HgBa2CuO4-δ building block), AND crucially the dopant/hole count in the right range.

But the simplest interaction that fits ALL measured points: **High iff (contains Hg AND contains Ba AND contains Cu) AND NOT (contains Tl simultaneously with Hg)** — let me test:

- O6Cu1Ba2Hg1Tl1: Hg+Ba+Cu+Tl → has both Hg and Tl → predicted low. Actual: 0.0 low ✓
- O10Ca2Cu3Ba2Tl2: has Tl but NO Hg → not excluded by "Hg AND Tl" rule. Need high. Has Ba+Cu+Tl. ✓ high (119) — OK because no Hg.
- O6Cu1Cd0.250Ba2Tl1.750: no Hg, has Tl+Ba+Cu. Actual 88 low. ✗ — rule predicts high (no Hg, so not excluded; has Ba+Cu). So this rule is WRONG.

So a simple "Hg & Ba & Cu, but not (Hg & Tl)" rule mislabels O6Cu1Cd0.250Ba2Tl1.750 (low, 88) and O7Cu3Ba2La1 (low, 89.6) and O7Cu3Y0.200Ba2La0.800 (low, 81).

### Reconsider: what makes the non-Hg materials low?

Non-Hg measured:
- O10Ca2Cu3Ba2Tl2 → 118.97 HIGH (Tl-Ba-Cu-Ca, 5 elem)
- O6Cu1Cd0.250Ba2Tl1.750 → 88.0 LOW (Tl-Ba-Cu-Cd, 5 elem)
- O7Cu3Ba2La1 → 89.6 LOW (Ba-Cu-La, 4 elem)
- O7Cu3Y0.200Ba2La0.800 → 81.0 LOW (Ba-Cu-Y-La, 5 elem)

The single high non-Hg is O10Ca2Cu3Ba2Tl2. Differentiator vs the other three:
- It has **Cu count = 3** AND **Ca**. The lows: Cu# = 1 (Cd sample), Cu# = 3 (La sample, Cu3) but no Ca and O=7.

Hmm, O7Cu3Ba2La1 has Cu#=3 like the high one, yet is low (89.6). Difference: the high one has O=10 and Ca; the low one has O=7 and La (no Ca).

So among Tl/Ba-cuprates: high needs (Ca + high O + Cu3)? O10Ca2Cu3Ba2Tl2 fits. This is the Tl-2223 family (Tl2Ba2Ca2Cu3O10) which is genuinely ~120 K. The Cu3 + Ca + Tl2 + Ba2 + O10 = Tl-2223 → high. Makes sense.

### Unmeasurable list — apply structural family recognition
| composition | elements |
|---|---|
| O6.160Ca1Cu2Ba2Hg1 | Hg,Ca,Cu,Ba,O (Hg-1212) |
| O6.270Ca1Cu2Ba2Hg1 | Hg,Ca,Cu,Ba,O (Hg-1212) |
| O4.180Cu1Ba2Hg1 | Hg,Cu,Ba,O (Hg-1201) |
| O4.047Cu1Ba2Hg1 | Hg,Cu,Ba,O (Hg-1201) |
| O4.066Cu1Ba2Hg1 | Hg,Cu,Ba,O (Hg-1201) |
| O4.160Cu1Ba2Hg1 | Hg,Cu,Ba,O (Hg-1201) |
| O8Ca0.850Cu2.200Ba2Tl1.800 | Tl,Ca,Cu,Ba,O (Tl-doped 1212) |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | Tl,Ca,Cu,Sr,Pb,O (Tl/Pb-1212 with Sr) |
| O8Cu3Y1Ba1.900La0.100 | Y,Cu,Ba,La,O (YBCO-like, no Hg/Tl) |
| O8Cu4Y1Ba1.870La0.130 | Y,Cu,Ba,La,O (Y-124-like, no Hg/Tl) |

Now the rule must be an interaction. Let me reconsider the measured Hg-1201 (O4.xx Cu1Ba2Hg1) data: ALL measured ones (O4.081→4.220) that returned a value are in range 92.0–98.0 → ALL HIGH (>=90) EXCEPT none below 90 in measured set... wait O4.200=94.2 high, O4.101=93.4 high, O4.103=92.5 high, O4.085=92.0 high. All Hg-1201 measured points are HIGH (>=90).

So for the Hg-1201 unmeasurables (O4.180, O4.047, O4.066, O4.160): same family, O content within/near the measured range → predict HIGH. The physics: HgBa2CuO4+δ (Hg-1201) is a known ~94-98 K superconductor across a wide δ range. The 4 unmeasurable Hg-1201 are all in the family → HIGH.

For the Hg-1212 unmeasurables (O6.160, O6.270): same Ca1Cu2Ba2Hg1 family as the 10 measured HIGH points (92–127) → predict HIGH. (O6.160 sits between O6.140=105 and O6.190=122; O6.270 sits between O6.240=123 and O6.300=100. Both clearly in the high regime.)

For Tl/Ca/Cu/Ba unmeasurable (O8Ca0.850Cu2.200Ba2Tl1.800): Tl-Ba-Ca-Cu cuprate (Tl-2212/1212 family). The measured Tl analogue O10Ca2Cu3Ba2Tl2 (Tl-2223) is high (119). Tl-1212/2212 with Ca are known high-Tc (~110 K, 128 K). Ca present, Cu2, Ba2, Tl~1.8 → HIGH.

For O7Ca1Cu2Sr2Tl0.500Pb0.500: Tl/Pb-Sr-Ca-Cu2 cuprate. This is the (Tl,Pb)Sr2CaCu2O7 family = Tl/Pb-1212 with Sr instead of Ba. Known high-Tc (~105-128 K). Has Ca+Cu2+Tl/Pb. → HIGH.

For O8Cu3Y1Ba1.900La0.100: Y-Ba-Cu3-O8, YBCO-123-like (Y1Ba2Cu3O7 is 92 K; La-substituted). High (YBCO ~92 K). Cu3+Ba+Y. → HIGH (borderline ~90-92, but >=90).

For O8Cu4Y1Ba1.870La0.130: Y-Ba-Cu4-O8, Y-124-like (YBa2Cu4O8 is ~80-82 K). Cu4 + Y + Ba, O8. Y-124 is genuinely ~80 K → LOW (<90).

Hmm — but is element-count the discriminant here? Both YBCO-like have 5 elements (Y,Cu,Ba,La,O). One high one low. The discriminant is Cu# (3 vs 4) → Y-123 vs Y-124. That's a structural-family distinction, not a simple factor.

### Consolidating the induced rule

The task demands an *interaction* rule, not one-factor. The cleanest interaction consistent with ALL measured data:

**A material is high-Tc iff it belongs to a known high-Tc cuprate structural family — i.e., it contains the combination (Cu + a heavy cation from {Hg, Tl}) + Ba or Sr + (Ca or Y) in the right stoichiometric layering), which is an interaction among (Cu present) AND (Hg or Tl present) AND (alkaline-earth Ba/Sr present) AND (Ca or Y present as the interlayer cation) — EXCLUDING the cases that break it: Hg+Tl together (kills it, Tc=0), and Cu4/Y-124 stacking (kills it, ~80 K).**

Let me verify this composite rule against every measured point:

| composition | Cu? | (Hg or Tl)? | (Ba or Sr)? | (Ca or Y)? | Hg&Tl both? | Cu4/Y124? | predicted | actual | ✓/✗ |
|---|---|---|---|---|---|---|---|---|---|
| O6.080..O6.310 Ca1Cu2Ba2Hg1 (10 pts) | Y | Hg | Ba | Ca | N | N | HIGH | all >=92 HIGH | ✓✓✓✓✓✓✓✓✓✓ |
| O4.xx Cu1Ba2Hg1 (10 pts) | Y | Hg | Ba | (no Ca, no Y) | N | N | — fails "Ca or Y" → LOW?? | all HIGH (92-98) | ✗✗✗✗ |

DAMN. The Hg-1201 (O4 Cu1Ba2Hg1) family has NO Ca/Y and is STILL high. So "Ca or Y present" is wrong.

This is the crux: Hg-1201 (HgBa2CuO4+δ) is high WITHOUT Ca/Y. So the interlayer-cation requirement only applies to the multi-layer (1212/1223) families.

### Rethink the interaction more carefully

Let me reconsider what truly separates high from low in the MEASURED set, then check it generalizes.

Measured HIGH: all 10 Ca1Cu2Ba2Hg1 series (Hg-1212), all 10 O4.xx Cu1Ba2Hg1 (Hg-1201), O10Ca2Cu3Ba2Tl2 (Tl-2223).
Measured LOW: O6Cu1Ba2Hg1Tl1 (0.0), O6Cu1Cd0.250Ba2Tl1.750 (88), O7Cu3Ba2La1 (89.6), O7Cu3Y0.200Ba2La0.800 (81).

What's common to ALL highs? **Hg OR (Tl+Ca+Cu3)**. What's common to lows?
- O6Cu1Ba2Hg1Tl1: Hg present but ALSO Tl → low. (Hg+Tl kills it.)
- O6Cu1Cd0.250Ba2Tl1.750: Tl present but Cu1 (not Cu3) AND Cd not Ca → low.
- O7Cu3Ba2La1: no Hg, no Tl → low.
- O7Cu3Y0.200Ba2La0.800: no Hg, no Tl → low.

So candidate interaction rule:
**High iff [ contains Hg AND NOT contains Tl ]  OR  [ contains Tl AND Ca AND (Cu count == 3) AND NOT Hg ]**

Check:
- Hg-1212 series: Hg, no Tl → HIGH ✓
- Hg-1201 series: Hg, no Tl → HIGH ✓
- O6Cu1Ba2Hg1Tl1: Hg AND Tl → first clause fails (has Tl); second clause needs Tl+Ca+Cu3 — has Tl but no Ca, Cu1 → fails. → LOW ✓ (0.0)
- O10Ca2Cu3Ba2Tl2: no Hg; Tl+Ca+Cu3 → HIGH ✓ (119)
- O6Cu1Cd0.250Ba2Tl1.750: no Hg; Tl but no Ca, Cu1 → LOW ✓ (88)
- O7Cu3Ba2La1: no Hg, no Tl → LOW ✓ (89.6)
- O7Cu3Y0.200Ba2La0.800: no Hg, no Tl → LOW ✓ (81)

This rule fits ALL 25 measured points. It's an interaction (Hg & ¬Tl) OR (Tl & Ca & Cu3 & ¬Hg). Good.

But wait — does "Cu count == 3" for the Tl clause correctly exclude O6Cu1Cd0.250Ba2Tl1.750 (Cu1)? Yes ✓. And O7Cu3Ba2La1 has no Tl at all → low ✓.

Hmm, but is the Tl clause's Ca requirement right, or should it be (Ca or Y)? The only high Tl sample is O10Ca2Cu3Ba2Tl2 (has Ca). I have no high Tl+Y sample to test. The Tl-2223 family with Ca is the high one. With Y, Tl-2212 (Tl2Ba2YCu2O8?) is ~passive/different. Keep "Ca" for the Tl clause — it matches the data.

Actually, let me reconsider whether the rule should mention Ba/Sr at all. All highs have Ba. But all lows except... O7Cu3Ba2La1 has Ba too. So Ba is not discriminative. Don't include Ba.

### Apply the induced rule to the 10 unmeasurables

| composition | Hg? | Tl? | Ca? | Cu# | rule outcome | prediction |
|---|---|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | Y | N | Y | 2 | Hg & ¬Tl → HIGH | **high** |
| O6.270Ca1Cu2Ba2Hg1 | Y | N | Y | 2 | Hg & ¬Tl → HIGH | **high** |
| O4.180Cu1Ba2Hg1 | Y | N | N | 1 | Hg & ¬Tl → HIGH | **high** |
| O4.047Cu1Ba2Hg1 | Y | N | N | 1 | Hg & ¬Tl → HIGH | **high** |
| O4.066Cu1Ba2Hg1 | Y | N | N | 1 | Hg & ¬Tl → HIGH | **high** |
| O4.160Cu1Ba2Hg1 | Y | N | N | 1 | Hg & ¬Tl → HIGH | **high** |
| O8Ca0.850Cu2.200Ba2Tl1.800 | N | Y | Y | ~2.2 | Tl & Ca & (Cu3?) — Cu≈2.2 not exactly 3 → fails second clause; no Hg → LOW?? | see below |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | N | Y | Y | 2 | Tl & Ca but Cu2 not 3 → LOW?? | see below |
| O8Cu3Y1Ba1.900La0.100 | N | N | N | 3 | no Hg, no Tl → LOW | **low** |
| O8Cu4Y1Ba1.870La0.130 | N | N | N | 4 | no Hg, no Tl → LOW | **low** |

Problem: my rule's Tl clause requires Cu count == 3 exactly. But two unmeasurable Tl+Ca cuprates have Cu≈2 (Tl-2212/1212 type), which my rule would label LOW. Physically, Tl-2212 (Tl2Ba2CaCu2O8) and Tl-1212 (TlBa2CaCu2O7) are HIGH (~108-128 K). So my rule is too strict — it mislabels genuine high-Tc Tl-1212/2212 as low.

I have no measured Tl+Ca+Cu2 point to confirm. So I'm extrapolating. Let me reconsider the rule to be physically sound AND fit measured data, while not over-fitting to the single Tl-2223 point.

### Physically-grounded interaction rule (final)

The real physics of these layered cuprates: high Tc arises from the **CuO2 planes** count and proper hole doping. The known families:
- Hg-1201 (HgBa2CuO4+δ): 1 CuO2 plane, ~94-98 K. HIGH.
- Hg-1212 (HgBa2CaCu2O6+δ): 2 CuO2 planes, ~127 K. HIGH.
- Hg-1223 (HgBa2Ca2Cu3O8+δ): 3 planes, ~135 K. HIGH.
- Tl-1201/1212/2212/2223/1223: corresponding high-Tc families (80–128 K).
- YBCO-123 (YBa2Cu3O7): 92 K HIGH. Y-124 (YBa2Cu4O8): 80 K LOW-ish (<90).
- Co-doped / wrong-stacking variants: low or zero.

The general rule: **High iff the material is a properly-stacked cuprate with an (Hg or Tl) mercury/thallium-oxide rock-salt layer AND Ba/Sr, OR a YBCO-123-type — but NOT when Hg and Tl coexist (the layers compete → Tc collapses to ~0), and NOT the Y-124 (Cu4) stacking.**

Operationally as an interaction over composition: I'll keep the rule that fits the measured data but broaden the Tl clause to "Tl present, no Hg, has Ca" (drop the strict Cu3=3, since both Cu2 and Cu3 Tl-Ca cuprates are high in reality):

Revised rule:
**High iff ( contains Hg AND NOT contains Tl ) OR ( contains Tl AND contains Ca AND NOT contains Hg ) OR ( YBCO-123 type: contains Y AND Ba AND Cu#=3 AND NOT Hg AND NOT Tl )**

Re-verify on measured:
- Hg-1212, Hg-1201 series → clause1 → HIGH ✓
- O6Cu1Ba2Hg1Tl1 → Hg&Tl: clause1 fails (Tl present), clause2 needs ¬Hg (fails, has Hg), clause3 no Y → LOW ✓ (0.0)
- O10Ca2Cu3Ba2Tl2 → clause2: Tl, Ca, ¬Hg → HIGH ✓ (119)
- O6Cu1Cd0.250Ba2Tl1.750 → clause2: Tl, no Ca → fails; clause1 no Hg; clause3 no Y → LOW ✓ (88)
- O7Cu3Ba2La1 → no Hg no Tl; clause3 no Y (has La not Y) → LOW ✓ (89.6)
- O7Cu3Y0.200Ba2La0.800 → clause3: Y, Ba, but Cu#=3 ✓ and ¬Hg ¬Tl ✓ → predicts HIGH?? Actual 81 LOW ✗.

PROBLEM: O7Cu3Y0.200Ba2La0.800 is Y+Ba+Cu3+O7 but actual Tc=81 (LOW). My clause3 predicts high. Wrong.

So YBCO-123-type isn't simply "Y+Ba+Cu3". The measured O7Cu3Y0.200Ba2La0.800 = Y0.2Ba2La0.8Cu3O7 — heavily La-doped YBCO, Tc 81. And O7Cu3Ba2La1 (no Y at all, La1) = 89.6, also low.

What distinguishes high YBCO (not in measured set) from these two low La-cuprates? The low ones have La dominating the rare-earth site (La0.8Y0.2 or pure La1). Pure La = no YBCO-123 (it's a different, lower-Tc cuprate, "Cu3Ba1La1O7"-like, Tl-free La-cuprates are ~89 K borderline/low). Heavy La doping suppresses Tc below 90.

Given I have NO measured high YBCO-123 point (the two YBCO-ish are both low), I should be conservative: the YBCO clause should NOT fire on these. Let me drop the standalone YBCO-123 clause entirely and treat the unmeasurable Y-cuprates by their family.

### Simplify: focus the rule on the Hg/Tl cuprates (which is where the data is)

Measured-data-fitting rule (interaction), dropping YBCO as not-high-without-La:
**High iff ( contains Hg AND NOT contains Tl ) OR ( contains Tl AND contains Ca AND NOT contains Hg ).**

Verify ALL measured:
- 10 Hg-1212 → HIGH ✓ (all 92-127)
- 10 Hg-1201 → HIGH ✓ (92-98)
- O6Cu1Ba2Hg1Tl1 (Hg&Tl) → clause1 blocked by Tl, clause2 blocked by Hg → LOW ✓ (0.0)
- O10Ca2Cu3Ba2Tl2 (Tl,Ca,¬Hg) → clause2 → HIGH ✓ (119)
- O6Cu1Cd0.250Ba2Tl1.750 (Tl,¬Ca,¬Hg) → LOW ✓ (88)
- O7Cu3Ba2La1 (¬Hg,¬Tl) → LOW ✓ (89.6)
- O7Cu3Y0.200Ba2La0.800 (¬Hg,¬Tl) → LOW ✓ (81)

✅ Fits all 25 measured points perfectly. Two clauses, both interactions (Hg∧¬Tl) ∨ (Tl∧Ca∧¬Hg).

Apply to 10 unmeasurables:
| composition | Hg? | Tl? | Ca? | prediction |
|---|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | Y | N | Y | HIGH (clause1) |
| O6.270Ca1Cu2Ba2Hg1 | Y | N | Y | HIGH (clause1) |
| O4.180Cu1Ba2Hg1 | Y | N | N | HIGH (clause1) |
| O4.047Cu1Ba2Hg1 | Y | N | N | HIGH (clause1) |
| O4.066Cu1Ba2Hg1 | Y | N | N | HIGH (clause1) |
| O4.160Cu1Ba2Hg1 | Y | N | N | HIGH (clause1) |
| O8Ca0.850Cu2.200Ba2Tl1.800 | N | Y | Y | HIGH (clause2) |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | N | Y | Y | HIGH (clause2) |
| O8Cu3Y1Ba1.900La0.100 | N | N | N | LOW |
| O8Cu4Y1Ba1.870La0.130 | N | N | N | LOW |

Predicted high: 8 materials; predicted low: 2 (the two Y-Ba-Cu-La-O cuprates).

### Sanity check against physics & the task hint
- Task: "One-factor rules wrong; governing rule is an interaction." ✓ — my rule is the interaction (Hg∧¬Tl)∨(Tl∧Ca∧¬Hg). "contains Hg" alone is wrong (O6Cu1Ba2Hg1Tl1 has Hg→low). "contains Tl" alone is wrong (Cd-doped Tl is low; Hg+Tl is low).
- The 6 Hg unmeasurables → high: consistent with measured Hg-1212/1201 families.
- The 2 Tl+Ca unmeasurables → high: Tl-2212/1212 (Cu2) and Tl/Pb-1212 are known high-Tc (108-128 K). ✓.
- The 2 Y-Ba-Cu-O unmeasurables → low: O8Cu4Y1Ba1.870La0.130 is Y-124 type (~80 K, low ✓). O8Cu3Y1Ba1.900La0.130 is YBCO-123-ish (YBa2Cu3O7=92K high) but with Ba1.9La0.13 → La-substituted; could be ~90 borderline. My rule (no Hg, no Tl → low) labels it low.

The Y-123 borderline is the one risk: O8Cu3Y1Ba1.900La0.130 ≈ YBCO-123 (92 K, high) with slight La. But my data-driven rule (only Hg/Tl cuprates observed high) labels it low. Given the measured Y/La cuprates (O7Cu3Y0.200Ba2La0.800=81, O7Cu3Ba2La1=89.6) are both low, and the rule that fits the data excludes non-Hg/non-Tl materials, I'll commit to LOW for both Y-cuprates. This is the consistent inductive choice from the data I have.

(If I tried to add a YBCO-123-high clause to make O8Cu3Y1Ba1.900La0.130 high, I'd have to explain why O7Cu3Y0.200Ba2La0.800 is low — and the only differentiator is La fraction, which I can't cleanly threshold from 2 points. So the data-consistent rule stays, both Y-cuprates = low.)

## Step D — final pick
Among unmeasurable predicted HIGH (8 materials), choose FEWEST elements (tie → higher expected Tc).
- O6.160Ca1Cu2Ba2Hg1 → 5 elements (O,Ca,Cu,Ba,Hg)
- O6.270Ca1Cu2Ba2Hg1 → 5 elements
- O4.180Cu1Ba2Hg1 → 4 elements (O,Cu,Ba,Hg)
- O4.047Cu1Ba2Hg1 → 4 elements
- O4.066Cu1Ba2Hg1 → 4 elements
- O4.160Cu1Ba2Hg1 → 4 elements
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5 elements (O,Ca,Cu,Ba,Tl)
- O7Ca1Cu2Sr2Tl0.500Pb0.500 → 6 elements (O,Ca,Cu,Sr,Tl,Pb)

Fewest = 4 elements → the O4.xx Cu1Ba2Hg1 group: O4.180, O4.047, O4.066, O4.160. Four-way tie on element count. Break by higher expected Tc.

Expected Tc for these (Hg-1201 family, extrapolate from measured O4 series which range 92–98, avg ~95-96, roughly flat ~94-98 across the family). I need the highest expected Tc among O4.047, O4.066, O4.160, O4.180.

Measured O4.xx Cu1Ba2Hg1 Tc vs O content:
- 4.081→98.0, 4.085→92.0, 4.096→95.6, 4.097→95.3, 4.101→93.4, 4.103→92.5, 4.150→97.0, 4.162→97.4, 4.200→94.2, 4.220→97.0

There's a mild pattern: Tc dips ~4.085-4.103 (92-93) and rises at 4.150-4.162 (97-97.4) and 4.220 (97.0). The unmeasurable O values:
- O4.047 — below measured range (lowest measured 4.081). Extrapolating down, Tc likely near the low end of the band, ~92-94. Possibly lower (more O-deficient, underdoped → lower). Estimate ~92-94.
- O4.066 — just below 4.081. ~94.
- O4.160 — between 4.150(97.0) and 4.162(97.4) → ~97.3, the highest expected.
- O4.180 — between 4.162(97.4) and 4.200(94.2) → ~95.5-96.

**O4.160Cu1Ba2Hg1 has the highest expected Tc (~97) among the tied 4-element group.** → Final pick = **O4.160Cu1Ba2Hg1**.

## Summary of answers
- peak (Step A) = O6.210Ca1Cu2Ba2Hg1, peak_tc = 127.0
- threshold = 127 - 32 = 95.0
- qualifying (Q1): O4.081Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.220Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1
- rule: (contains Hg AND NOT contains Tl) OR (contains Tl AND contains Ca AND NOT contains Hg) — i.e. high-Tc cuprates are the Hg-bearing (Tl-free) family or the Tl-bearing Ca-cuprate family; coexistence of Hg and Tl, or absence of both, gives low Tc.
- predictions (10): see table above.
- final (Step D) = O4.160Cu1Ba2Hg1

Budget used: 25 / 45.
