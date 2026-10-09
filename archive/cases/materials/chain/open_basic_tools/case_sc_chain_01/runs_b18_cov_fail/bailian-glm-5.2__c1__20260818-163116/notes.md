# Superconductor Tc study — notes

## Setup
- Budget: 18 units total, shared between measure_tc and search_db (1 unit each).
- Seed: `O6.080Ca1Cu2Ba2Hg1` (elements O, Ca, Cu, Ba, Hg — 5 elements).
- High = Tc >= 90 K; low otherwise.
- Steps: A (series peak T*), B (Hg & exactly 4 elements, Tc >= T*-32 = Q1), C (predict unmeasurable), D (fewest elements among predicted high).

## Budget log (starting at 18)
- search Hg p1: 1 (→3 results, 27 total) [budget 17→ no wait used=1]
- search Ca1Cu2Ba2Hg1 p1: 2 [used=2]
- search Ca1Cu2Ba2Hg1 p2: 3
- search Ca1Cu2Ba2Hg1 p3: 4
- search Ca1Cu2Ba2Hg1 p4: 5
- measure O6.140=105, O6.240=123: 6,7
- measure O6.270=NONE(unmeasurable), O6.300=100: 8
- measure O6.220=127, O6.080=92: 9,10
- measure O6.210=127: 11
- search Hg p2,p3: 12,13
- search Hg p4,p5: 14,15
- REMAINING: 3 units

## Step A result
T* = 127 K at O6.220Ca1Cu2Ba2Hg1 (O6.210 also 127, tie; pick 6.220).
Series dome: 0.080=92, 0.140=105, 0.210=127, 0.220=127, 0.240=123, 0.270=unmeasurable, 0.300=100.
threshold1 = 127 - 32 = 95 K.

## Hg search progress (27 total, PAGE_SIZE=3)
Discovered 4-element (O,Cu,Ba,Hg): O4.047, 0.066, 0.081, 0.085, 0.096, 0.097, 0.101, 0.103, 0.150, 0.160, 0.162, 0.180, 0.200, 0.220 (14 so far)
5-element: O6.080Ca1Cu2Ba2Hg1 (seed family)
Pages 6-9 NOT fetched (would cost 4, only 3 left).

## CRISIS: only 3 budget left. Cannot measure all 4-elem candidates. Must infer Tc from pattern + measure strategically.

## FINAL: budget EXHAUSTED (18/18) after Hg p6,7,8. No measure budget left.
Spent the last 3 on Hg pages 6,7,8 (more seed-family members — p6:6.100,6.130,6.140; p7:6.160,6.190,6.210; p8:6.220,6.240,6.270). Page 9 (3 results) NOT fetched.

## Full Hg inventory (27 materials; 24 fetched)
### 4-element O4.xxxCu1Ba2Hg1 (Hg-1201 series): 14 members
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
### 5-element O6.0yyCa1Cu2Ba2Hg1 (Hg-1212 seed series): 12 members (all fetched)
O6.080, 0.100, 0.130, 0.140, 0.160, 0.190, 0.210, 0.220, 0.240, 0.270, 0.300, 0.310
### Unfetched: 3 more (page 9). Total 14+12 = 26, +3 = 29? But DB says 27. Recount: 4-elem=14, 5-elem=12 = 26, but p9 has_more was true at p8. Hmm 27 total → 3 unfetched on p9 → but that's 29. Discrepancy: maybe 4-element count is actually 12 (not 14). Let me recount pages 1-5: p1=3, p2=3, p3=3, p4=3, p5=2(four-element 4.200,4.220)+1(five-element 6.080). So 4-element = 3+3+3+3+2 = 14. 5-element across p5(1)+p6(3)+p7(3)+p8(3)+p9(?) = 1+3+3+3 = 10 + p9. For total 27: 14 + 13 = 27 → 5-element = 13, so p9 = 3 (13th,14th? no). 5-element: p5:1, p6:3, p7:3, p8:3 = 10, p9:3 → 13. 14+13=27. ✓. So 3 unfetched 5-element members on p9 (likely O6.300, O6.310, +1 more, OR a 13th oxygen value).

Actually I already saw O6.300, O6.310 via the Ca1Cu2Ba2Hg1 search (12 members p1-4 there). So 5-elem has exactly 12. Then 14+12=26, need 1 more from p9. So p9 has just 1 result (has_more may have been true erroneously, or p9 = 1 result). Either way ~1 unfetched.

## Step B: threshold1 = 95. Need Hg & 4-element & Tc>=95.
The 4-element series is HgBa2CuO4+x (Hg-1201). Known Tc: rises with x, peaks ~94-97 K near x=0.2-0.4 (O4.20-4.40). With threshold 95, only near-peak qualify. But I CANNOT measure any (no budget). 
- O4.200, O4.220 (x≈0.2) are near peak → likely ≥95. 
- Lower oxygen (O4.047-0.180) → likely <95.
- I have NO measured 4-element Tc values at all.

## Step C: unmeasurable materials found
- O6.270Ca1Cu2Ba2Hg1 (from Step A ternary) — unmeasurable.
- Possibly others among unfetched p9.
Predict via dome: 5-elem dome peaks at 0.220(127), 0.240(123). At 0.270 (past peak, descending toward 0.300=100) → predicted ~110-115 → HIGH (≥90). 

## Step D: among predicted-high unmeasurable, fewest elements (tie→higher Tc). Only O6.270 (5 elem) is unmeasurable so far. If 4-element unmeasurables exist on p9, prefer those.

## HONEST STATUS: I cannot fully complete Steps B/C/D with measurements. Must give best-effort inference answer.

## ANSWER WRITTEN to answer.json
- peak: O6.220Ca1Cu2Ba2Hg1, peak_tc 127, threshold 95
- qualifying (Q1, best-guess from physics, NOT measured): O4.200Cu1Ba2Hg1, O4.220Cu1Ba2Hg1
- predictions: O6.270Ca1Cu2Ba2Hg1 = high (interpolated between 0.240=123 and 0.300=100)
- final: O6.270Ca1Cu2Ba2Hg1 (only unmeasurable found with predicted high)
- rule: unimodal doping-dome interpolation between nearest measured neighbors.

## CAVEATS (budget mismanagement)
- Overspent 5 search units on the Hg paginated search (pages 6-8) trying to enumerate ALL Hg materials, leaving zero budget to measure any 4-element Step-B candidates. Q1 is a physics guess, not measured.
- Better plan would have been: after finding the seed series (5 searches) + ternary peak (~4-5 measures) + a couple confirmations, reserve ~6-8 units to (a) fetch full Hg list efficiently, (b) measure a representative spread of the O4.xxx 4-element series to map its dome and find which >=95.
- The 4-element Hg-1201 series (O4.xxxCu1Ba2Hg1) Tc dome is known to peak ~94-97 K; threshold 95 is knife-edge, so Q1 membership is genuinely uncertain without measurement.
- Only one unmeasurable (O6.270) was actually encountered via measure_tc; predictions dict has just that one entry.
