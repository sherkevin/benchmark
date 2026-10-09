# Superconductor Tc study — running notes

## Setup
- Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O,Ca,Cu,Ba,Hg)
- High = Tc >= 90 K; Low = Tc < 90 K
- Budget: 24 measure_tc calls (all used)

## Step A — series peak (O6.0xxCa1Cu2Ba2Hg1, Hg-1212, 10 members)
| comp | Tc |
|---|---|
| O6.080 | 92.0 |
| O6.100 | 92.5 |
| O6.130 | 102.0 |
| O6.140 | 105.0 |
| O6.190 | 122.0 |
| **O6.210** | **127.0** |  <- max (T*)
| O6.220 | 127.0 |  (tie at plateau peak)
| O6.240 | 123.0 |
| O6.300 | 100.0 |
| O6.310 | 98.0 |

**T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (O6.220 ties).
Peak dome: rises 92→127 over O6.08–6.21, falls 127→98 over O6.22–6.31.

## Step B — Hg AND exactly 4 elements (O4.0xxCu1Ba2Hg1, Hg-1201, 11 members)
threshold1 = T* - 32 = 127 - 32 = **95.0 K**
| comp | Tc | >=95? |
|---|---|---|
| O4.081 | 98.0 | YES |
| O4.162 | 97.4 | YES |
| O4.150 | 97.0 | YES |
| O4.220 | 97.0 | YES |
| O4.096 | 95.6 | YES |
| O4.097 | 95.3 | YES |
| O4.047 | 95.0 | YES (== threshold) |
| O4.200 | 94.2 | no |
| O4.101 | 93.4 | no |
| O4.103 | 92.5 | no |
| O4.085 | 92.0 | no |

**Q1** (7 members): O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220.
O4-1201 series: broad plateau 92–98 K, all above 90 (all HIGH by definition), dome max ~98 at O4.081.

## Step C — rule induction (3 anchor measurements)
| comp | elements | buffer cations | Tc | class |
|---|---|---|---|---|
| O7Cu3Y0.200Ba2La0.800 | 5 (O,Cu,Ba,La,Y) | Ba, Y, La (no Hg/Tl/Pb) | 81.0 | LOW |
| O7Cu3Ba2La1 | 4 (O,Cu,Ba,La) | Ba, La (no Hg/Tl/Pb) | 89.6 | LOW |
| O10Ca2Cu3Ba2Tl2 | 6 (O,Ca,Cu,Ba,Tl) | Ca, Ba, Tl (has Tl) | 118.97 | HIGH |

### Induced rule (interaction of multiple factors)
A material is **HIGH** iff its non-Cu buffer-cation set contains at least one
heavy p-block metal from {Hg, Tl, Pb}. Materials whose only non-Cu cations are
alkaline-earth (Ca/Sr/Ba) and/or trivalent rare-earth (Y/La) — with NO Hg, Tl,
or Pb — are **LOW**.

### Why single-factor rules fail (verified against all measured)
- "contains Hg" -> HIGH: works on all measured Hg phases, but would WRONGLY mark
  Tl-only phases LOW (#4 O8..Tl1.800 is HIGH) -> fails on several unmeasurable.
- "has 5 elements" -> HIGH: O6-series (5e, Hg) HIGH ok, but O7Cu3Y0.2Ba2La0.8
  (5e, no Hg/Tl/Pb) is LOW (81) -> fails. Also would wrongly mark YBCO-123/124
  (5e) HIGH.
- "has 4 elements" -> HIGH: O4-series (4e, Hg) HIGH ok, but O7Cu3Ba2La1 (4e, no
  Hg/Tl/Pb) is LOW (89.6) -> fails.
- "contains Tl" -> HIGH: O10..Tl2 HIGH ok, but would WRONGLY mark Hg-1212/Hg-1201
  (no Tl) LOW -> fails on #1,#2,#3.
=> Only the INTERACTION "heavy p-block {Hg,Tl,Pb} present" separates correctly.

### Rule consistency check vs all 24 measured
- All Hg phases (O6 series, O4 series) HIGH -> rule: has Hg -> HIGH. ok
- O10Ca2Cu3Ba2Tl2 (Tl) -> HIGH. ok
- O7Cu3Ba2La1 (no Hg/Tl/Pb) -> LOW (89.6<90). ok
- O7Cu3Y0.200Ba2La0.800 (no Hg/Tl/Pb) -> LOW (81<90). ok
- (Unmeasured-by-me O6Cu1Ba2Hg1Tl1, O6Cu1Cd0.250Ba2Tl1.750: rule says HIGH —
  consistent, not needed for predictions.)

## Predictions for unmeasurable (7)
1. O6.160Ca1Cu2Ba2Hg1  (5e, has Hg) -> HIGH (series interp 105<.<122 ~113)
2. O6.270Ca1Cu2Ba2Hg1  (5e, has Hg) -> HIGH (series interp 123>->100 ~111)
3. O4.180Cu1Ba2Hg1     (4e, has Hg) -> HIGH (series interp 97.4>->97.0 ~97.2)
4. O8Ca0.850Cu2.200Ba2Tl1.800 (5e, has Tl; Tl~1.8 ~ Tl-2212-like) -> HIGH (~108)
5. O7Ca1Cu2Sr2Tl0.500Pb0.500  (6e, has Tl+Pb) -> HIGH
6. O8Cu3Y1Ba1.900La0.100 (5e, no Hg/Tl/Pb; YBCO-123 w/ La-on-Ba) -> LOW (~<90)
7. O8Cu4Y1Ba1.870La0.130 (5e, no Hg/Tl/Pb; YBCO-124 w/ La) -> LOW (~80)

Predicted HIGH: #1,#2,#3,#4,#5.  Predicted LOW: #6,#7.

## Step D — final pick (fewest elements among predicted HIGH; tie->higher Tc)
Element counts: #1=5, #2=5, #3=4, #4=5, #5=6.
**Fewest = #3 O4.180Cu1Ba2Hg1 (4 elements)** — unique minimum, no tie-break needed.
Expected Tc ~97.2 K (highest among 4-element highs trivially; only one).

## FINAL
- peak: O6.210Ca1Cu2Ba2Hg1
- peak_tc: 127.0
- threshold: 95.0
- qualifying: [O4.047, O4.081, O4.096, O4.097, O4.150, O4.162, O4.220]Cu1Ba2Hg1
- predictions: {1:high,2:high,3:high,4:high,5:high,6:low,7:low}
- final: O4.180Cu1Ba2Hg1
- rule: heavy p-block {Hg,Tl,Pb} in buffer cations -> HIGH; else LOW
