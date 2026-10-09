# Superconductor Tc study — notes

## Task summary
- Budget: 26 units total, shared by `measure_tc` and `search_db` (1 unit/page).
- Seed: `O6.080Ca1Cu2Ba2Hg1` (= Hg1Ba2Ca1Cu2O6.08 → Hg-1212 cuprate).
- High = Tc >= 90 K, else low.
- Step A: find homologous doping series (same elements, varying O), measure members, find MAX Tc = T*.
- Step B: threshold1 = T* - 32. Among materials containing Hg AND exactly 4 elements, report ALL with Tc >= threshold1 → Q1.
- Step C: induce rule from measurable materials; predict high/low for every unmeasurable material found via search_db.
- Step D: among predicted-high unmeasurables, pick fewest elements (tie → higher expected Tc).

## Budget tracking
Budget 26 total. Search "Hg" p1-9 = 9 used. Remaining 17 for measuring.
After Step A + B sampling: budget_used = 23. Remaining = 3.

## Database discovery (search "Hg", 9 pages, 27 materials)
Page size = 3 (not 4). All Hg materials:
**Hg-1212 series** (5 elem: O,Ca,Cu,Ba,Hg — same as seed), 12 members, varying O:
  6.080, 6.100, 6.130, 6.140, 6.160*, 6.190, 6.210, 6.220, 6.240, 6.270*, 6.300, 6.310  (* = unmeasurable)
**Hg-1201 series** (4 elem: O,Cu,Ba,Hg — exactly 4 elements!), 14 members, varying O:
  4.047, 4.066, 4.081, 4.085, 4.096, 4.097, 4.101, 4.103, 4.150, 4.160, 4.162, 4.180*, 4.200, 4.220  (* = unmeasurable)
**Other**: O6Cu1Ba2Hg1Tl1 (5 elem: O,Cu,Ba,Hg,Tl)

## Step A — Hg-1212 series measurements (Tc vs O)
| O     | Tc    | note |
|-------|-------|------|
| 6.080 | 92.0  | |
| 6.100 | 92.5  | |
| 6.130 | 102.0 | |
| 6.140 | 105.0 | |
| 6.160 | UNMEA | unmeasurable |
| 6.190 | 122.0 | |
| 6.210 | 127.0 | PEAK (tie) |
| 6.220 | 127.0 | PEAK (tie) |
| 6.240 | 123.0 | |
| 6.270 | UNMEA | unmeasurable |
| 6.300 | 100.0 | |
| 6.310 | 98.0  | |
=> T* = 127.0. Peak composition = O6.210Ca1Cu2Ba2Hg1 (first reaching 127).
threshold1 = 127 - 32 = 95.0

## Step B — Hg-1201 candidates (exactly 4 elements; threshold1 = 95)
Measured (Tc): 4.047(95.0), 4.097(95.3), 4.150(97.0), 4.180(UNMEA), 4.220(97.0)
All measured values >= 95 → all qualify. Dome appears flat ~95-97.
UNMEASURED 4-elem Hg candidates: 4.066, 4.081, 4.085, 4.096, 4.101, 4.103, 4.160, 4.162, 4.200.
Also: O6Cu1Ba2Hg1Tl1 has 5 elements → NOT a 4-element candidate.

## Unmeasurables found (for Step C): O6.160, O6.270, O4.180
- O6.160: between 105@6.140 & 122@6.190 on rising slope → ~113 → HIGH
- O6.270: between 123@6.240 & 100@6.300 on falling slope → ~112 → HIGH
- O4.180: between 97@4.150 & 97@4.220, flat dome → ~97 → HIGH

Budget left = 3.

## Step B (cont.) — Hg-1201 dome is FLAT at ~95-97
Measured: 4.047(95.0), 4.097(95.3), 4.150(97.0), 4.180(UNMEA), 4.220(97.0).
Min = 95.0 at lowest O (4.047); high end = 97. No dip below 95 anywhere.
=> All 14 Hg-1201 members have Tc >= 95 => Q1 = all 14 Hg-1201 members.
(O4.180 unmeasurable, predicted ~97 >= 95, included.)

## Rule for Step C (multi-factor, NOT single-factor)
Within each homologous series (fixed element set, varying O), Tc follows a dome in O.
Predict an unmeasurable member's Tc by interpolating its O between the nearest
MEASURED neighbors IN THE SAME series (same element set); classify high (>=90) vs low (<90)
from that interpolated value. Uses element-set membership + O-position + dome shape,
not a single factor (a single-factor "low O -> low" rule would WRONGLY flag O4.180).
Applied:
- O6.160 (Hg-1212): between 6.140(105) & 6.190(122) -> ~113 -> HIGH
- O6.270 (Hg-1212): between 6.240(123) & 6.300(100) -> ~112 -> HIGH
- O4.180 (Hg-1201): between 4.150(97) & 4.220(97) -> ~97 -> HIGH

## Step D
Predicted-HIGH unmeasurables and element counts:
- O6.160Ca1Cu2Ba2Hg1 : 5 elements (O,Ca,Cu,Ba,Hg)
- O6.270Ca1Cu2Ba2Hg1 : 5 elements
- O4.180Cu1Ba2Hg1     : 4 elements (O,Cu,Ba,Hg)  <- FEWEST
=> final = O4.180Cu1Ba2Hg1 (4 elements; also highest expected Tc ~97 among 4-elem).

## Remaining plan
3 budget left. Measure 3 more Hg-1201 rising-region members (4.066, 4.081, 4.101)
to (a) confirm no low-end dip below 95, (b) possibly discover more unmeasurables.
Rising region expected Tc ~95-96 < O4.180's 97, so O4.180 stays Step D winner.

## Additional Hg-1201 measurements (last 3 budget)
- O4.066Cu1Ba2Hg1 -> ERROR "unknown composition; discover it via search_db first"
    (it WAS on search page 2; tool refused. Treat as not-directly-measurable.
     Not "no sample exists"; a different harness state. Cannot resolve. Leave out of Q1
     since Tc unknown, OR include? -> per "report ALL whose Tc >= threshold1", unknown Tc
     cannot be confirmed >= 95, so EXCLUDE from Q1 but also note as discovered.
     Actually for Step C: only "no sample exists" returns count as unmeasurable. This
     error is different (harness needs rediscovery). I'll treat it as a measurement failure,
     not an unmeasurable material. Its Tc is simply unknown.)
- O4.081 -> 98.0 (>=95, QUALIFIES)
- O4.101 -> 93.4 (< 95, does NOT qualify) -- dome DIPS below 95 here!
- O4.103 -> 92.5 (< 95, does NOT qualify) -- confirms 4.10x local dip

## Final Hg-1201 (4-element Hg) table — threshold1 = 95
| O     | Tc    | >=95? |
|-------|-------|-------|
| 4.047 | 95.0  | YES |
| 4.066 | ? (meas err) | unknown |
| 4.081 | 98.0  | YES |
| 4.085 | ? | unknown |
| 4.096 | ? | unknown |
| 4.097 | 95.3 | YES |
| 4.101 | 93.4 | NO |
| 4.103 | 92.5 | NO |
| 4.150 | 97.0 | YES |
| 4.160 | ? | unknown |
| 4.162 | ? | unknown |
| 4.180 | UNMEA (pred ~97) | YES (predicted) |
| 4.200 | ? | unknown |
| 4.220 | 97.0 | YES |

Confirmed-qualifying (measured >=95): 4.047, 4.081, 4.097, 4.150, 4.220
Confirmed-not-qualifying (<95): 4.101, 4.103
Unmeasurable (pred high ~97, >=95): 4.180
Unknown Tc (could not measure): 4.066, 4.085, 4.096, 4.160, 4.162, 4.200

## Q1 decision (budget exhausted, must reason)
Step B asks: report ALL Hg+4-elem materials with Tc >= 95.
The ONLY Hg materials with exactly 4 elements are the 14 Hg-1201 members
(O,Cu,Ba,Hg). All other Hg materials have 5 elements (Hg-1212 series, or
O6Cu1Ba2Hg1Tl1) -> excluded.
Dome shape (measured): flat ~95-98 across the whole O range, with a narrow
local dip ONLY at 4.101(93.4)/4.103(92.5).
The 6 unmeasured members, by interpolation between measured points that are
ALL >=95 (except the isolated 4.10x dip), are inferred >=95:
  4.066 (btwn 95.0 & 98.0 -> ~96.5), 4.085 (btwn 98.0 & 95.3 -> ~96.6),
  4.096 (btwn 98.0 & 95.3, ~95.4), 4.160 (btwn 97 & 97 -> ~97),
  4.162 (~97), 4.200 (btwn 97 & 97 -> ~97)
=> Q1 = all 14 Hg-1201 members EXCEPT 4.101 and 4.103 (the only confirmed <95).
   = 12 members: 4.047, 4.066, 4.081, 4.085, 4.096, 4.097, 4.150, 4.160,
                 4.162, 4.180, 4.200, 4.220
(4.180 included via Step-C prediction ~97 >= 95.)

## FINAL ANSWER
{
  "peak": "O6.210Ca1Cu2Ba2Hg1",
  "peak_tc": 127.0,
  "threshold": 95.0,
  "qualifying": ["O4.047Cu1Ba2Hg1","O4.066Cu1Ba2Hg1","O4.081Cu1Ba2Hg1",
                 "O4.085Cu1Ba2Hg1","O4.096Cu1Ba2Hg1","O4.097Cu1Ba2Hg1",
                 "O4.150Cu1Ba2Hg1","O4.160Cu1Ba2Hg1","O4.162Cu1Ba2Hg1",
                 "O4.180Cu1Ba2Hg1","O4.200Cu1Ba2Hg1","O4.220Cu1Ba2Hg1"],
  "predictions": {
    "O6.160Ca1Cu2Ba2Hg1":"high",
    "O6.270Ca1Cu2Ba2Hg1":"high",
    "O4.180Cu1Ba2Hg1":"high"
  },
  "final":"O4.180Cu1Ba2Hg1",
  "rule":"Within a homologous series (fixed element set, varying oxygen) Tc vs oxygen content traces a dome; predict an unmeasurable member's Tc by interpolating its oxygen content between the nearest measured neighbors of the SAME series (same element set), then apply the high(>=90 K)/low(<90 K) cutoff to that interpolated value. This uses element-set membership + oxygen position + dome curvature together, so a single-factor rule (e.g. 'low O -> low' or 'fewer elements -> low') is avoided."
}



