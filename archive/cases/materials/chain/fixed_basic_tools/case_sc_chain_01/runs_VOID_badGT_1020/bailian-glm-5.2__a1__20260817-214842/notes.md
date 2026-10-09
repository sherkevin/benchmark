# Superconductor Tc study — notes

Seed: `O6.080Ca1Cu2Ba2Hg1`. High = Tc >= 90 K. Budget: 34 (used 29).

## Step A — all measurable.txt measurements
| composition | Tc | elem set | family |
|---|---|---|---|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | 5 {O,Ca,Cu,Ba,Hg} | Hg-1212 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | 5 | Hg-1212 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | 5 | Hg-1212 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | 5 | Hg-1212 |
| O6.160Ca1Cu2Ba2Hg1 | NO SAMPLE | 5 | (also in unmeasurable) |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | 5 | Hg-1212 |
| O6.210Ca1Cu2Ba2Hg1 | **127.0** | 5 | Hg-1212 | <- MAX (tie w/ O6.220) |
| O6.220Ca1Cu2Ba2Hg1 | **127.0** | 5 | Hg-1212 | <- MAX (tie) |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | 5 | Hg-1212 |
| O6.270Ca1Cu2Ba2Hg1 | NO SAMPLE | 5 | (also in unmeasurable) |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | 5 | Hg-1212 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | 5 | Hg-1212 |
| O4.081Cu1Ba2Hg1 | 98.0 | 4 {O,Cu,Ba,Hg} | Hg-1201 |
| O4.162Cu1Ba2Hg1 | 97.4 | 4 | Hg-1201 |
| O4.150Cu1Ba2Hg1 | 97.0 | 4 | Hg-1201 |
| O4.220Cu1Ba2Hg1 | 97.0 | 4 | Hg-1201 |
| O4.096Cu1Ba2Hg1 | 95.6 | 4 | Hg-1201 |
| O4.097Cu1Ba2Hg1 | 95.3 | 4 | Hg-1201 |
| O4.180Cu1Ba2Hg1 | NO SAMPLE | 4 | (also in unmeasurable) |
| O4.047Cu1Ba2Hg1 | NO SAMPLE | 4 | (also in unmeasurable) |
| O4.200Cu1Ba2Hg1 | 94.2 | 4 | Hg-1201 |
| O4.101Cu1Ba2Hg1 | 93.4 | 4 | Hg-1201 |
| O4.103Cu1Ba2Hg1 | 92.5 | 4 | Hg-1201 |
| O4.085Cu1Ba2Hg1 | 92.0 | 4 | Hg-1201 |
| O6Cu1Ba2Hg1Tl1 | 0.0 | 5 {O,Cu,Ba,Hg,Tl} | Hg+Tl mixed -> LOW |
| O10Ca2Cu3Ba2Tl2 | 118.97 | 5 {O,Ca,Cu,Ba,Tl} | Tl-2223 |
| O6Cu1Cd0.250Ba2Tl1.750 | 88.0 | 5 {O,Cu,Cd,Ba,Tl} | Tl+Cd, no Ca -> LOW |
| O7Cu3Ba2La1 | 89.6 | 4 {O,Cu,Ba,La} | no reservoir -> LOW |
| O7Cu3Y0.200Ba2La0.800 | 81.0 | 5 {O,Cu,Y,Ba,La} | no reservoir -> LOW |

**T* (peak) = 127.0 K**, achieved by O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1 (tie).
Pick O6.210 (first in list order). Note tie in answer.

## Step B — threshold1 = T* - 32 = 127.0 - 32 = 95.0 K
Filter: contains Hg AND exactly 4 elements. Only Hg-1201 (O4.xxx Cu1Ba2Hg1, 4 elem).
(O6Cu1Ba2Hg1Tl1 has 5 elem -> excluded; O6.xxx series has 5 elem -> excluded.)
Among Hg-1201 with measured Tc >= 95.0:
- O4.081Cu1Ba2Hg1 (98.0)  ✓
- O4.162Cu1Ba2Hg1 (97.4) ✓
- O4.150Cu1Ba2Hg1 (97.0) ✓
- O4.220Cu1Ba2Hg1 (97.0) ✓
- O4.096Cu1Ba2Hg1 (95.6) ✓
- O4.097Cu1Ba2Hg1 (95.3) ✓
Excluded (<95): O4.200(94.2), O4.101(93.4), O4.103(92.5), O4.085(92.0).
(O4.180, O4.047 have no measured Tc -> handled in Step C, not Q1.)
**Q1 = 6 members.**

## Step C — induced rule
HIGH families in training: {O,Ca,Cu,Ba,Hg} (Hg-1212), {O,Cu,Ba,Hg} (Hg-1201), {O,Ca,Cu,Ba,Tl} (Tl-2223).
LOW: {O,Cu,Ba,Hg,Tl} (Hg+Tl mixed, 0K), {O,Cu,Cd,Ba,Tl} (Tl+Cd no Ca, 88), {O,Cu,Ba,La} (no reservoir, 89.6), {O,Cu,Y,Ba,La} (no reservoir, 81).

RULE (multi-factor): High Tc requires Ba + Cu + a single uncontaminated Hg/Tl reservoir:
 (i) Hg present AND Tl absent -> high (Hg-1201 / Hg-1212, Ca optional);
 (ii) Tl present AND Hg absent AND Ca present AND Ba present -> high (Tl-2223).
 Low otherwise: Hg+Tl together, Tl without Ca, Tl/Cd or Tl/Pb-Sr analogs lacking Ba, La/Y-only (no Hg/Tl).

Sanity vs warning: "contains Hg" fails on O6Cu1Ba2Hg1Tl1 (low, both) & on Tl-2223 (high, no Hg) -> trap test #7.
 "5 elements -> high" fails on test 3-6 (4-elem Hg-1201, high) and 9,10 (5-elem, low) -> "several". ✓ matches warning.

Predictions for unmeasurable (10):
1. O6.160Ca1Cu2Ba2Hg1  -> high (Hg, no Tl; Hg-1212, between 105 & 122)
2. O6.270Ca1Cu2Ba2Hg1  -> high (Hg-1212, between 123 & 100)
3. O4.180Cu1Ba2Hg1     -> high (Hg-1201, between 97.4 & 94.2, ~96)
4. O4.047Cu1Ba2Hg1     -> high (Hg-1201; all measured O4 are 92-98, induced family=high)
5. O4.066Cu1Ba2Hg1     -> high (Hg-1201, between est & O4.081=98, ~96)
6. O4.160Cu1Ba2Hg1     -> high (Hg-1201, between 97.0 & 97.4, ~97.3)
7. O8Ca0.850Cu2.200Ba2Tl1.800 -> high (Tl~2 + Ca + Ba, Tl-2212/2223 family)
8. O7Ca1Cu2Sr2Tl0.500Pb0.500  -> low (Tl+Ca but Sr not Ba + Pb; not a high family; ~80K real)
9. O8Cu3Y1Ba1.900La0.100     -> low (no Hg/Tl reservoir; same set as #25 which was 81)
10. O8Cu4Y1Ba1.870La0.130    -> low (no Hg/Tl reservoir; same family)

## Step D — final pick
Predicted-high: 1,2,3,4,5,6,7. Element counts: 1,2,7 = 5 elem; 3,4,5,6 = 4 elem.
Fewest elements = 4 -> candidates {3,4,5,6} (all Hg-1201 O4.xxx).
Tie-break higher expected Tc (interpolate from neighbors):
- O4.160: between O4.150=97.0 and O4.162=97.4 -> ~97.3  <-- highest
- O4.180: between O4.162=97.4 and O4.200=94.2 -> ~95.9
- O4.066: between (est ~93) and O4.081=98 -> ~95.8
- O4.047: ~92-93 (lowest, underdoped edge)
**final = O4.160Cu1Ba2Hg1**
