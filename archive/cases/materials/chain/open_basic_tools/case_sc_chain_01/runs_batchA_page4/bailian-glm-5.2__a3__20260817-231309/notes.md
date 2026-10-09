# Superconductor Tc study — running notes

## Task overview
- Budget: 45 units total (shared: search_db @1/page, measure_tc @1/measure).
  NOTE: measure_tc on a non-existent sample returns "no sample" and is FREE (doesn't consume budget).
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements O, Ca, Cu, Ba, Hg = 5 elements).
- High = Tc >= 90 K; Low = Tc < 90 K.
- Step A: homologous doping series (same element set, varying O) → find MAX M = T*.
- Step B: threshold1 = T* - 32. Q1 = Hg-containing + exactly 4 elements + Tc >= threshold1.
- Step C: predict high/low for every unmeasurable material discovered. Multi-factor rule needed.
- Step D: among predicted-HIGH unmeasurables, pick FEWEST elements (tie-break higher expected Tc).

## Searches
- search_db("Hg") pages 1–7: 27 total Hg materials (cost 7). Fully enumerated (page 7 has_more=false).

## All 27 Hg materials
### Series A — O6.x Ca1Cu2Ba2Hg1 (5 elements; seed's homologous series)
| O     | Tc    | status |
|-------|-------|--------|
| 6.080 | 92.0  | HIGH (measured) |
| 6.100 | 92.5  | HIGH |
| 6.130 | 102.0 | HIGH |
| 6.140 | 105.0 | HIGH |
| 6.160 | —     | UNMEASURABLE |
| 6.190 | 122.0 | HIGH |
| 6.210 | 127.0 | HIGH ← peak (tied) |
| 6.220 | 127.0 | HIGH ← peak (tied) |
| 6.240 | 123.0 | HIGH |
| 6.270 | —     | UNMEASURABLE |
| 6.300 | 100.0 | HIGH |
| 6.310 | 98.0  | HIGH |

Dome: rises 92→127 (peak ~6.21–6.22), falls to 98. **T* = 127.** Peak comp = O6.210Ca1Cu2Ba2Hg1 (tied with O6.220).

### Series B — O4.x Cu1Ba2Hg1 (4 elements; Hg + exactly 4 elements → Step B candidates)
| O     | Tc    | >=95? (Q1) |
|-------|-------|------------|
| 4.047 | —     | UNMEASURABLE |
| 4.066 | —     | UNMEASURABLE |
| 4.081 | 98.0  | YES |
| 4.085 | 92.0  | no |
| 4.096 | 95.6  | YES |
| 4.097 | 95.3  | YES |
| 4.101 | 93.4  | no |
| 4.103 | 92.5  | no |
| 4.150 | 97.0  | YES |
| 4.160 | —     | UNMEASURABLE |
| 4.162 | 97.4  | YES |
| 4.180 | —     | UNMEASURABLE |
| 4.200 | 94.2  | no |
| 4.220 | 97.0  | YES |

### Other
- O6Cu1Ba2Hg1Tl1 (5 elements; has Tl): Tc = 0.0 → LOW. Measurable.

## Step A
T* = 127 at O6.210Ca1Cu2Ba2Hg1 (tied with O6.220Ca1Cu2Ba2Hg1). Report O6.210Ca1Cu2Ba2Hg1.

## Step B
threshold1 = 127 - 32 = 95.
Q1 (Hg, exactly 4 elements, measured Tc >= 95) = {
  O4.081Cu1Ba2Hg1 (98.0),
  O4.096Cu1Ba2Hg1 (95.6),
  O4.097Cu1Ba2Hg1 (95.3),
  O4.150Cu1Ba2Hg1 (97.0),
  O4.162Cu1Ba2Hg1 (97.4),
  O4.220Cu1Ba2Hg1 (97.0)
}
(Unmeasurable O4 members have no measured Tc → handled in Step C, not Q1.)

## Step C — unmeasurable materials discovered (6 total, all from Hg search)
1. O6.160Ca1Cu2Ba2Hg1 — in O6 dome (between 6.140=105 and 6.190=122) → ~115 → HIGH
2. O6.270Ca1Cu2Ba2Hg1 — in O6 dome (between 6.240=123 and 6.300=100) → ~110 → HIGH
3. O4.047Cu1Ba2Hg1 — oxygen 0.047 < 0.08 (below dome lower edge) → LOW
4. O4.066Cu1Ba2Hg1 — oxygen 0.066 < 0.08 → LOW
5. O4.160Cu1Ba2Hg1 — in O4 range (between 4.150=97.0 and 4.162=97.4) → ~97.3 → HIGH
6. O4.180Cu1Ba2Hg1 — in O4 range (between 4.162=97.4 and 4.200=94.2) → ~95.9 → HIGH

## Induced rule (multi-factor)
HIGH iff (Hg-site is a pure Hg layer, i.e., NO Tl) AND (oxygen excess >= ~0.08, i.e., inside the superconducting dome).
- Tl co-substitution at the Hg site (e.g. Hg1Tl1) collapses Tc to 0 → LOW (O6Cu1Ba2Hg1Tl1: Tc=0).
- Heavily underdoped samples with oxygen excess < ~0.08 fall below the dome's lower edge → LOW (O4.047, O4.066).
- Otherwise HIGH: O6 dome spans 0.080–0.310 (Tc 92–127, peak 127); O4 dome spans 0.081–0.220 (Tc ~92–98).
Why multi-factor: the single-factor rule "no Tl → HIGH" would wrongly flag O4.047 & O4.066 as HIGH; the oxygen-doping (second factor) makes them LOW. Conversely "oxygen >= 0.08 → HIGH" alone would be fine on measured set but the Tl factor is needed to mark the Tl material LOW. Lowest measured HIGH in both series sits at oxygen ~0.080–0.081 → 0.08 is the induced dome floor.

## Step D — final pick
Predicted-HIGH unmeasurables: {O6.160 (5 elem), O6.270 (5 elem), O4.160 (4 elem), O4.180 (4 elem)}.
Fewest elements → 4-element ones: O4.160Cu1Ba2Hg1, O4.180Cu1Ba2Hg1.
Tie-break higher expected Tc: O4.160 ~97.3 > O4.180 ~95.9.
**Final = O4.160Cu1Ba2Hg1.**

## Budget
Total used: 7 (search) + 21 (measurable measurements; unmeasurable attempts were free) = 28. Remaining 17. Sufficient.
