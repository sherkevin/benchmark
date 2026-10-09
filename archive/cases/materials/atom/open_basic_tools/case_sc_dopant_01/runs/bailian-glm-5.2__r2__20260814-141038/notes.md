# Notes — predicting the Gd-series Tc curve

## Task
Predict Tc(K) of every Gd-series composition `RE(1-x)Pr(x)Ba2Cu3O7` (RE = Gd),
which has **not been synthesized** and so cannot be `measure_tc`'d. Infer it from the
synthesized members of the same Pr-substituted RE-123 cuprate family.

## 1. Discover the family (search_db)
Searched every rare-earth symbol. The full family in the DB is **30 compositions**,
all `O7Cu3Ba2Pr(x)RE(1-x)`, with RE ∈ {**Nd**, **Gd**, **Er**}:

- **Nd** (Z=60), synthesized, 10 comps: x_Pr = 0.030,0.080,0.150,0.200,0.300,0.500,0.600,0.700,0.800,0.900
- **Er** (Z=68), synthesized, 9 comps:  x_Pr = 0.030,0.076,0.150,0.200,0.300,0.400,0.500,0.800,0.900
- **Gd** (Z=64), **un-synthesized target**, 11 comps: x_Pr = 0.030,0.078,0.150,0.200,0.300,0.400,0.500,0.600,0.700,0.800,0.900

No other RE (Y, Sm, Eu, Dy, Ho, Yb, Lu, La, Tb, Tm, Ce) is present.

## 2. Measure the synthesized samples (real measure_tc calls)

### Nd series (RE = Nd) — fragile to Pr
| x_Pr | Tc (K) |
|------|--------|
| 0.030 | 85.1 |
| 0.080 | 68.2 |
| 0.150 | 40.7 |
| 0.200 | 31.4 |
| 0.300 | 5.6 |
| 0.500 | 0.0 |
| 0.600 | 0.0 |
| 0.700 | 0.0 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

### Er series (RE = Er) — robust to Pr
| x_Pr | Tc (K) |
|------|--------|
| 0.030 | 93.9 |
| 0.076 | 91.1 |
| 0.150 | 82.9 |
| 0.200 | 76.4 |
| 0.300 | 61.1 |
| 0.400 | 45.8 |
| 0.500 | 28.6 |
| 0.800 | 0.0 |
| 0.900 | 0.0 |

## 3. Pattern inferred
1. Pr substitution **suppresses** Tc in every RE member; at high enough x_Pr the
   material loses superconductivity (Tc = 0), as expected (Pr-123 itself is
   non-superconducting).
2. The **rate of suppression depends on RE**: Nd (lighter, larger ion, Z=60) is
   *fragile* — already 0 by x=0.5; Er (heavier, smaller ion, Z=68) is *robust* —
   still 28.6 K at x=0.5.
3. **Gd is the exact lanthanide-series midpoint between Nd and Er**: Z_Gd=64 lies
   halfway between Z_Nd=60 and Z_Er=68 (fraction f = 4/8 = 0.5). The
   Pr-suppression property varies smoothly across the lanthanide series, so Gd's
   Tc(x) is the midpoint of the Nd and Er curves.

## 4. Extrapolation model (bilinear interpolation, anchored on real measurements)
For each Gd Pr-content x:
```
Tc_Gd(x) = 0.5 * [ Tc_Nd(x) + Tc_Er(x) ]
```
where `Tc_Nd(x)` and `Tc_Er(x)` are obtained by **linear interpolation** along each
RE's measured Tc-vs-x curve (and clamped to 0 once a neighbor curve is already 0).

### Worked Gd predictions
| x_Pr | Tc_Nd(x) [interp] | Tc_Er(x) [interp] | Tc_Gd = avg |
|------|-------------------|-------------------|-------------|
| 0.030 | 85.1 (meas) | 93.9 (meas) | 89.5 |
| 0.078 | 68.876 | 90.878 | 79.9 |
| 0.150 | 40.7 (meas) | 82.9 (meas) | 61.8 |
| 0.200 | 31.4 (meas) | 76.4 (meas) | 53.9 |
| 0.300 | 5.6 (meas) | 61.1 (meas) | 33.4 |
| 0.400 | 2.8 | 45.8 (meas) | 24.3 |
| 0.500 | 0.0 (meas) | 28.6 (meas) | 14.3 |
| 0.600 | 0.0 | 19.067 | 9.5 |
| 0.700 | 0.0 | 9.533 | 4.8 |
| 0.800 | 0.0 (meas) | 0.0 (meas) | 0.0 |
| 0.900 | 0.0 (meas) | 0.0 (meas) | 0.0 |

### Sanity checks
- Endpoint x→0: interpolated Gd at x=0.030 is 89.5 K; real GdBa2Cu3O7 (x=0) is
  ~92–93 K — consistent, the curve extrapolates to the right endpoint.
- Both neighbors are 0 at x≥0.8, so Gd = 0 there (no longer superconducting).
- Gd's curve sits between the fragile Nd curve and the robust Er curve at every
  x — the physically expected ordering.

## 5. Assumptions (stated for transparency)
- The Pr-suppression-vs-RE dependence is assumed monotonic and ~linear across the
  Nd→Gd→Er span. With only two synthesized RE anchors, the midpoint is the
  uniquely principled interpolation; Gd's Z being exactly midway (64 = (60+68)/2)
  is what makes the 50/50 average the natural choice.
- Linear interpolation along x_Pr within each RE curve (no curvature model is
  identifiable from the sparse, near-linear measured points).
