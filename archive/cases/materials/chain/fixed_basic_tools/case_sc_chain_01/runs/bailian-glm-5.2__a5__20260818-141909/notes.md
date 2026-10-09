# Superconductor Tc Study Notes

## Task overview
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- High = Tc >= 90 K; Low = Tc < 90 K
- Budget: 18 measure_tc calls
- Steps A (series peak), B (filter Hg + exactly 4 elements >= threshold1), C (induce rule, predict unmeasurable), D (fewest elements among predicted-high)

## Compositions in measurable.txt (27 lines)
Series 1 (Hg-Cu-Ba-Ca-O, varying O): O6.080 ... O6.310 Ca1Cu2Ba2Hg1 (10 entries)
Series 2 (Hg-Cu-Ba-O, varying O): O4.081 ... O4.085 Cu1Ba2Hg1 (11 entries)
Other:
- O6Cu1Ba2Hg1Tl1
- O10Ca2Cu3Ba2Tl2
- O6Cu1Cd0.250Ba2Tl1.750
- O7Cu3Ba2La1
- O7Cu3Y0.200Ba2La0.800

## Compositions in unmeasurable.txt (7)
1. O6.160Ca1Cu2Ba2Hg1  (Hg-Ba-Cu-Ca-O series, gap fill)
2. O6.270Ca1Cu2Ba2Hg1  (same series, gap fill)
3. O4.180Cu1Ba2Hg1     (Hg-Ba-Cu-O series, gap fill)
4. O8Ca0.850Cu2.200Ba2Tl1.800  (Tl-Ba-Ca-Cu-O)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500    (Tl-Pb-Sr-Ca-Cu-O)
6. O8Cu3Y1Ba1.900La0.100        (Y-Ba-La-Cu-O)
7. O8Cu4Y1Ba1.870La0.130        (Y-Ba-La-Cu-O)

## Strategy
- Budget 18 calls. measurable has 27 compositions. Can't measure all.
- Step A: Need MAX Tc in the series in measurable.txt. Likely peaks are the Hg-1223-like family. Need to be smart.
- Step A "the series" = the homologous doping series the seed belongs to = O(x)Ca1Cu2Ba2Hg1 family (10 members). Measure all 10 to find max = T*.
- Then Step B: among materials containing Hg AND exactly 4 elements, Tc >= threshold1 (T*-32). The Series 2 (O4.x Cu1Ba2Hg1) has exactly 4 elements (O,Cu,Ba,Hg). Need their Tc. The Series 1 has 5 elements (O,Ca,Cu,Ba,Hg) -> NOT in B.
  - Wait: "exactly 4 elements" — count distinct elements. Series2: O,Cu,Ba,Hg = 4 elements. Good.
  - Others with Hg + 4 elements? None of the "other" have Hg except none. O6Cu1Ba2Hg1Tl1 has 5 (O,Cu,Ba,Hg,Tl). So only Series 2 qualifies for B.
- So I need to measure Series 2 members to know which are >= threshold1.

Budget plan:
- Series 1: 10 members -> 10 calls (for Step A max).
- Series 2: 11 members but I need those >= threshold1. Threshold = T* - 32. If T* ~ 120ish, threshold ~88. Need to measure Series 2 to find which >= threshold. That's 11 more = 21 total. Too many (budget 18).

Need to be clever. Let me first measure Series 1 to find T*. Then measure Series 2. 10+11=21 > 18. Need to cut.

Alternative: Maybe Series 2 max is around the same doping peak. I can measure Series 2 and find which exceed threshold. But 11 is a lot.

Actually budget = 18. 10 (series1) + 11 (series2) = 21. Over by 3.

Hmm. But Step C needs NO measurements (predict only, measure returns no sample). So all 18 can go to A+B.

Could I skip some Series 1 members? The peak of a doping series — I should measure enough to find the max. But to be safe (the task says "measure the series members"), I should measure all 10. Let me reconsider — maybe I don't need ALL Series 2 for B. Step B asks to report ALL with Tc >= threshold1. I must measure to know. But maybe threshold is high enough that only a few exceed.

Let me think about budget more carefully. 18 calls. If Series 1 = 10 and I measure all, I have 8 left for Series 2 (11). I'd miss 3.

Better: Measure Series 1 (10) to get T*. Then threshold = T*-32. Then measure Series 2 strategically. But I must report ALL qualifying, so missing some could miss a qualifier.

Alternative interpretation: maybe "the series" in Step A is broader. But seed is O6.080Ca1Cu2Ba2Hg1, and "same element set, varying oxygen" = the Ca1Cu2Ba2Hg1 with varying O. That's the 10 members lines 1-10. Clear.

Let me just measure Series 1 first (10 calls), then decide. Possibly the structure shows threshold high enough that few Series 2 exceed, and I sample Series 2 around its expected peak.

Actually, the cleanest: measure all 10 of Series 1, then measure Series 2 but I only have 8 calls. Let me see the oxygen values of Series 2: 4.047,4.081,4.085,4.096,4.097,4.101,4.103,4.150,4.162,4.200,4.220. 11 values clustered around 4.08-4.22.

I'll measure Series 1 (10), then the 8 "middle" of Series 2 likely near peak. But risk missing qualifiers at ends.

Let me reconsider total: maybe I can find T* without all 10 of Series 1 if the peak is obvious. But "measure the series members" implies all. And to claim MAX I need all.

Decision: Measure all 10 Series 1 + as many Series 2 as budget allows (8), prioritizing ones near the expected peak. Then for the 3 unmeasured Series 2, I'll need to reason. Actually, if I can induce the rule in Step C, maybe the rule tells me about the doping peak shape and I can interpolate for unmeasured Series 2... but Step B requires measured Tc (must use tool, never guess).

Tight. Let me start measuring Series 1 and reassess as data comes in.

## Measurements

### Step A — Series 1 (O(x)Ca1Cu2Ba2Hg1)
| O | Tc |
|---|---|
| 6.080 | 92.0 |
| 6.100 | 92.5 |
| 6.130 | 102.0 |
| 6.140 | 105.0 |
| 6.190 | 122.0 |
| 6.210 | 127.0 | <-- MAX |
| 6.220 | 127.0 | <-- MAX (tie) |
| 6.240 | 123.0 |
| 6.300 | 100.0 |
| 6.310 | 98.0 |

T* = 127 K. Peak composition = O6.210Ca1Cu2Ba2Hg1 (first to hit max; tie with O6.220).
threshold1 = 127 - 32 = 95 K.
Budget used: 10 / 18. Remaining: 8.

### Step B — filter Hg & exactly 4 elements, Tc >= 95
Candidates (contain Hg, exactly 4 distinct elements):
- Series 2: O4.x Cu1Ba2Hg1  (elements: O,Cu,Ba,Hg = 4) ✓ — 11 members
- O6Cu1Ba2Hg1Tl1 has 5 elements (O,Cu,Ba,Hg,Tl) -> excluded
- All other "other" measurables lack Hg -> excluded
So only Series 2 is in scope for B.

Series 2 oxygen values (sorted): 4.047, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.162, 4.200, 4.220.
Note: dense cluster 4.081–4.103 (6 values) = experimenter's peak-region focus (Hg-1201, real Tc ~97 K).
With 8 remaining calls for 11 members, measure all 6 cluster + shoulder brackets 4.047 (underdoped) and 4.150 (just overdoped of cluster). If both shoulders < 95, qualifiers ⊆ cluster (all measured). Skipped 4.162/4.200/4.220 lie further on overdoped side; if 4.150 < 95 and dome declines monotonically past peak, these are < 95 too.

### Series 2 results (O4.x Cu1Ba2Hg1)
| O | Tc | >=95? |
|---|----|-------|
| 4.047 | 95.0 | YES (=95) |
| 4.081 | 98.0 | YES |
| 4.085 | 92.0 | no |
| 4.096 | 95.6 | YES |
| 4.097 | 95.3 | YES |
| 4.101 | 93.4 | no |
| 4.103 | 92.5 | no |
| 4.150 | 97.0 | YES |
| 4.162 | not measured | ? |
| 4.200 | not measured | ? |
| 4.220 | not measured | ? |

Series 2 dome shape: rises 4.047(95)->4.081(98 peak so far), dips to 4.085(92), back up 4.096(95.6)->4.097(95.3), declines 4.101(93.4)->4.103(92.5), then 4.150(97) a SECOND local peak.

**CRITICAL**: Tc is NOT monotonic past the first peak — 4.150 = 97 is a strong second peak (>=95). So 4.162/4.200/4.220 (further overdoped) CANNOT be assumed below threshold. The "dome declines monotonically" assumption is REFUTED. I have no budget to measure them, but Step B requires measured Tc (rule: "never guess a measured number"). These 3 are genuinely unknown.

Confirmed qualifiers (measured >=95): O4.047Cu1Ba2Hg1, O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1.
Ambiguous (unmeasured): O4.162Cu1Ba2Hg1, O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1.

PRACTICAL CORRECTION: the dense cluster in measurable.txt (4.047,4.081,4.085,4.096,4.097,4.101,4.103 — the experimenter deliberately sampled the 4.04–4.10 peak region of Hg-1201) signals that the high-Tc region of this family sits around O≈4.05–4.10, and the sparse tail (4.150,4.162,4.200,4.220) is the overdoped decline. 4.150=97 may be a measurement/real local bump, but the experimenter's focus and the overall decline 4.101->4.103 suggest 4.162/4.200/4.220 trend down. Best evidence-based guess: 4.162 borderline-ish, 4.200 & 4.220 below 95. Cannot be certain. I'll note this honestly and use measured-only set as Q1, flagging the 3 as unverified.

**Q1 (measured & verified Tc >= 95)**:
- O4.047Cu1Ba2Hg1 (95.0)
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
Unverified (no budget): O4.162, O4.200, O4.220 — beyond the 4.150 local peak on the overdoped side; likely below 95 (declining trend 4.150->4.200) but cannot be cited per "never guess a measured number."

## Step C — induce rule & predict unmeasurable (threshold for high/low = 90 K)

Structural identification of measured families:
- Series 1 (Ca1Cu2Ba2Hg1, O6.x) = Hg-1212 (n=2 CuO2 layers); peak 127 K (literature Hg-1212 ≈127 K ✓). 5 elements.
- Series 2 (Cu1Ba2Hg1, O4.x) = Hg-1201 (n=1 CuO2 layer); peak ~98 K (literature Hg-1201 ≈97 K ✓). 4 elements.
Both families are uniformly >=90 across the measured range → all measured are HIGH. So "high" anchors = Hg-cuprates near optimal O.

Unmeasurable analysis (use cuprate-structure + doping domain knowledge):
1. O6.160Ca1Cu2Ba2Hg1 — Hg-1212, O between 6.140(105) & 6.190(122) → ~113 K. HIGH.
2. O6.270Ca1Cu2Ba2Hg1 — Hg-1212, O between 6.240(123) & 6.300(100) → ~110 K. HIGH.
3. O4.180Cu1Ba2Hg1 — Hg-1201, O just past 4.150(97) peak; neighbors 4.103(92.5), 4.150(97) → ~92–95 K. HIGH (>=90, with margin; 90-threshold not 95 resolves Step B tension).
4. O8Ca0.850Cu2.200Ba2Tl1.800 — Tl≈1.8 (double Tl layer), O8, Ba2, Ca, Cu2 → Tl-2212 analog (Tl2Ba2CaCu2O8, ~108 K). HIGH.
5. O7Ca1Cu2Sr2Tl0.500Pb0.500 — Tl0.5+Pb0.5=1 (single mixed layer), Sr2 (not Ba), O7, Ca1Cu2 → (Tl,Pb)Sr-1212 analog (~70–85 K; Sr + single-layer + 2 CuO2). LOW.
6. O8Cu3Y1Ba1.900La0.100 — YBa2Cu3O7-123 with La on Ba site (10%); La-on-Ba disorder suppresses Tc (~85–88 K). LOW.
7. O8Cu4Y1Ba1.870La0.130 — YBa2Cu4O8 = Y-124 (Cu4, O8), inherently ~81 K, + La disorder → LOW.

Check against hint "single-factor rules wrong on several":
- "contains Tl → high" → wrong on #5 (low). ✓
- "has 5 elements → high" (5-elem: 1,2,4,6,7) → wrong on #3 (high, 4 elem), #6 & #7 (low, 5 elem). Wrong on 3. ✓
- "contains Hg → high" → correct on 1,2,3 (all high); not "wrong on several" but that's fine, hint says single-factor rules are wrong on several OF THEM (the unmeasurables), not that every single-factor rule fails.

Multi-factor rule (interaction): {charge-reservoir cation & layer count} × {alkaline-earth Ba vs Sr} × {oxygen content} × {Cu-layer / structure type}, plus absence of suppressive disorder (La on Ba).

Predicted HIGH: {1,2,3,4}; LOW: {5,6,7}.

## Step D — final pick
Predicted-high unmeasurables and element counts:
- #1 O6.160Ca1Cu2Ba2Hg1: 5 elements (O,Ca,Cu,Ba,Hg)
- #2 O6.270Ca1Cu2Ba2Hg1: 5 elements
- #3 O4.180Cu1Ba2Hg1: 4 elements (O,Cu,Ba,Hg) ← fewest
- #4 O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements (O,Ca,Cu,Ba,Tl)
Final = O4.180Cu1Ba2Hg1 (4 elements, no tie). Expected Tc ~92–95 K.

## Summary for answer.json
- peak: O6.210Ca1Cu2Ba2Hg1
- peak_tc: 127
- threshold: 95
- qualifying: 5 measured members above
- predictions: 7 entries (1-4 high, 5-7 low)
- final: O4.180Cu1Ba2Hg1
- rule: multi-factor interaction described above

## Budget reconciliation
18 calls used exactly:
- Step A Series 1: 10 (6.080,6.100,6.130,6.140,6.190,6.210,6.220,6.240,6.300,6.310)
- Step B Series 2: 8 (4.047,4.081,4.085,4.096,4.097,4.101,4.103,4.150)
Total = 18. Step C = 0 (predictions only; unmeasurable have no sample).

## Step B caveat (recorded for honesty)
3 Series-2 members (4.162, 4.200, 4.220) could not be measured (budget). Because Series-2 Tc is non-monotonic (4.150=97 after 4.103=92.5), I cannot rule them out from Q1 with certainty. I report Q1 as the measured-and-verified members only, consistent with the task's "never guess a measured number" rule. The 3 are flagged here, not in Q1.

## Predicted vs measured cross-check for high/low boundary (90 K)
Step C uses 90 K cutoff (task: high >= 90). #3 O4.180 interpolated ~92–95 → high (with margin). All Series-2 measured values fall 92–98 K, all >=90 → high, consistent with #3 high.




