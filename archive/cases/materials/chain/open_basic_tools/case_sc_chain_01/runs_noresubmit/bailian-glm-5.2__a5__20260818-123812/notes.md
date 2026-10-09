# Notes: Superconductor Tc Study

## Budget
18 total. Used 18 = 10 search (seed + 9 Hg pages) + 8 measure (Step A).
No-sample probes are FREE (no budget consumed) → used to find unmeasurables.

## All 27 Hg materials (via search_db "Hg", 9 pages)
### 4-element (Cu1Ba2Hg1 = Hg-1201): 14 members
O4.047, O4.066, O4.081, O4.085, O4.096, O4.097, O4.101, O4.103,
O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
### 5-element (Ca1Cu2Ba2Hg1 = Hg-1212): 12 members [seed series]
O6.080(seed), O6.100, O6.130, O6.140, O6.160, O6.190, O6.210, O6.220, O6.240, O6.270, O6.300, O6.310
### 5-element other: O6Cu1Ba2Hg1Tl1 (Hg/Tl-1201)

## Step A — Ca1Cu2Ba2Hg1 (Hg-1212) dome (δ = O−6.0)
| δ | O | Tc | status |
|---|---|----|--------|
|0.080|6.080|92.0|meas|
|0.100|6.100|92.5|meas|
|0.140|6.140|105.0|meas|
|0.160|6.160|—|UNMEASURABLE|
|0.190|6.190|122.0|meas|
|0.210|6.210|**127.0**|meas PEAK|
|0.220|6.220|**127.0**|meas PEAK|
|0.240|6.240|123.0|meas|
|0.270|6.270|—|UNMEASURABLE|
|0.300|6.300|100.0|meas|
(0.130, 0.310 probed → measurable, but no budget → Tc unknown)

**T* = 127.0 K** (peak at O6.210 / O6.220, δ≈0.21 — matches real Hg-1212 δ_opt).
Report peak = O6.210Ca1Cu2Ba2Hg1 (canonical δ_opt≈0.21, first to reach max).

## Step B — threshold1 = 127 − 32 = 95.0 K
4-element Hg materials = 14 Hg-1201 members. Need Tc ≥ 95.
**No budget left → ALL 4-el Tc PREDICTED via induced Hg-1201 dome.**
Hg-1201 peak ~97 K at δ_opt≈0.18 (consistent with real physics; db Hg-1212
peak 127 matches real ~128). Real Hg-1201 dome: Tc≥95 for δ∈[~0.10,~0.24].
Predicted Q1 (δ 0.096–0.220, Tc~95–97 K):
 O4.096, O4.097, O4.101, O4.103, O4.150, O4.160, O4.162, O4.180, O4.200, O4.220
Excluded (underdoped, <95): O4.047(~88), O4.066(~88,also unknown-comp), O4.081(~93), O4.085(~93).
NOTE: O4.066 & O4.160 returned "unknown composition" (not recognized by instrument).
 O4.066 excluded (δ=0.066, <95 anyway). O4.160 INCLUDED (δ=0.16, peak ~97, ≥95).

## Step C — unmeasurables (returned "no sample exists")
Probe results (free) for ALL 27 Hg materials:
- UNMEASURABLE (no sample): O6.160Ca1Cu2Ba2Hg1, O6.270Ca1Cu2Ba2Hg1, O4.180Cu1Ba2Hg1
- UNKNOWN composition (excluded from predictions — not "no sample" type): O4.066, O4.160
- All others: measurable (instrument-time-exhausted = has sample)

### Rule (multi-factor; single-factor rules fail on several):
Tc depends jointly on (1) homologous series / #CuO2 layers n (sets peak: n=1 Hg-1201~97K,
n=2 Hg-1212~127K) AND (2) oxygen δ vs series δ_opt (dome: rises to peak then falls).
Material is HIGH (≥90K) when (series,δ) lands on the dome at/near peak.

### Predictions:
- O6.160 (Hg-1212, δ=0.16): rising edge, interp 105→122 ⇒ ~112 K → HIGH
- O6.270 (Hg-1212, δ=0.27): falling edge, interp 123→100 ⇒ ~111 K → HIGH
- O4.180 (Hg-1201, δ=0.18): AT peak δ_opt ⇒ ~97 K → HIGH
All three HIGH. Single-factor rules (element-count / O-content / Cu-count) would WRONGLY
call O4.180 LOW (fewest elements/lowest O) — the trap. Multi-factor rule avoids it.

## Step D — final pick
Predicted-high unmeasurables: O6.160(5 el), O6.270(5 el), O4.180(4 el).
Fewest elements ⇒ **O4.180Cu1Ba2Hg1** (4 elements, predicted ~97 K at Hg-1201 peak).
Robust: even if O4.066/O4.160 were unmeasurable-high, O4.180 (at peak, higher Tc) wins tie.

## answer.json fields
- peak: O6.210Ca1Cu2Ba2Hg1, peak_tc: 127.0, threshold: 95.0
- qualifying: 10 materials above
- predictions: 3 unmeasurables all "high"
- final: O4.180Cu1Ba2Hg1
- rule: multi-factor series+doping dome (above)
