# Superconductor Tc study — notes

## Seed & series (Step A)
Seed: `O6.080Ca1Cu2Ba2Hg1` (5 elements: O,Ca,Cu,Ba,Hg).
Series via search "Ca1Cu2Ba2Hg1" (12 members, 2/page, 6 pages).

Series Tc measurements (oxygen dome):
| O | Tc | avail |
|--------|------|------|
| 6.080 | 92.0 | yes |
| 6.100 | 92.5 | yes |
| 6.130 | 102.0 | yes |
| 6.140 | 105.0 | yes |
| 6.160 | null | NO (unmeasurable) |
| 6.190 | 122.0 | yes |
| 6.210 | **127.0** | yes (PEAK) |
| 6.220 | **127.0** | yes (PEAK) |
| 6.240 | 123.0 | yes |
| 6.270 | null | NO (unmeasurable) |
| 6.300 | 100.0 | yes |
| 6.310 | 98.0 | yes |

**T* = 127.0 K** (peak at O6.210 and O6.220, tied). Peak composition = O6.210Ca1Cu2Ba2Hg1.

## threshold1 (Step B)
threshold1 = T* - 32 = 127.0 - 32 = **95.0 K**

## Family B — Hg + exactly 4 elements
"Hg" search → 27 total = 12 (O6 series, 5-elem) + 14 (O4.0XXCu1Ba2Hg1, 4-elem) + 1 (O6Cu1Ba2Hg1Tl1, 5-elem).
Family B (4-elem Hg) = the 14 O4.0XXCu1Ba2Hg1 materials.

Tc measurements (4-elem family):
| composition | Tc | avail |
|---|---|---|
| O4.047Cu1Ba2Hg1 | null | NO (unmeasurable) |
| O4.066Cu1Ba2Hg1 | null | NO (unmeasurable) |
| O4.081Cu1Ba2Hg1 | 98.0 | yes |
| O4.085Cu1Ba2Hg1 | 92.0 | yes |
| O4.096Cu1Ba2Hg1 | 95.6 | yes |
| O4.097Cu1Ba2Hg1 | 95.3 | yes |
| O4.101Cu1Ba2Hg1 | 93.4 | yes |
| O4.103Cu1Ba2Hg1 | 92.5 | yes |
| O4.150Cu1Ba2Hg1 | 97.0 | yes |
| O4.160Cu1Ba2Hg1 | null | NO (unmeasurable) |
| O4.162Cu1Ba2Hg1 | 97.4 | yes |
| O4.180Cu1Ba2Hg1 | null | NO (unmeasurable) |
| O4.200Cu1Ba2Hg1 | 94.2 | yes |
| O4.220Cu1Ba2Hg1 | 97.0 | yes |

### Q1 (Step B): family B materials with Tc >= 95.0
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)
(Excludes 92.0, 93.4, 92.5, 94.2, and the 4 unmeasurable.)

## Extra material measured
O6Cu1Ba2Hg1Tl1 (5-elem, Tl): Tc = 0.0 (LOW, insulating — has Tl not Ca).

## Budget
- search_db pages: 6 (series) + 7 (Hg family pages 1-8) + 1 (Hg1 p1) + 1 (Hg1 p2) = 15... recount:
  - 1 (seed direct) + 1 (Ca1Cu2Ba2Hg1 p1) ... actually count from tool outputs.
- Final budget_used = 41 of 45. Remaining 4.
- Unmeasurable measurements cost 0 budget (confirmed).

## Step C — unmeasurable materials discovered (6 total, all via Hg-family searches)
| composition | #elem | O | context |
|---|---|---|---|
| O6.160Ca1Cu2Ba2Hg1 | 5 | 6.160 | series member (unmeasured) |
| O6.270Ca1Cu2Ba2Hg1 | 5 | 6.270 | series member (unmeasured) |
| O4.047Cu1Ba2Hg1 | 4 | 4.047 | 4-elem family, lowest O |
| O4.066Cu1Ba2Hg1 | 4 | 4.066 | 4-elem family, low O |
| O4.160Cu1Ba2Hg1 | 4 | 4.160 | 4-elem family, high O |
| O4.180Cu1Ba2Hg1 | 4 | 4.180 | 4-elem family, high O |

## Rule (induced)
Within a fixed-element Hg cuprate family, Tc follows an oxygen-doping DOME:
Tc rises with oxygen from the under-doped low end, peaks in the optimal window,
then falls to the over-doped low end. Boundary Tc=90 K = "high".
- Series (Ca1Cu2Ba2Hg1, O6.080–6.310): measurable points trace a dome peaking at O≈6.21–6.22.
  - 6.080→92.0 (just above high/low boundary), 6.100→92.5, 6.130→102, 6.140→105, [6.160 unmeasured, interior of dome], 6.190→122, 6.210→127, 6.220→127, 6.240→123, 6.300→100, 6.310→98.
  - O6.160 sits between 105 (6.140) and 122 (6.190) — interior of dome, well above 90 → **HIGH** (expected ~110-115).
  - O6.270 sits between 123 (6.240) and 100 (6.300) — past peak, descending; interpolation ~112 → **HIGH**.
- 4-elem family (O4.0XXCu1Ba2Hg1, no Ca): dome is flatter/noisier, peak ~97-98.
  - Measured: 4.081→98, 4.085→92, 4.096→95.6, 4.097→95.3, 4.101→93.4, 4.103→92.5, 4.150→97, 4.162→97.4, 4.200→94.2, 4.220→97.
  - O4.047 (O below all measured low-O neighbor 4.081=98): extrapolating the low-O rise backward, still likely ≥~90 → **HIGH** (interior/dome rising side; nearest neighbor high).
  - O4.066 (between 4.047 and 4.081=98): dome rising side → **HIGH**.
  - O4.160 (between 4.150=97 and 4.162=97.4): interior peak → **HIGH**.
  - O4.180 (between 4.162=97.4 and 4.200=94.2): descending side but still well above 90 → **HIGH**.
- Counter-check: O6Cu1Ba2Hg1Tl1 = 0.0 (LOW) — different element set (Tl not Ca); single-factor "has Hg→high" would be WRONG. Confirms need for element-set + oxygen-window rule.

## Step D — final pick
Unmeasurable predicted HIGH:
- 4-elem: O4.047, O4.066, O4.160, O4.180
- 5-elem: O6.160, O6.270
Fewest elements = 4 → among {O4.047, O4.066, O4.160, O4.180}.
Tie-break higher expected Tc:
- O4.047 ~ extrapolate rising side, maybe ~95-98 (but lowest O, most uncertain).
- O4.066 ~ between 4.047 and 4.081(98) → ~98 (rising toward peak).
- O4.160 ~ peak region (neighbors 97, 97.4) → ~97.
- O4.180 ~ descending (97.4→94.2) → ~96.
Highest expected Tc → **O4.066Cu1Ba2Hg1** (~98).

## Caveats / decisions
- Budget ran low (4 left); could not probe more queries. The 27 Hg materials are fully accounted for (12 series + 14 four-elem + 1 Tl). "Hg1" re-search confirmed no additional materials.
- All 6 unmeasurable materials are Hg-family (no non-Hg unmeasurables discovered). Element-count mix (4× 4-elem, 2× 5-elem) matches the "fewest elements" tiebreak design.

### Per-material expected Tc (interpolation/extrapolation within family)
Series dome (clear: 92→127→98):
- O6.160 (5-elem, series): between 6.140(105) & 6.190(122) → ~112 → **HIGH**
- O6.270 (5-elem, series): between 6.240(123) & 6.300(100) → ~112 → **HIGH**
1201 family — KEY: data is a flat HIGH plateau 92-98 across O=[4.081,4.220]; the LOWEST-O measured point (4.081) is actually the HIGHEST Tc (98.0). So no under-doped drop-off is evidenced; extrapolate plateau = high:
- O4.160 (4-elem): between 4.150(97.0) & 4.162(97.4) → ~97.3 → **HIGH**
- O4.180 (4-elem): between 4.162(97.4) & 4.200(94.2) → ~95.9 → **HIGH**
- O4.066 (4-elem): just below 4.081(98), flat plateau → ~95-97 → **HIGH**
- O4.047 (4-elem): below 4.081(98), plateau → ~94-96 → **HIGH**

### Why ALL 6 unmeasurables predicted HIGH (data-driven)
- 2 series unmeasurables: solidly interior to the series dome (interpolate ~112 K).
- 4 1201 unmeasurables: 4.160/4.180 are interior interpolations (~97/~96); 4.047/4.066 extrapolate the flat 1201 high-plateau (lowest-O measured = 4.081 is the peak 98, so no under-doped decline visible).
- Counterexample to "has Hg → high" is the Tl material O6Cu1Ba2Hg1Tl1 (Tc=0, measurable) — different element set (Tl not Ca) → insulating. This shows ELEMENT SET matters; all 6 unmeasurables have good element sets (Ca-series or 1201), so high.

### Warning interpretation (multi-factor, not single-factor)
Single-factor rules that fail on several unmeasurables:
- "has Hg → high": correct on the 6 (all high), but IGNORES the Tl insulator → wrong as a general rule.
- "oxygen in [6.0,6.3] → high" (the series window): would WRONGLY predict all 4 of the 1201 unmeasurables (O≈4.05-4.22) as LOW — wrong on 4 (="several"). This is the trap the warning flags.
→ Correct rule = multi-factor: superconducting element set (Ca-Cu-Ba-Hg series OR Cu-Ba-Hg 1201) AND oxygen within THAT family's specific doping window. The 1201 window is at O≈4, not O≈6.

### Step D
Predicted-HIGH unmeasurables, FEWEST elements (4-elem): {O4.047(~95), O4.066(~96), O4.160(~97.3), O4.180(~95.9)}.
Tie-break higher expected Tc → **O4.160Cu1Ba2Hg1** (~97.3, confident interpolation between two close high points 97.0 & 97.4).
- Robust: O4.160's 97.3 is the highest AND most confident (interpolation) among the 4-elem candidates; final unaffected by the 4.047/4.066 extrapolation uncertainty.

## FINAL
- peak = O6.210Ca1Cu2Ba2Hg1 (T*=127.0; tied apex with O6.220=127.0, pick lower-O of plateau)
- threshold1 = 95.0
- Q1 (4-elem Hg, measured Tc ≥ 95.0): O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0), O4.162(97.4), O4.220(97.0)
  (unmeasurable 4-elem O4.047/066/160/180 excluded — null Tc can't satisfy ≥95)
- predictions: O6.160→high, O6.270→high, O4.047→high, O4.066→high, O4.160→high, O4.180→high
- final = O4.160Cu1Ba2Hg1
- budget used: 41/45 (unmeasurable probes were free)
