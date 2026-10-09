# Notes — superconductor Tc study

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O, Ca, Cu, Ba, Hg)
- High = Tc >= 90 K; low = Tc < 90 K
- Budget: 45 `get_property(tc)` calls.

## Step A — series peak (O6.xxxCa1Cu2Ba2Hg1 series; same element set as seed, varying O)

| Composition | Tc (K) | note |
|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | |
| O6.160Ca1Cu2Ba2Hg1 | null | unmeasurable (also in unmeasurable.txt) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | ← peak (tie) |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | ← peak (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | |
| O6.270Ca1Cu2Ba2Hg1 | null | unmeasurable (also in unmeasurable.txt) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | |

**T\* = 127.0 K** (peak composition `O6.210Ca1Cu2Ba2Hg1`; tie with O6.220). Step A says "find the composition with MAXIMUM Tc"; two compositions tie at 127.0. I report the first-encountered `O6.210Ca1Cu2Ba2Hg1`. (Both equal; tie-break is not specified at Step A.)

## Step B — filter family B (Hg + exactly 4 elements, Tc >= threshold1)
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

Hg-containing materials with exactly 4 elements = the `O4.xxxCu1Ba2Hg1` series (O, Cu, Ba, Hg = 4 elements).

| Composition | Tc (K) | >= 95.0? |
|---|---|---|
| O4.047Cu1Ba2Hg1 | null | unmeasurable |
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.085Cu1Ba2Hg1 | 92.0 | no |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.101Cu1Ba2Hg1 | 93.4 | no |
| O4.103Cu1Ba2Hg1 | 92.5 | no |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.180Cu1Ba2Hg1 | null | unmeasurable |
| O4.200Cu1Ba2Hg1 | 94.2 | no |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |

**Q1 (measured, qualifying):** O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1, O4.162Cu1Ba2Hg1, O4.220Cu1Ba2Hg1.
(O4.047 and O4.180 are unmeasurable — prediction deferred to Step C.)

## Step C — extra measurable materials (to induce the rule)

| Composition | Elements | Tc (K) | high? |
|---|---|---|---|
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl (5) | 0.0 | low |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl (5) | 118.97 | high |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl (5) | 88.0 | low |
| O7Cu3Ba2La1 | O,Cu,Ba,La (4) | 89.6 | low |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Ba,Y,La (5) | 81.0 | low |

Budget used so far: 25 / 45.

## Rule induction (analysis)

All measured high-Tc materials and their element sets:

- O6.xxxCa1Cu2Ba2Hg1 series — elements {O, Ca, Cu, Ba, Hg} — all high (92–127) across a wide O range.
- O4.xxxCu1Ba2Hg1 series — elements {O, Cu, Ba, Hg} (4 elements, NO Ca) — high (92–98).
- O10Ca2Cu3Ba2Tl2 — {O, Ca, Cu, Ba, Tl} — high (118.97).
- O6Cu1Ba2Hg1Tl1 — {O, Cu, Ba, Hg, Tl} — LOW (0.0).
- O6Cu1Cd0.250Ba2Tl1.750 — {O, Cu, Cd, Ba, Tl} — low (88.0).
- O7Cu3Ba2La1 — {O, Cu, Ba, La} (4) — low (89.6).
- O7Cu3Y0.200Ba2La0.800 — {O, Cu, Ba, Y, La} (5) — low (81.0).

Key observations:
1. **Contains Hg alone is not enough**: O6Cu1Ba2Hg1Tl1 contains Hg but is LOW (0.0). And actually every Hg cuprate above is high EXCEPT the Tl co-doped one. So Hg presence does not guarantee high.
2. **Element count is not the rule**: both 4-element (O4 Cu1Ba2Hg1 = high; O7Cu3Ba2La1 = low) and 5-element materials (Ca-Cu-Ba-Hg = high; Cu-Ba-Hg-Tl = low) appear on both sides. So "has 5 elements" / "has 4 elements" fails.
3. The discriminating interaction: **Cu–O planes (the cuprate backbone) carry superconductivity; high Tc requires the cuprate motif with the right hole-doping cations. The low/zero materials break it via specific substitution.** Looking closer at the TWO zeros/nears:
   - O6Cu1Ba2Hg1Tl1 = 0.0: Hg AND Tl both present, Cu=1 only → over/under-doped or phase competition; Tl+Hg together without Ca → not high.
   - O6Cu1Cd0.250Ba2Tl1.750 = 88.0 (just below high): Cd+Tl replace Hg+Ca; no Hg, Cd instead → borderline-low.
   - O7Cu3Ba2La1 = 89.6: no alkaline-earth chain cation of the right type (La substitutes for the Ba/Y-site in a way that depresses Tc just below 90).
   - O7Cu3Y0.200Ba2La0.800 = 81.0: Y+La on the chain → depressed.

The cleanest interaction that separates high from low: **High Tc requires an alkaline-earth (Ca/Ba/Sr) + Cu + (Hg OR Tl) layer system on a cuprate — specifically the combination "Cu–Ba–(Hg or Tl) backbone present, AND an alkaline-earth cation (Ca or Ba acting as the separating layer) present."**

Let me test that interaction rule against the measured set:

Material | Cu–O–Ba backbone? | alkaline earth divalent layer (Ca/Ba/Sr)? | Hg or Tl? | predicted | actual
- O6.xxxCa1Cu2Ba2Hg1 | yes (Cu+Ba+O) | yes (Ca) | Hg yes | high | high ✓
- O4.xxxCu1Ba2Hg1 | yes | Ba is the AE | Hg yes | high | high ✓
- O10Ca2Cu3Ba2Tl2 | yes (Cu+Ba+O) | yes (Ca) | Tl yes | high | high ✓
- O6Cu1Ba2Hg1Tl1 | yes (Cu+Ba+O) | NO Ca, no Sr; only Ba+Hg+Tl... but Ba is AE | Hg+Tl both | — actual LOW (0.0)

Hmm, O6Cu1Ba2Hg1Tl1 has Ba (an alkaline earth) and Cu and O and Hg+Tl → by the above it'd predict high, but it's 0.0. So that rule is too permissive. Need refinement.

Refined interaction: The O6Cu1Ba2Hg1Tl1 case shows **simultaneous Hg AND Tl kills Tc** (phase competition / wrong stacking). The Cd case shows **substituting the wrong metal (Cd for Hg; Tl for the Ca layer) drops Tc below 90**. The La/Y-on-Ba-site cases drop below 90.

Let me reconsider what truly separates the four low cases from the high ones:

HIGH set element-sets:
- {O,Ca,Cu,Ba,Hg}
- {O,Cu,Ba,Hg}            (no Ca, but still high ~92–98)
- {O,Ca,Cu,Ba,Tl}

LOW set element-sets:
- {O,Cu,Ba,Hg,Tl}         (Hg+Tl together, no Ca) → 0.0
- {O,Cu,Cd,Ba,Tl}         (Cd+Tl, no Hg, no Ca) → 88.0
- {O,Cu,Ba,La}            (La replaces... only Cu/Ba/La/O) → 89.6
- {O,Cu,Ba,Y,La}          → 81.0

Notice: the high set always has **Ca** OR is the bare **{O,Cu,Ba,Hg}** 4-element mercury cuprate. The low set lacks Ca AND (for the La/Y ones) lacks Hg/Tl entirely, OR has Hg+Tl simultaneously.

The unifying interaction rule (fits all measured data):
> **High Tc ⟺ the material is a cuprate (Cu + O present) that EITHER (a) contains Ca together with a Tl- or Hg-layer (Ca + (Hg or Tl)), OR (b) is the 4-element mercury barium cuprate {O,Cu,Ba,Hg} (Ba providing the alkaline-earth layer in place of Ca). Simultaneous Hg+Tl, or substitution of La/Y/Cd into the structure, destroys high-Tc.**

Let me verify this against ALL measured:
- O6.xxxCa1Cu2Ba2Hg1: Ca + Hg + cuprate → high ✓ (all high)
- O4.xxxCu1Ba2Hg1: case (b) {O,Cu,Ba,Hg} → high ✓ (all 92–98, all ≥90 ✓)
- O10Ca2Cu3Ba2Tl2: Ca + Tl + cuprate → high ✓
- O6Cu1Ba2Hg1Tl1: Hg AND Tl simultaneously → not high → 0.0 ✓
- O6Cu1Cd0.250Ba2Tl1.750: no Ca, Cd+Tl, not the Hg-barium-4 → low ✓ (88.0)
- O7Cu3Ba2La1: no Ca, no Hg/Tl, La-based → low ✓ (89.6)
- O7Cu3Y0.200Ba2La0.800: no Ca, no Hg/Tl, Y/La → low ✓ (81.0)

Rule fits all measured cases with no exception. Apply to the 10 unmeasurable materials.

## Step C — predictions for unmeasurable.txt

1. **O6.160Ca1Cu2Ba2Hg1** — {O,Ca,Cu,Ba,Hg}, Ca+Hg cuprate → **high**. (Interpolating the series: between 105.0 @6.140 and 122.0 @6.190 → ~115 K, definitely high.)
2. **O6.270Ca1Cu2Ba2Hg1** — {O,Ca,Cu,Ba,Hg}, Ca+Hg cuprate → **high**. (Series: 123.0 @6.240 and 100.0 @6.300 → ~112 K, high.)
3. **O4.180Cu1Ba2Hg1** — {O,Cu,Ba,Hg} 4-element Hg-Ba cuprate → **high**. (Series neighbors: 97.4 @4.162, 94.2 @4.200 → ~96 K, high.)
4. **O4.047Cu1Ba2Hg1** — {O,Cu,Ba,Hg} 4-element Hg-Ba cuprate → **high**. (Lowest-O member; series is 92–98 across the whole O4 range, so ~93 K → high, just above 90.)
5. **O4.066Cu1Ba2Hg1** — {O,Cu,Ba,Hg} 4-element → **high**. (~92–93 K, high.)
6. **O4.160Cu1Ba2Hg1** — {O,Cu,Ba,Hg} 4-element → **high**. (~97 K, high.)
7. **O8Ca0.850Cu2.200Ba2Tl1.800** — {O,Ca,Cu,Ba,Tl}: Ca + Tl + cuprate → **high**. (Ca present, Tl layer present, cuprate present, no Hg+Tl simultaneity problem, no La/Y/Cd) → high. Expected Tc near O10Ca2Cu3Ba2Tl2 (118.97) family → high.
8. **O7Ca1Cu2Sr2Tl0.500Pb0.500** — {O,Ca,Cu,Sr,Tl,Pb}: Ca + Tl + cuprate → but has Pb and Sr (Sr is alkaline earth too). Ca + Tl present, cuprate (Cu-O) present. No Hg+Tl simultaneity. → **high**. (Tl-Ca-Sr-Cu cuprate family, expected high ~100+ K.)
9. **O8Cu3Y1Ba1.900La0.100** — {O,Cu,Ba,Y,La}: NO Ca, NO Hg/Tl, Y+La on the chain site → **low** (like O7Cu3Y0.200Ba2La0.800 = 81.0).
10. **O8Cu4Y1Ba1.870La0.130** — {O,Cu,Ba,Y,La}: same signature as #9 → **low**.

Let me double-check #4 and #5 (O4.047 and O4.066) — they're at the very low-O edge of the O4 series. The O4 series measured values: O4.081=98, O4.085=92, O4.096=95.6, O4.097=95.3, O4.101=93.4, O4.103=92.5, then 4.150=97, 4.162=97.4, 4.200=94.2, 4.220=97. So across O=0.047 → 0.220 the series stays in ~92–98. O4.047 and O4.066 are slightly below the lowest measured (4.081=98, 4.085=92); extrapolating, they should be ~90–92, still high (≥90) but near the boundary. The rule (case b: 4-element Hg-Ba cuprate) classifies them high — consistent with the family all being ≥92. I'll keep **high** but note the boundary proximity for #4.

Actually — reconsider the boundary for #4 O4.047: the lowest measured in the family is O4.085=92.0 and O4.103=92.5; nothing measured is below 92. At O=0.047 (well below 0.085), mercury barium cuprates are oxygen-poor; the family's measured floor is ~92. By the structural rule (the 4-element Hg-Ba cuprate motif is high-Tc), classify **high**.

## Step D — final pick
Among unmeasurable materials predicted **high** (1,2,3,4,5,6,7,8), choose the one with the FEWEST elements.

Element counts of the predicted-high unmeasurables:
- O6.160Ca1Cu2Ba2Hg1 → 5
- O6.270Ca1Cu2Ba2Hg1 → 5
- O4.180Cu1Ba2Hg1 → 4  ← fewest (tie)
- O4.047Cu1Ba2Hg1 → 4  ← fewest (tie)
- O4.066Cu1Ba2Hg1 → 4  ← fewest (tie)
- O4.160Cu1Ba2Hg1 → 4  ← fewest (tie)
- O8Ca0.850Cu2.200Ba2Tl1.800 → 5
- O7Ca1Cu2Sr2Tl0.500Pb0.500 → 6

Tie at 4 elements among the four O4.xxxCu1Ba2Hg1 unmeasurables. Break ties by **higher expected Tc**:
- O4.047 ~92 (lowest-O edge)
- O4.066 ~92–93
- O4.160 ~97 (between 4.150=97 and 4.162=97.4) ← highest expected
- O4.180 ~96 (between 4.162=97.4 and 4.200=94.2)

**Final = `O4.160Cu1Ba2Hg1`** (4 elements, highest expected Tc among the 4-element tie).

Budget used: 25 / 45.
