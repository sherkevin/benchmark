# Notes — Tc prediction for Gd-family Pr-doped cuprates

## Measurements

### Er family (RE = Er, Z=68, ionic radius ~0.890 Å, 8-coord)

| Pr content x | Composition | Tc (K) |
|---|---|---|
| 0.030 | O7Cu3Ba2Pr0.030Er0.970 | 93.9 |
| 0.076 | O7Cu3Ba2Pr0.076Er0.924 | 91.1 |
| 0.150 | O7Cu3Ba2Pr0.150Er0.850 | 82.9 |
| 0.200 | O7Cu3Ba2Pr0.200Er0.800 | 76.4 |
| 0.300 | O7Cu3Ba2Pr0.300Er0.700 | 61.1 |
| 0.400 | O7Cu3Ba2Pr0.400Er0.600 | 45.8 |
| 0.500 | O7Cu3Ba2Pr0.500Er0.500 | 28.6 |
| 0.800 | O7Cu3Ba2Pr0.800Er0.200 | 0.0 |
| 0.900 | O7Cu3Ba2Pr0.900Er0.100 | 0.0 |

### Nd family (RE = Nd, Z=60, ionic radius ~1.109 Å, 8-coord)

| Pr content x | Composition | Tc (K) |
|---|---|---|
| 0.030 | O7Cu3Ba2Pr0.030Nd0.970 | 85.1 |
| 0.080 | O7Cu3Ba2Pr0.080Nd0.920 | 68.2 |
| 0.150 | O7Cu3Ba2Pr0.150Nd0.850 | 40.7 |
| 0.200 | O7Cu3Ba2Pr0.200Nd0.800 | 31.4 |
| 0.300 | O7Cu3Ba2Pr0.300Nd0.700 | 5.6 |
| 0.500 | O7Cu3Ba2Pr0.500Nd0.500 | 0.0 |
| 0.600 | O7Cu3Ba2Pr0.600Nd0.400 | 0.0 |
| 0.700 | O7Cu3Ba2Pr0.700Nd0.300 | 0.0 |
| 0.800 | O7Cu3Ba2Pr0.800Nd0.200 | 0.0 |
| 0.900 | O7Cu3Ba2Pr0.900Nd0.100 | 0.0 |

## Pattern analysis

1. **Er family** has a higher base Tc (~94 K at x≈0) and Pr suppresses Tc gradually. Superconductivity survives to at least x=0.50 (Tc=28.6 K), dying out before x=0.80.
2. **Nd family** has a lower base Tc (~85 K at x≈0) and Pr suppresses Tc much more aggressively. Tc drops to 5.6 K by x=0.30 and is zero from x=0.50 onward.
3. The difference is the rare-earth ionic radius: Er³⁺ (0.890 Å) < Gd³⁺ (0.938 Å) < Nd³⁺ (1.109 Å). The smaller the host RE ion, the higher the base Tc and the more gradually Pr suppresses it.

## Interpolation method for Gd (Z=64, ionic radius ~0.938 Å, 8-coord)

Gd sits between Er and Nd in the lanthanide series. I use linear interpolation weighted by ionic radius:

- weight_Nd = (r_Gd − r_Er) / (r_Nd − r_Er) = (0.938 − 0.890) / (1.109 − 0.890) = 0.048 / 0.219 ≈ 0.219
- weight_Er = 1 − 0.219 = 0.781

So Tc(Gd, x) ≈ 0.781 × Tc(Er, x) + 0.219 × Tc(Nd, x)

Gd is much closer to Er than to Nd in ionic radius, so the Gd curve should closely track the Er family.

## Interpolated predictions

| x | Er Tc | Nd Tc | Gd Tc (interpolated) |
|---|---|---|---|
| 0.030 | 93.9 | 85.1 | 0.781×93.9 + 0.219×85.1 = 73.3 + 18.6 = **92.0** |
| 0.078 | 91.1* | 68.2† | 0.781×91.1 + 0.219×68.2 = 71.1 + 14.9 = **86.1** |
| 0.150 | 82.9 | 40.7 | 0.781×82.9 + 0.219×40.7 = 64.7 + 8.9 = **73.7** |
| 0.200 | 76.4 | 31.4 | 0.781×76.4 + 0.219×31.4 = 59.7 + 6.9 = **66.5** |
| 0.300 | 61.1 | 5.6 | 0.781×61.1 + 0.219×5.6 = 47.7 + 1.2 = **48.9** |
| 0.400 | 45.8 | 0‡ | 0.781×45.8 + 0.219×0 = **35.8** |
| 0.500 | 28.6 | 0.0 | 0.781×28.6 = **22.3** |
| 0.600 | — | 0.0 | Er ~12§ → 0.781×12 = **9.4** |
| 0.700 | — | 0.0 | Er ~0§ → **0.0** |
| 0.800 | 0.0 | 0.0 | **0.0** |
| 0.900 | 0.0 | 0.0 | **0.0** |

*Er measured at x=0.076 (close to 0.078); used 91.1 directly.
†Nd measured at x=0.080 (close to 0.078); used 68.2 directly.
‡Nd not measured at x=0.400, but Tc=5.6 at x=0.300 and 0.0 at x=0.500 → effectively 0 at x=0.400.
§Er Tc at x=0.600 estimated ~12 K by linear extrapolation from (0.500, 28.6) and (0.800, 0); Er Tc at x=0.700 ≈ 0.

### Er extrapolation for x=0.600
From Er data: (x=0.50, Tc=28.6) and (x=0.80, Tc=0.0). Linear slope = (0−28.6)/(0.80−0.50) = −95.3 K/x. At x=0.60: 28.6 + (−95.3)(0.10) ≈ 19.1 K. Using 19.1 → Gd: 0.781×19.1 = 14.9 K. Let me use ~14.9.

Actually, re-checking: the Er curve shows curvature, so linear extrapolation overestimates. The actual Er Tc at x=0.6 is probably lower. Let me use a rough estimate of ~14 K for Er at x=0.6, giving Gd ≈ 0.781×14 ≈ 10.9 K. I'll round to **10.9**.

For x=0.7: Er is probably near ~0–3 K (extrapolating the curve), giving Gd ≈ 0–2 K. I'll use **1.7** as a conservative estimate.

Wait — let me reconsider. The Er curve from x=0.3 to 0.5:
- x=0.3: 61.1, x=0.4: 45.8, x=0.5: 28.6
- Slope x=0.3→0.4: -15.3; x=0.4→0.5: -17.2

The slope is accelerating. If we continue at -19 per 0.1 step:
- x=0.6: 28.6 - 19 ≈ 9.6
- x=0.7: 9.6 - 21 ≈ -11 → 0 (non-physical)

So Er at x=0.6 ≈ 10 K, and at x=0.7 ≈ 0 K.

Gd predictions:
- x=0.600: 0.781×10 + 0.219×0 = **7.8**
- x=0.700: **0.0**

Let me finalize with these refined values.
