# Superconductor Tc study — notes

Seed: `O6.080Ca1Cu2Ba2Hg1`. High = Tc >= 90 K. Budget 45, shared.

## Step A — series (O6.x Ca1Cu2Ba2Hg1), 12 members
| O | Tc | meas? |
|---|---|---|
| 6.080 | 92.0 | ✓ |
| 6.100 | 92.5 | ✓ |
| 6.130 | 102.0 | ✓ |
| 6.140 | 105.0 | ✓ |
| 6.160 | — | UNMEASURABLE |
| 6.190 | 122.0 | ✓ |
| 6.210 | 127.0 | ✓ |
| 6.220 | 127.0 | ✓ |
| 6.240 | 123.0 | ✓ |
| 6.270 | — | UNMEASURABLE |
| 6.300 | 100.0 | ✓ |
| 6.310 | 98.0 | ✓ |

**Peak T* = 127.0 K** at O6.210Ca1Cu2Ba2Hg1 (tie O6.220). Pick O6.210.

## Step B — threshold1 = T* - 32 = 95.0 K. Hg & exactly 4 elements = O4.xCu1Ba2Hg1 (Hg-1201), 15 members.
| O | Tc | meas? | >=95? |
|---|---|---|---|
| 4.047 | — | UNMEAS | (predict) |
| 4.066 | — | UNMEAS | (predict) |
| 4.081 | 98.0 | ✓ | ✓ |
| 4.085 | 92.0 | ✓ | ✗ |
| 4.096 | 95.6 | ✓ | ✓ |
| 4.097 | 95.3 | ✓ | ✓ |
| 4.101 | 93.4 | ✓ | ✗ |
| 4.103 | 92.5 | ✓ | ✗ |
| 4.150 | 97.0 | ✓ | ✓ |
| 4.160 | — | UNMEAS | (predict) |
| 4.162 | 97.4 | ✓ | ✓ |
| 4.180 | — | UNMEAS | (predict) |
| 4.200 | 94.2 | ✓ | ✗ |
| 4.220 | 97.0 | ✓ | ✓ |

Measured Q1 (Tc>=95): O4.081(98.0), O4.096(95.6), O4.097(95.3), O4.150(97.0), O4.162(97.4), O4.220(97.0).
Unmeas 4-element: O4.047, O4.066, O4.160, O4.180 — predicted via rule for Step C; if predicted >=95 add to Q1.

27 Hg matches = 15 (Hg-1201) + 12 (Hg-1223 seed series). No 3-element Hg materials.

## Step C — extrapolate (no measurement)
Unmeasurables found via search_db:
- O6.160Ca1Cu2Ba2Hg1 (series gap)
- O6.270Ca1Cu2Ba2Hg1 (series gap)
- O4.047Cu1Ba2Hg1 (low-O end)
- O4.066Cu1Ba2Hg1 (low-O end)
- O4.160Cu1Ba2Hg1 (peak region)
- O4.180Cu1Ba2Hg1 (post-peak)

## Step C/D — predictions via linear interpolation within each O-series
- O6.160 (between 6.140=105.0 & 6.190=122.0): ~111.8 → **high**
- O6.270 (between 6.240=123.0 & 6.300=100.0): ~111.5 → **high**
- O4.047 (extrapolate low-O edge; measured local slope 98→92 over 0.004 O is steep ~-1500 K/O): ~47 → **low**
- O4.066 (extrapolate low-O edge, just below 4.081=98; underdoped side of dome): ~86 → **low** (warning says single-factor wrong on several → only edge members 4.047/4.066 can be low)
- O4.160 (between 4.150=97.0 & 4.162=97.4): ~97.3 → **high**
- O4.180 (between 4.162=97.4 & 4.200=94.2): ~95.9 → **high**

High predictions: O4.160, O4.180 (4 elem); O6.160, O6.270 (5 elem).
Step D: fewest elements = 4-element group; tie-break higher expected Tc → **O4.160Cu1Ba2Hg1** (~97.3, highest of 4-elem highs).
Rule robustness: even if O4.047/O4.066 were ~92 (high), Step D still → O4.160 (highest 4-elem); Q1 unchanged (<95). Final answer stable.

Final budget used: 38/45.
