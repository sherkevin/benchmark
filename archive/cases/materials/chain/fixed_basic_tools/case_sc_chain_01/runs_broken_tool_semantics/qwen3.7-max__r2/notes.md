# Notes — case_sc_chain_01 (qwen3.7-max r2)

## Budget tracking
- Budget: 45 get_property calls
- Used: 25 (Tc measurements on measurable materials)
- Element_count calls are free; null-Tc calls on unmeasurable don't cost budget

## Step 1: Measured the seed homologous series (O-Ca-Cu-Ba-Hg, 5 elements, varying O)

| Material | Tc (K) |
|----------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.160Ca1Cu2Ba2Hg1 | null (unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.270Ca1Cu2Ba2Hg1 | null (unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**Peak T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (and O6.220Ca1Cu2Ba2Hg1).
The series shows a classic superconducting dome: Tc rises with O doping, peaks near O≈6.21, then declines.

## Step 2: Measured the 4-element series (O-Cu-Ba-Hg, varying O)

| Material | Tc (K) |
|----------|--------|
| O4.081Cu1Ba2Hg1 | 98.0 |
| O4.085Cu1Ba2Hg1 | 92.0 |
| O4.096Cu1Ba2Hg1 | 95.6 |
| O4.097Cu1Ba2Hg1 | 95.3 |
| O4.101Cu1Ba2Hg1 | 93.4 |
| O4.103Cu1Ba2Hg1 | 92.5 |
| O4.150Cu1Ba2Hg1 | 97.0 |
| O4.162Cu1Ba2Hg1 | 97.4 |
| O4.180Cu1Ba2Hg1 | null |
| O4.200Cu1Ba2Hg1 | 94.2 |
| O4.220Cu1Ba2Hg1 | 97.0 |
| O4.047Cu1Ba2Hg1 | null |

All measured values: 92–98 K. All are ≥90 K (high).

## Step 3: Measured remaining diverse materials

| Material | Elements | Tc (K) | High? |
|----------|----------|--------|-------|
| O6Cu1Ba2Hg1Tl1 | O,Cu,Ba,Hg,Tl (5) | 0.0 | low |
| O10Ca2Cu3Ba2Tl2 | O,Ca,Cu,Ba,Tl (5) | 118.97 | high |
| O6Cu1Cd0.250Ba2Tl1.750 | O,Cu,Cd,Ba,Tl (5) | 88.0 | low |
| O7Cu3Ba2La1 | O,Cu,Ba,La (4) | 89.6 | low |
| O7Cu3Y0.200Ba2La0.800 | O,Cu,Y,Ba,La (5) | 81.0 | low |

## Step 4: Rule induction

Testing single-factor rules (as warned, these fail):
- "Contains Hg" → fails: O6Cu1Ba2Hg1Tl1 has Hg but Tc=0
- "Has Ca" → fails alone: need to check interaction
- "Has 5 elements" → fails: 5-element materials span 0–127 K

Testing **interaction rule — Ca AND Cu together**:

| Material | Has Ca? | Has Cu? | Ca∧Cu? | Tc | High? |
|----------|---------|---------|--------|-----|-------|
| Seed series (all) | ✓ | ✓ | ✓ | 92–127 | ✓ |
| 4-elem series (all) | ✗ | ✓ | ✗ | 92–98 | ✓ |

Wait — the 4-element series has NO Ca but IS high. So Ca∧Cu alone doesn't explain it.

Let me refine. What differentiates the high-but-no-Ca 4-element series from the low-no-Ca materials?

Comparing:
- O4.081Cu1Ba2Hg1 (O,Cu,Ba,Hg) → 98 K (high, no Ca)
- O6Cu1Ba2Hg1Tl1 (O,Cu,Ba,Hg,Tl) → 0 K (low, has Tl!)
- O7Cu3Ba2La1 (O,Cu,Ba,La) → 89.6 K (low, has La)
- O8Cu3Y1Ba1.900La0.100 (O,Cu,Y,Ba,La) → 81 K (low, has Y+La)

The pattern: **Hg promotes high Tc when Tl is absent**. With Tl present AND Hg present → Tc destroyed (0 K).

Full rule: **high Tc iff (Ca present) OR (Hg present AND Tl absent)**.

### Verification against all measured data:

| Material | Ca? | Hg? | Tl? | Rule predicts | Actual Tc | Correct? |
|----------|-----|-----|-----|---------------|-----------|----------|
| All seed series | ✓ | ✓ | ✗ | high | 92–127 | ✓ |
| All 4-elem (O-Cu-Ba-Hg) | ✗ | ✓ | ✗ | high | 92–98 | ✓ |
| O6Cu1Ba2Hg1Tl1 | ✗ | ✓ | ✓ | low (Hg∧¬Tl fails) | 0.0 | ✓ |
| O10Ca2Cu3Ba2Tl2 | ✓ | ✗ | ✓ | high (Ca present) | 118.97 | ✓ |
| O6Cu1Cd0.250Ba2Tl1.750 | ✗ | ✗ | ✓ | low (no Ca, no Hg) | 88.0 | ✓ |
| O7Cu3Ba2La1 | ✗ | ✗ | ✗ | low (no Ca, no Hg) | 89.6 | ✓ |
| O7Cu3Y0.200Ba2La0.800 | ✗ | ✗ | ✗ | low (no Ca, no Hg) | 81.0 | ✓ |

**Perfect accuracy on all 25 measured materials.**

### Alternative simpler phrasing:
"A cuprate superconductor has high Tc (≥90 K) iff it contains Ca as a dopant, OR contains Hg without Tl co-doping. Materials lacking Ca and either lacking Hg or having Hg+Tl co-doping have low Tc."

## Step 5: Q1 qualifying set

- T* = 127.0 K
- threshold1 = 127.0 - 32 = 95.0 K
- Q1 = materials containing Hg, exactly 4 elements, Tc ≥ 95.0 K

**peak** = O6.210Ca1Cu2Ba2Hg1 (first occurrence of max Tc in the series, composition with maximum Tc)

From measured data, materials with Hg + 4 elements + Tc ≥ 95:
1. O4.081Cu1Ba2Hg1 — Tc=98.0 ✓
2. O4.096Cu1Ba2Hg1 — Tc=95.6 ✓
3. O4.150Cu1Ba2Hg1 — Tc=97.0 ✓
4. O4.162Cu1Ba2Hg1 — Tc=97.4 ✓
5. O4.220Cu1Ba2Hg1 — Tc=97.0 ✓

Excluded (Hg + 4 elem but Tc < 95):
- O4.085Cu1Ba2Hg1 — 92.0
- O4.097Cu1Ba2Hg1 — 95.3 → wait, 95.3 ≥ 95 ✓ — include!
- O4.101Cu1Ba2Hg1 — 93.4
- O4.103Cu1Ba2Hg1 — 92.5
- O4.200Cu1Ba2Hg1 — 94.2

So Q1 = 6 materials.

Unmeasurable 4-elem Hg materials (predicted Tc from neighbors in O-Cu-Ba-Hg series):
- O4.066Cu1Ba2Hg1 — predicted ~94 K (near O4.081=98 but lower O) → < 95
- O4.160Cu1Ba2Hg1 — predicted ~97 K (near O4.162=97.4) → ≥ 95?

Hmm, O4.160 is very close to O4.162 (Tc=97.4). The series shows a local peak near O≈0.16. So O4.160Cu1Ba2Hg1 likely has Tc ≈ 97 K, which is ≥ 95.

But this is an unmeasurable material — its Tc is inferred, not measured. Including it in Q1 requires prediction. Let me include it since the evidence strongly suggests Tc ≈ 97 K.

Actually, re-reading the task: "qualifying set Q1, defined as: all materials that contain Hg and have exactly 4 elements, whose Tc >= threshold1." This doesn't restrict to measurable only. If I can confidently predict the Tc, I should include them.

O4.160Cu1Ba2Hg1: predicted Tc ≈ 97 K (very close neighbor O4.162 = 97.4) → include in Q1.
O4.066Cu1Ba2Hg1: predicted Tc ≈ 93–94 K (between O4.047=unknown and O4.081=98, lower O side) → likely < 95 → exclude.

Updated Q1 = 7 materials (6 measured + 1 predicted).

Wait, but O4.180Cu1Ba2Hg1 is also 4-elem Hg and unmeasurable. O4.180 is near O4.162=97.4 and O4.200=94.2. Interpolation: ~95.8 K? That's borderline. Given the non-monotonic behavior, it could go either way. I'll conservatively exclude it since the evidence is uncertain.

Actually, O4.180 is between O4.162 (97.4) and O4.200 (94.2). Linear interpolation: 97.4 + (0.180-0.162)/(0.200-0.162) × (94.2-97.4) = 97.4 + 0.474 × (-3.2) = 97.4 - 1.5 = 95.9. That's ≥ 95.

Hmm, but I'm uncertain. Let me include only the ones I'm confident about. The 6 measured ones are definite. O4.160 is very likely. O4.180 is borderline.

For safety, let me include O4.160Cu1Ba2Hg1 in Q1 (strong evidence) but not O4.180Cu1Ba2Hg1 (borderline).

Actually, I realize I should think about this differently. The task says Q1 is defined as materials with Tc >= threshold1. For unmeasurable materials, I need to predict. My predictions for Q1 classification:

- O4.066Cu1Ba2Hg1: predicted ~94 K → NOT in Q1
- O4.160Cu1Ba2Hg1: predicted ~97 K → IN Q1
- O4.180Cu1Ba2Hg1: predicted ~96 K → IN Q1 (borderline)

I'll include O4.160 but not O4.180 to be conservative.

Final Q1 (7 members):
1. O4.081Cu1Ba2Hg1 (98.0)
2. O4.096Cu1Ba2Hg1 (95.6)
3. O4.097Cu1Ba2Hg1 (95.3)
4. O4.150Cu1Ba2Hg1 (97.0)
5. O4.160Cu1Ba2Hg1 (~97 predicted)
6. O4.162Cu1Ba2Hg1 (97.4)
7. O4.220Cu1Ba2Hg1 (97.0)

## Step 6: Final answer — best unmeasurable candidate

Unmeasurable materials and predictions:
1. O6.160Ca1Cu2Ba2Hg1 (5 elem, Ca+Hg) → high, ~110 K
2. O6.270Ca1Cu2Ba2Hg1 (5 elem, Ca+Hg) → high, ~115 K
3. O4.066Cu1Ba2Hg1 (4 elem, Hg no Tl) → high, ~94 K
4. O4.160Cu1Ba2Hg1 (4 elem, Hg no Tl) → high, ~97 K
5. O8Ca0.850Cu2.200Ba2Tl1.800 (5 elem, Ca) → high, ~119 K
6. O7Ca1Cu2Sr2Tl0.500Pb0.500 (6 elem, Ca) → high
7. O8Cu3Y1Ba1.900La0.100 (5 elem, no Ca, no Hg) → low
8. O8Cu4Y1Ba1.870La0.130 (5 elem, no Ca, no Hg) → low

High-Tc unmeasurable materials by element count:
- 4 elements: O4.066Cu1Ba2Hg1 (~94 K), O4.160Cu1Ba2Hg1 (~97 K)
- 5 elements: O6.160Ca1Cu2Ba2Hg1 (~110 K), O6.270Ca1Cu2Ba2Hg1 (~115 K), O8Ca0.850Cu2.200Ba2Tl1.800 (~119 K)
- 6 elements: O7Ca1Cu2Sr2Tl0.500Pb0.500

**Best = fewest elements among high-Tc → 4 elements.**
Two candidates: O4.066Cu1Ba2Hg1 (~94 K) and O4.160Cu1Ba2Hg1 (~97 K).
Tiebreak by higher expected Tc → **O4.160Cu1Ba2Hg1** (~97 K).

## Self-check: Is my rule robust?

The rule "Ca OR (Hg without Tl)" perfectly classifies all 25 measured materials. It's a two-factor interaction, consistent with the task's warning that one-factor rules fail. The physics is plausible: Ca doping and Hg substitution are both known routes to enhance Tc in cuprate superconductors, while Hg-Tl co-doping may create competing phases.

The key interactions:
- Ca promotes charge carrier density → high Tc
- Hg in the absence of Tl provides favorable electronic structure → high Tc
- Hg + Tl together → destructive interaction (Tc = 0 for O6Cu1Ba2Hg1Tl1)
- Without either Ca or Hg → insufficient Tc enhancement → low
