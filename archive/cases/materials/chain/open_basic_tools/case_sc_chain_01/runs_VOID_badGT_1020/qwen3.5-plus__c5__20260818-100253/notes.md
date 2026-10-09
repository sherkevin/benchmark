# Notes on Superconductor Tc Study

## Budget: 34 units total - EXHAUSTED

## Seed Material: O6.080Ca1Cu2Ba2Hg1 (Tc=92.0K)

## Step A - Series Peak (COMPLETED)
Homologous series (O, Ca, Cu, Ba, Hg - same elements, varying oxygen):

| Composition | Tc (K) | Sample Available |
|-------------|--------|------------------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 | Yes |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 | Yes |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 | Yes |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 | Yes |
| O6.160Ca1Cu2Ba2Hg1 | - | No |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 | Yes |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 | Yes |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 | Yes |
| O6.270Ca1Cu2Ba2Hg1 | - | No |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 | Yes |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 | Yes |

**Peak T* = 127.0K at O6.210Ca1Cu2Ba2Hg1**

## Step B - Filter family B (COMPLETED)
- threshold1 = T* - 32 = 127 - 32 = 95
- Materials with Hg AND exactly 4 elements (O, Cu, Ba, Hg)

4-element Hg materials measured:
| Composition | Tc (K) | >= 95? |
|-------------|--------|--------|
| O4.081Cu1Ba2Hg1 | 98.0 | Yes |
| O4.085Cu1Ba2Hg1 | 92.0 | No |
| O4.096Cu1Ba2Hg1 | 95.6 | Yes |
| O4.097Cu1Ba2Hg1 | 95.3 | Yes |
| O4.101Cu1Ba2Hg1 | 93.4 | No |
| O4.103Cu1Ba2Hg1 | 92.5 | No |
| O4.150Cu1Ba2Hg1 | 97.0 | Yes |

**Q1 (qualifying): O4.081Cu1Ba2Hg1, O4.096Cu1Ba2Hg1, O4.097Cu1Ba2Hg1, O4.150Cu1Ba2Hg1**

## Step C - Extrapolate (COMPLETED)
Unmeasurable materials found:
- O4.047Cu1Ba2Hg1 (4 elements) → predicted low (oxygen too low)
- O4.066Cu1Ba2Hg1 (4 elements) → predicted low (oxygen too low)
- O4.160Cu1Ba2Hg1 (4 elements) → predicted high (near optimal 4.15)
- O4.180Cu1Ba2Hg1 (4 elements) → predicted low (beyond optimal)
- O6.160Ca1Cu2Ba2Hg1 (5 elements) → predicted high (within range)
- O6.270Ca1Cu2Ba2Hg1 (5 elements) → predicted high (within range)

**Rule:** Tc depends on optimal oxygen doping for each material family. Materials with oxygen content significantly below the optimal range have Tc < 90 (low), while those within or near the measured optimal range have Tc >= 90 (high).

## Step D - Final pick (COMPLETED)
Among predicted high unmeasurable materials:
- O4.160Cu1Ba2Hg1 (4 elements)
- O6.160Ca1Cu2Ba2Hg1 (5 elements)
- O6.270Ca1Cu2Ba2Hg1 (5 elements)

**Final answer: O4.160Cu1Ba2Hg1** (fewest elements = 4)
