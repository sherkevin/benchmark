# Notes — predicting the Gd-series Tc curve

## 1. Family

`RE(1-x) Pr(x) Ba2 Cu3 O7` cuprates. Pr substitution for the rare earth RE suppresses Tc;
the suppression rate depends on RE. Three RE members present in the DB: **Er, Nd, Gd**.
Gd has not been synthesized → must predict.

## 2. Discovery (search_db)

- `search_db("Pr")` → 30 compositions across Er / Nd / Gd.
- `search_db("Gd")` → 11 Gd compositions (the full Gd series to predict).
- `search_db("Er")` → 9 Er compositions.
- `search_db("Nd")` → 10 Nd compositions.

### Gd series (x = Pr fraction) — 11 compositions, ALL unsynthesized
| x      | composition              |
|--------|--------------------------|
| 0.030  | O7Cu3Ba2Pr0.030Gd0.970   |
| 0.078  | O7Cu3Ba2Pr0.078Gd0.922   |
| 0.150  | O7Cu3Ba2Pr0.150Gd0.850   |
| 0.200  | O7Cu3Ba2Pr0.200Gd0.800   |
| 0.300  | O7Cu3Ba2Pr0.300Gd0.700   |
| 0.400  | O7Cu3Ba2Pr0.400Gd0.600   |
| 0.500  | O7Cu3Ba2Pr0.500Gd0.500   |
| 0.600  | O7Cu3Ba2Pr0.600Gd0.400   |
| 0.700  | O7Cu3Ba2Pr0.700Gd0.300   |
| 0.800  | O7Cu3Ba2Pr0.800Gd0.200   |
| 0.900  | O7Cu3Ba2Pr0.900Gd0.100   |

## 3. Measured Tc (real measure_tc calls)

### Er series (x, Tc/K)
0.030 → 93.9 ; 0.076 → 91.1 ; 0.150 → 82.9 ; 0.200 → 76.4 ; 0.300 → 61.1 ;
0.400 → 45.8 ; 0.500 → 28.6 ; 0.800 → 0.0 ; 0.900 → 0.0

### Nd series (x, Tc/K)
0.030 → 85.1 ; 0.080 → 68.2 ; 0.150 → 40.7 ; 0.200 → 31.4 ; 0.300 → 5.6 ;
0.500 → 0.0 ; 0.600 → 0.0 ; 0.700 → 0.0 ; 0.800 → 0.0 ; 0.900 → 0.0

### Gd series → all `sample_available: false` (confirmed, no Tc).

## 4. Pattern inferred

- At a given Pr fraction x, Tc ranks **Er > Nd** consistently: e.g. x=0.15 Er 82.9 vs Nd 40.7;
  x=0.30 Er 61.1 vs Nd 5.6. Pr suppresses Tc faster for Nd than Er.
- Both series cross zero at high x: Er dies between x=0.5 (28.6) and x=0.8 (0.0);
  Nd dies between x=0.3 (5.6) and x=0.5 (0.0).
- Gd sits between Er and Nd in the rare-earth series (ionic radius / mass ordering:
  Nd (large) → Gd → Er (small)). So Gd's Tc curve should lie **between** the Er and Nd curves.

## 5. Extrapolation method

Interpolate each synthesized series onto the Gd x-grid, then take the Gd prediction as the
midpoint (average) of the Er and Nd interpolated values at each Gd x — the natural
"interpolate across the RE axis at fixed x". Clip negatives to 0 (no superconductivity).

### Linear interpolation of Er onto Gd x-grid (piecewise-linear between measured points)
- x=0.030 → 93.9 (direct)
- x=0.078 → between 0.076(91.1) & 0.150(82.9): t=(0.078-0.076)/0.074=0.0270 → 91.1-0.0270*8.2 = 90.88
- x=0.150 → 82.9 (direct)
- x=0.200 → 76.4 (direct)
- x=0.300 → 61.1 (direct)
- x=0.400 → 45.8 (direct)
- x=0.500 → 28.6 (direct)
- x=0.600 → between 0.500(28.6) & 0.800(0.0): t=0.1/0.3=0.333 → 28.6-0.333*28.6 = 19.07
- x=0.700 → between 0.500(28.6) & 0.800(0.0): t=0.667 → 28.6-0.667*28.6 = 9.53
- x=0.800 → 0.0 (direct)
- x=0.900 → 0.0 (direct)

### Linear interpolation of Nd onto Gd x-grid
- x=0.030 → 85.1 (direct)
- x=0.078 → between 0.030(85.1) & 0.080(68.2): t=0.048/0.050=0.96 → 85.1-0.96*16.9 = 68.88
- x=0.150 → 40.7 (direct)
- x=0.200 → 31.4 (direct)
- x=0.300 → 5.6 (direct)
- x=0.400 → between 0.300(5.6) & 0.500(0.0): t=0.5 → 5.6-0.5*5.6 = 2.8
- x=0.500 → 0.0
- x=0.600 → 0.0
- x=0.700 → 0.0
- x=0.800 → 0.0
- x=0.900 → 0.0

### Gd predicted = avg(Er interp, Nd interp), clipped to ≥0
- x=0.030 → (93.9+85.1)/2  = 89.5
- x=0.078 → (90.88+68.88)/2 = 79.9
- x=0.150 → (82.9+40.7)/2   = 61.8
- x=0.200 → (76.4+31.4)/2   = 53.9
- x=0.300 → (61.1+5.6)/2    = 33.4
- x=0.400 → (45.8+2.8)/2    = 24.3
- x=0.500 → (28.6+0.0)/2    = 14.3
- x=0.600 → (19.07+0.0)/2   = 9.5
- x=0.700 → (9.53+0.0)/2    = 4.8
- x=0.800 → 0.0
- x=0.900 → 0.0
