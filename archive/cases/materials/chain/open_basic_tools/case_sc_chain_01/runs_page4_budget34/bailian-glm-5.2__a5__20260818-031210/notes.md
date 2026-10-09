# Superconductor Tc study — notes

## Task summary
- Seed: `O6.080Ca1Cu2Ba2Hg1`
- Budget: 34 units total (shared between measure_tc and search_db)
- "high" = Tc >= 90 K
- Steps A-D, write answer.json at the end

## Plan
- Step A: seed is in a homologous doping series (same elements, varying oxygen). Find series members by searching, measure each, find max Tc = T*.
- Step B: threshold1 = T* - 32. Among Hg-containing materials with exactly 4 elements, report all with Tc >= threshold1.
- Step C: induce rule from measurable materials, predict high/low for every unmeasurable one found via search.
- Step D: among predicted-high unmeasurable materials, pick fewest elements (tie-break: higher expected Tc).

## Budget tracking
- Starting budget: 34

## Search/measurement log

### Step A — series members (query "Ca1Cu2Ba2Hg1", 12 matches over 3 pages)
Series members (varying oxygen):
- O6.080Ca1Cu2Ba2Hg1 (seed)
- O6.100Ca1Cu2Ba2Hg1
- O6.130Ca1Cu2Ba2Hg1
- O6.140Ca1Cu2Ba2Hg1
- O6.160Ca1Cu2Ba2Hg1
- O6.190Ca1Cu2Ba2Hg1
- O6.210Ca1Cu2Ba2Hg1
- O6.220Ca1Cu2Ba2Hg1
- O6.240Ca1Cu2Ba2Hg1
- O6.270Ca1Cu2Ba2Hg1
- O6.300Ca1Cu2Ba2Hg1
- O6.310Ca1Cu2Ba2Hg1

Measurements:
- O6.080Ca1Cu2Ba2Hg1 = 92.0 K (seed)
- O6.100Ca1Cu2Ba2Hg1 = 92.5 K
- O6.130Ca1Cu2Ba2Hg1 = 102.0 K
- O6.140Ca1Cu2Ba2Hg1 = 105.0 K
- O6.160Ca1Cu2Ba2Hg1 = UNMEASURABLE (no sample)
- O6.190Ca1Cu2Ba2Hg1 = 122.0 K
- O6.210Ca1Cu2Ba2Hg1 = 127.0 K  ← MAX
- O6.220Ca1Cu2Ba2Hg1 = 127.0 K  ← MAX (tie)
- O6.240Ca1Cu2Ba2Hg1 = 123.0 K
- O6.270Ca1Cu2Ba2Hg1 = UNMEASURABLE (no sample)
- O6.300Ca1Cu2Ba2Hg1 = 100.0 K
- O6.310Ca1Cu2Ba2Hg1 = 98.0 K

T* = 127.0 K (peak; both O6.210 and O6.220 tie at 127.0; curve symmetric around apex ~6.215).
Peak composition reported: O6.220Ca1Cu2Ba2Hg1 (Tc=127.0). Note O6.210 also = 127.0.

Budget used: 14, remaining: 20.

Curve: rises 92→127 then falls 127→98. Unmeasurable points: 6.160 (between 105 and 122) and 6.270 (between 123 and 100). By interpolation: 6.160 ≈ 113-115 K (high); 6.270 ≈ 111-112 K (high).

### KEY OBSERVATION — unmeasurable measurements are FREE
From budget_used values: measurable measure_tc increments budget by 1; unmeasurable (null) does NOT increment.
- 6.140 → budget_used 8; 6.160 (unmeas) → budget_used 8 (no change)
- 6.240 → 12; 6.270 (unmeas) → 12 (no change)
So unmeasurable samples cost 0 budget. This is critical for Step B/C planning.

### Step B — Hg + exactly 4 elements
Searched "Hg" (7 pages, 27 results, budget 14→21).
Element counts:
- O4.xxxCu1Ba2Hg1 (14 of them) → O, Cu, Ba, Hg = exactly 4 elements ✓
- O6.xxxCa1Cu2Ba2Hg1 (12) → 5 elements ✗ (also Ca)
- O6Cu1Ba2Hg1Tl1 → 5 elements ✗ (Tl)

So Q1 candidates = the 14 O4.xxxCu1Ba2Hg1 materials.
threshold1 = T* - 32 = 127 - 32 = 95.0 K.

O4 series (sorted by oxygen):
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220

Need Tc for each to determine Q1 (Tc >= 95). Budget remaining 13. With unmeasurable=free, should fit.

Hg "Hg" search budget tracking: 14 (post Step A) → +7 pages = 21 used, 13 remaining.

### Unmeasurable materials discovered so far (for Step C predictions)
- O6.160Ca1Cu2Ba2Hg1 (from Step A)
- O6.270Ca1Cu2Ba2Hg1 (from Step A)
More may surface while measuring the O4 series.

### O4 series measurements (Step B/C data)
threshold1 = 95.0 K.
- O4.047 → unmeasurable
- O4.066 → unmeasurable
- O4.081 → 98.0 ✓ (>=95)
- O4.085 → 92.0 ✗
- O4.096 → 95.6 ✓
- O4.097 → 95.3 ✓
- O4.101 → 93.4 ✗
- O4.103 → 92.5 ✗
- O4.150 → 97.0 ✓
- O4.160 → unmeasurable
- O4.162 → 97.4 ✓
- O4.180 → unmeasurable
- O4.200 → 94.2 ✗
- O4.220 → 97.0 ✓

Budget after O4 measurements: 31 used, 3 remaining.

### Q1 (Step B answer) — Hg + exactly 4 elements, Tc >= 95.0
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

### Full unmeasurable list (Step C predictions) — all from search_db
A. O6.160Ca1Cu2Ba2Hg1
B. O6.270Ca1Cu2Ba2Hg1
C. O4.047Cu1Ba2Hg1
D. O4.066Cu1Ba2Hg1
E. O4.160Cu1Ba2Hg1
F. O4.180Cu1Ba2Hg1
(plus possibly more from non-Hg searches — but budget=3, so I will NOT do more searches; these 6 are all unmeasurables discovered.)
WARNING: Task says predictions must include ALL unmeasurables discovered via search_db. I have only searched "Hg" (and the seed exact + Ca1Cu2Ba2Hg1). To be safe re "ALL unmeasurable materials you discovered via search_db" → only those found in my searches count. 6 total.

### Step C — rule induction
ALL measured materials (20 of them) are HIGH (≥90): every Hg material measured is ≥90 K.
So a naive single-factor rule ("contains Hg → high" / "exactly 4 elements → high" / "Cu1 → high") would predict ALL 6 unmeasurables as HIGH. The task warns this is WRONG on several → several unmeasurables must be LOW.

Chemistry of Hg-cuprates (matches the data):
- O6·Ca1·Cu2·Ba2·Hg1 = Hg-1212 family, peak ≈127 K (matches measured T*=127). Tall, BROAD dome.
  Measured across O6.08–6.31: 92–127 K, all ≥90 → entire measured range is HIGH.
- O4·Cu1·Ba2·Hg1 = Hg-1201 family, peak ≈94–98 K (matches measured ~92–98). LOW, NARROW dome.
  HIGH only near optimal doping (O≈4.08–4.22); underdoped tail at low O (below onset ~4.07) → LOW (<90, insulating).

MULTI-FACTOR RULE: high/low = family (Cu-layer count / Ca presence / O base) × oxygen-doping position on that family's dome.
- Hg-1212 (5 elem, O6, Cu2, Ca1): dome tall & broad → all members HIGH.
- Hg-1201 (4 elem, O4, Cu1, no Ca): dome low → HIGH only inside optimal window; LOW in underdoped tail.

This is why single-factor rules fail: "4 elements → high" / "Cu1 → high" / "Hg → high" all wrongly label the Hg-1201 underdoped tail as high.

### Step C — predictions
- O6.160Ca1Cu2Ba2Hg1 → HIGH (Hg-1212, mid-dome; interpolate 6.140=105,6.190=122 → ≈113)
- O6.270Ca1Cu2Ba2Hg1 → HIGH (Hg-1212, mid-dome; interpolate 6.240=123,6.300=100 → ≈111)
- O4.047Cu1Ba2Hg1 → LOW (Hg-1201, heavily underdoped, O below dome onset)
- O4.066Cu1Ba2Hg1 → LOW (Hg-1201, underdoped, at/below dome onset ≈4.07)
- O4.160Cu1Ba2Hg1 → HIGH (Hg-1201, optimal; interpolate 4.150=97.0,4.162=97.4 → ≈97.3)
- O4.180Cu1Ba2Hg1 → HIGH (Hg-1201, optimal; interpolate 4.162=97.4,4.200=94.2 → ≈96.1)

### Step D — final pick
Predicted-HIGH unmeasurables: O4.160, O4.180, O6.160, O6.270.
Fewest elements: 4 → O4.160 (4 elem) and O4.180 (4 elem) tie. (O6 ones have 5.)
Tie-break higher expected Tc: O4.160 (≈97.3) > O4.180 (≈96.1).
FINAL = O4.160Cu1Ba2Hg1.

(Robust: even if O4.047/O4.066 were called high, their expected Tc < 97.3, so O4.160 still wins.)

### Step A peak (revisited)
Measured max = 127.0 K, tie between O6.210 and O6.220 (apex ≈6.215). Report O6.220Ca1Cu2Ba2Hg1, T*=127.0. Unmeasurable 6.160/6.270 are mid-dome (~113/111), not the peak.

### FINAL VALUES
- peak: O6.220Ca1Cu2Ba2Hg1
- peak_tc: 127.0
- threshold: 95.0
- qualifying (Q1, measured Tc≥95, Hg+4elem): O4.081, O4.096, O4.097, O4.150, O4.162, O4.220
- predictions: 6 entries above
- final: O4.160Cu1Ba2Hg1
- rule: multi-factor (family × doping) as above

Budget final: 31/34 used.

