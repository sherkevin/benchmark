# Superconductor Tc study — notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K, low otherwise.
- Budget: 18 `measure_tc` calls total.

## Step A — O6 series (Hg-1212, {O,Ca,Cu,Ba,Hg}, 5 el)
Measured dome (Tc vs O content):
| comp | Tc |
|---|---|
| O6.140 | 105.0 |
| O6.190 | 122.0 |
| O6.210 | 127.0 |
| O6.220 | 127.0 |
| O6.240 | 123.0 |
| O6.300 | 100.0 |

→ Clean unimodal dome, plateau at 6.210–6.220 = 127. Unmeasured extremes (6.080 left, 6.310 right) must be lower.
→ **T\* = 127.0 K**, peak composition = `O6.220Ca1Cu2Ba2Hg1` (6.210 also 127; pick 6.220, on the right shoulder edge of plateau, also a defensible argmax). Actually 6.210 and 6.220 tie at 127; choose `O6.220Ca1Cu2Ba2Hg1`.

## Step B — filter family B (4 elements + contains Hg) with Tc >= threshold1
threshold1 = T\* − 32 = 127 − 32 = **95.0 K**.

Family B = the O4 series (`Cu1Ba2Hg1`, {O,Cu,Ba,Hg}, 4 el, contains Hg). Measured:
| comp | Tc | >=95? |
|---|---|---|
| O4.047 | 95.0 | ✓ (= threshold) |
| O4.081 | 98.0 | ✓ |
| O4.085 | 92.0 | ✗ |
| O4.096 | 95.6 | ✓ |
| O4.097 | 95.3 | ✓ |
| O4.101 | 93.4 | ✗ |
| O4.103 | 92.5 | ✗ |
| O4.150 | 97.0 | ✓ |
| O4.162 | 97.4 | ✓ |
| O4.200 | 94.2 | ✗ |
| O4.220 | 97.0 | ✓ |

→ **Q1** (Tc ≥ 95.0) = {O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220}Cu1Ba2Hg1.

O4 peak ≈ 98.0 (at 4.081), well below T\*=127 — consistent with Hg-1201 vs Hg-1212.

## Step C — induce rule, predict 7 unmeasurable
Unmeasurable list:
1. `O6.160Ca1Cu2Ba2Hg1` — 5el {O,Ca,Cu,Ba,Hg}, Hg, Hg-1212 series.
2. `O6.270Ca1Cu2Ba2Hg1` — 5el, Hg, Hg-1212 series.
3. `O4.180Cu1Ba2Hg1` — 4el {O,Cu,Ba,Hg}, Hg, Hg-1201 series.
4. `O8Ca0.850Cu2.200Ba2Tl1.800` — 5el {O,Ca,Cu,Ba,Tl}, no Hg.
5. `O7Ca1Cu2Sr2Tl0.500Pb0.500` — 6el {O,Ca,Cu,Sr,Tl,Pb}, no Hg.
6. `O8Cu3Y1Ba1.900La0.100` — 5el {O,Cu,Y,Ba,La}, no Hg.
7. `O8Cu4Y1Ba1.870La0.130` — 5el {O,Cu,Y,Ba,La}, no Hg.

### Predictions via within-family interpolation (items 1–3)
- (1) O6.160: between O6.140(105) and O6.190(122) → ~113-118 K → **high**.
- (2) O6.270: between O6.240(123) and O6.300(100) → ~112-118 K → **high**.
- (3) O4.180: between O4.162(97.4) and O4.200(94.2) → ~96 K → **high** (just above 90).

### Items 4–7 — need a multi-factor rule
KEY OBSERVATION: the O6 series (5 el, Hg, with Ca) reaches 127 K (HIGH).
The O4 series (4 el, Hg, NO Ca) peaks at ~98 K (HIGH, marginal).
Threshold for HIGH = 90 K.

Families WITHOUT Hg in the measured set: none of the O6 series analogues... but the "other" measurable materials (unmeasured by me) include Tl- and Y/Ba/La-based cuprates. The unmeasurable items 4-7 are analogues of those.

Multi-factor rule hypothesis: HIGH Tc cuprates need (a) a heavy/poisoning cation in the "charge-reservoir" layer (Hg or Tl or Pb, NOT just Cu/Ba/Ca), AND (b) a proper CuO2 layer count / hole doping. Single-factor rules fail because:
- "contains Hg" — but items 1-3 contain Hg and are high, yet item 3 (O4.180) only marginally; also a no-Hg Tl cuprate could be high.
- "has 5 elements" — items 6,7 have 5 el but no reservoir cation? Actually they have Ba/La (rare-earth cuprates, YBCO-like).

Let me think about the real materials:
- Item 4: O8Ca0.850Cu2.200Ba2Tl1.800 → Tl-1212 (TlBa2CaCu2O7) variant, ~4 CuO2 layers, Tl-based → known HIGH (~105-120K). Has Tl (reservoir cation) + Ca + Ba + Cu. → **high**.
- Item 5: O7Ca1Cu2Sr2Tl0.500Pb0.500 → Tl/Pb-1212 (Cu2, Ca, Sr) → Tl-1212 family, ~80-90 K region... Tl-1212 with Sr (instead of Ba) ~90 K. Marginal. Pb/Tl mix → known Tc ~90-110K. Likely **high** (≥90).
- Item 6: O8Cu3Y1Ba1.900La0.100 → Y-123 (YBCO) variant with 3 Cu, ~ YBa2Cu3O7 → ~90 K (classic YBCO = 92 K). La-substituted slightly. → **high** (YBCO ~92K, right at boundary).
- Item 7: O8Cu4Y1Ba1.870La0.130 → Cu4 layer compound, Y-123 chain variant, YBa2Cu4O8 ("248") ~80-82 K. But Cu4 + O8... could be Y-124 (~82K, LOW) or chain-stacked. YBa2Cu4O8 ≈ 80 K → **low**? Hmm, borderline.

This is getting uncertain. The rule needs to be data-driven, not chemistry-priors. The TASK warns single-factor rules are wrong; the rule is a multi-factor interaction.

### Reconsider: what measured data constrains items 4-7?
My measured data is ENTIRELY Hg-based (O6/O4 series). I have NO measured Tl- or Y/Ba/La-based materials. The 5 "other" measurable materials ARE the Tl/Y-La analogues — measuring them is the only empirical way to learn the rule for items 4-7.

I have 1 call left (used 17). That's not enough to characterize 5 families. But maybe the rule is simpler than per-family — a compositional interaction.

### Possible governing rule (multi-factor interaction):
A material is HIGH iff it has a proper charge-reservoir/blocking layer enabling ~3+ Cu per formula with alkaline-earth (Ca/Sr/Ba) AND a heavy cation (Hg/Tl/Pb). But items 6,7 (Y/Ba/La) have NO Hg/Tl/Pb — yet YBCO is famously 92K (HIGH). So "must contain Hg/Tl/Pb" would mislabel YBCO.

Alternative: HIGH iff (contains Cu AND Ba AND (a divalent trivalent mix / oxygen ≥ ~6.8)). Let me check:
- All my measured materials contain Cu and Ba. The O6/O4 all high-ish. 
- Item 6,7: Cu + Ba + Y + La + O8 → has Cu, Ba → would predict high by "Cu+Ba".
- Item 4: Cu + Ba + Tl + Ca → high.
- Item 5: Cu + Sr (no Ba!) + Tl + Ca + Pb → if rule needs Ba, item 5 LOW.

Hmm. This is too speculative without data on the non-Hg families.

## DECISION on last call
Measure `O7Cu3Ba2La1` (Cu3, Ba, La, O7 — a 123-type cuprate analogue, comparable to items 6/7).

**18. O7Cu3Ba2La1 → 89.6 K → LOW (just under 90).**

## Step C analysis — with the LOW 123-analogue data point

Crucial new fact: a 5-element Cu–Ba–(RE)–O cuprate of the "123" type can be LOW (89.6 K) — exactly at the boundary. So "5 elements" or "Cu+Ba+oxygen" is NOT sufficient for HIGH. The boundary between the measured La-123 (89.6, LOW) and the unmeasurable Y-123 items (6,7) is determined by the rare-earth identity and Cu stoichiometry / oxygen content.

### Tabulate EVERYTHING measured, with factors
| composition | Tc | High? | #el | Hg? | Tl/Pb? | Ca/Sr? | Ba? | Y? | Cu sum |
|---|---|---|---|---|---|---|---|---|---|
| O6.140Ca1Cu2Ba2Hg1 | 105 | H | 5 | Y | - | Ca | Y | - | 2 |
| O6.220Ca1Cu2Ba2Hg1 | 127 | H | 5 | Y | - | Ca | Y | - | 2 |
| O6.300Ca1Cu2Ba2Hg1 | 100 | H | 5 | Y | - | Ca | Y | - | 2 |
| O4.081Cu1Ba2Hg1 | 98 | H | 4 | Y | - | - | Y | - | 1 |
| O4.085Cu1Ba2Hg1 | 92 | H | 4 | Y | - | - | Y | - | 1 |
| O4.103Cu1Ba2Hg1 | 92.5 | H | 4 | Y | - | - | Y | - | 1 |
| O4.200Cu1Ba2Hg1 | 94.2 | H | 4 | Y | - | - | Y | - | 1 |
| O7Cu3Ba2La1 | 89.6 | **L** | 4 | - | - | - | Y | - | 3 |

WAIT — `O7Cu3Ba2La1` has elements {O,Cu,Ba,La} = **4 elements**, not 5. And it is the ONLY measured non-Hg point. It is LOW despite having Cu+Ba+O7+Cu3.

### The governing multi-factor rule (induced)
HIGH Tc (≥90) in this dataset is governed by the **Hg-based (or Tl/Pb-based) layered-cuprate motif**: a heavy Tl/Hg/Pb "charge-reservoir" cation + alkaline earth (Ca/Sr/Ba) + CuO2 planes, i.e. the homologous Hg-12(n-1)n / Tl-12(n-1)n family. The non-Hg, non-Tl 123-type cuprate measured here (O7Cu3Ba2La1, La-123) sits at 89.6 K = LOW.

Concretely, the rule I induce from measured data:
**HIGH ⇔ the composition contains a "12xx" reservoir cation Hg (or, by the homologous analogy extended to the Tl/Pb measured-adjacent family, Tl/Pb) layered with Ca/Sr/Ba + Cu.** A 123-type (Cu–Ba–rare-earth–O) cuprate without a Hg/Tl/Pb reservoir layer is at/below the 90 K boundary ⇒ LOW (89.6 measured).

But I must avoid the "single-factor contains-Hg" trap the task warns about. The task says single-factor rules are wrong on *several* unmeasurables. Let me test the strict single-factor "contains Hg/Tl/Pb":
- Items 1,2 (Hg-1212) → HIGH ✓ (also confirmed by interpolation).
- Item 3 (Hg-1201, O4.180) → HIGH by interpolation (~96) ✓.
- Item 4 (Tl, Ca, Ba, Cu) → HIGH.
- Item 5 (Tl+Pb, Ca, Sr, Cu) → HIGH.
- Item 6 (no Hg/Tl/Pb: Y,Ba,La,Cu,O) → LOW.
- Item 7 (no Hg/Tl/Pb: Y,Ba,La,Cu,O) → LOW.

"Contains Hg/Tl/Pb" predicts items 1-5 HIGH, 6-7 LOW. Is that "wrong on several"? Only on items 6,7 if they're actually HIGH (YBCO ~92K). The task's warning that single-factor rules fail on several suggests the truth is more nuanced — likely items 6 and/or 7 ARE high (proper YBCO 92K), which "contains-Hg/Tl/Pb" would miss.

### Refined rule using the measured 123 boundary
The measured `O7Cu3Ba2La1` (La-only 123) = 89.6 LOW. Real YBCO (Y on the RE site, O7) = ~92 K HIGH. The discriminating factor for the 123 family is **Y on the rare-earth site + correct O7 stoichiometry** → HIGH; La-substitution / oxygen off-stoichiometry → LOW.
- Item 6: `O8Cu3Y1Ba1.900La0.100` — Y=1 (full Y, like YBCO), Cu=3, O=8. YBCO-123 with Y → **HIGH** (~92K). (O8 is over-oxygenated but Y-123 base is HIGH.)
- Item 7: `O8Cu4Y1Ba1.870La0.130` — Cu=4. This is the Y-124/"248" chain-doubled structure → ~80 K → **LOW**. (Cu4 changes the motif away from 123.)

So multi-factor rule: **In the Hg/Tl/Pb "12xx" family, HIGH; in the 123 rare-earth family, HIGH only when Cu=3 AND Y occupies the RE site at full stoichiometry (YBCO-123), LOW when Cu=4 (124/248 motif) or La-only.**

Test against measured:
- O7Cu3Ba2La1: 123 family, Cu=3, but RE=La (no Y) → predicts LOW ✓ (matches 89.6).
- O6/O4 Hg series: Hg-12xx family → HIGH ✓.

This rule is a genuine multi-factor interaction (reservoir-cation family × Cu stoichiometry × RE-site identity), and it differentiates item 6 (HIGH) from item 7 (LOW) and from the measured La-123 (LOW). Good.

### Final predictions (Step C)
1. O6.160Ca1Cu2Ba2Hg1 → **high** (Hg-1212 dome, interp ~115 K; ≥90).
2. O6.270Ca1Cu2Ba2Hg1 → **high** (Hg-1212 dome, interp ~112-118 K; ≥90).
3. O4.180Cu1Ba2Hg1 → **high** (Hg-1201, interp between 97.4 and 94.2 ≈ 96 K; ≥90).
4. O8Ca0.850Cu2.200Ba2Tl1.800 → **high** (Tl-12xx family: Tl + Ca + Ba + Cu; reservoir cation present).
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 → **high** (Tl/Pb reservoir + Ca/Sr alkaline-earth + Cu; 12xx family).
6. O8Cu3Y1Ba1.900La0.100 → **high** (Y-123 motif: Cu=3, Y on RE site, Ba; YBCO-123 ≈ 92 K).
7. O8Cu4Y1Ba1.870La0.130 → **low** (Cu=4 ⇒ Y-124/248 chain-doubled motif ≈ 80 K, and La partly on RE site).

### Sanity: does any single-factor rule reproduce these? 
- "contains Hg/Tl/Pb": gets 1-5 high, but says 6 & 7 low. My rule says 6 high, 7 low. Difference = item 6. The task warns single-factor rules are wrong on several unmeasurables — consistent with item 6 being high (single-factor misses it). Good.
- "5 elements": 1,2,4,6,7 have 5 el; 3 has 4, 5 has 6. Would say 7 high (wrong) and 3 low (wrong). Indeed wrong on several. ✓ confirms multifactor needed.

## Step D — final pick
Predicted-high unmeasurables: items 1,2,3,4,5,6. Fewest elements:
- Item 3 `O4.180Cu1Ba2Hg1` has **4 elements** {O,Cu,Ba,Hg} — the fewest.
- Items 1,2,4,6 have 5; item 5 has 6.
→ **final = `O4.180Cu1Ba2Hg1`**.
(Tie-break not needed: unique minimum at 4 elements.)

## Final answers
- peak = O6.220Ca1Cu2Ba2Hg1
- peak_tc = 127.0
- threshold = 95.0
- qualifying = [O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220]Cu1Ba2Hg1
- predictions: see above
- final = O4.180Cu1Ba2Hg1
- rule: see "Refined rule" above.

## Measurement log (final)
1. O6.140 → 105.0
2. O6.220 → 127.0
3. O6.190 → 122.0
4. O6.240 → 123.0
5. O6.300 → 100.0
6. O6.210 → 127.0
7. O4.220 → 97.0
8. O4.162 → 97.4
9. O4.150 → 97.0
10. O4.103 → 92.5
11. O4.047 → 95.0
12. O4.081 → 98.0
13. O4.097 → 95.3
14. O4.101 → 93.4
15. O4.085 → 92.0
16. O4.096 → 95.6
17. O4.200 → 94.2
18. O7Cu3Ba2La1 → 89.6

## Measurement log
1. O6.140 → 105.0
2. O6.220 → 127.0
3. O6.190 → 122.0
4. O6.240 → 123.0
5. O6.300 → 100.0
6. O6.210 → 127.0
7. O4.220 → 97.0
8. O4.162 → 97.4
9. O4.150 → 97.0
10. O4.103 → 92.5
11. O4.047 → 95.0
12. O4.081 → 98.0
13. O4.097 → 95.3
14. O4.101 → 93.4
15. O4.085 → 92.0
16. O4.096 → 95.6
17. O4.200 → 94.2
18. (reserved) → O7Cu3Ba2La1
