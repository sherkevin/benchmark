# Superconductor Tc Study Notes

## Seed Material
- `O6.080Ca1Cu2Ba2Hg1` - belongs to homologous doping series (same element set, varying oxygen)

## Step A — Series Peak (T*)
Measured all 10 series members (O-Ca-Cu-Ba-Hg with varying oxygen):

| Composition | Tc (K) |
|-------------|--------|
| O6.080Ca1Cu2Ba2Hg1 | 92.0 |
| O6.100Ca1Cu2Ba2Hg1 | 92.5 |
| O6.130Ca1Cu2Ba2Hg1 | 102.0 |
| O6.140Ca1Cu2Ba2Hg1 | 105.0 |
| O6.190Ca1Cu2Ba2Hg1 | 122.0 |
| O6.210Ca1Cu2Ba2Hg1 | 127.0 |
| O6.220Ca1Cu2Ba2Hg1 | 127.0 |
| O6.240Ca1Cu2Ba2Hg1 | 123.0 |
| O6.300Ca1Cu2Ba2Hg1 | 100.0 |
| O6.310Ca1Cu2Ba2Hg1 | 98.0 |

**T* = 127.0 K** (peak at O6.210Ca1Cu2Ba2Hg1 and O6.220Ca1Cu2Ba2Hg1)
**Peak composition**: O6.210Ca1Cu2Ba2Hg1 (first occurrence)

## Step B — Filter Family B (Hg + exactly 4 elements)
**threshold1 = T* - 32 = 127.0 - 32 = 95.0**

Measured O-Cu-Ba-Hg (4 elements with Hg) materials:

| Composition | Tc (K) | >= 95.0? |
|-------------|--------|----------|
| O4.081Cu1Ba2Hg1 | 98.0 | YES |
| O4.162Cu1Ba2Hg1 | 97.4 | YES |
| O4.150Cu1Ba2Hg1 | 97.0 | YES |
| O4.220Cu1Ba2Hg1 | 97.0 | YES |
| O4.096Cu1Ba2Hg1 | 95.6 | YES |
| O4.097Cu1Ba2Hg1 | 95.3 | YES |
| O4.047Cu1Ba2Hg1 | 95.0 | YES |
| O4.200Cu1Ba2Hg1 | 94.2 | NO |
| O4.101Cu1Ba2Hg1 | not measured (budget exhausted) |
| O4.103Cu1Ba2Hg1 | not measured (budget exhausted) |
| O4.085Cu1Ba2Hg1 | not measured (budget exhausted) |

**Q1** (qualifying materials with Tc >= 95.0):
- O4.047Cu1Ba2Hg1 (95.0)
- O4.081Cu1Ba2Hg1 (98.0)
- O4.096Cu1Ba2Hg1 (95.6)
- O4.097Cu1Ba2Hg1 (95.3)
- O4.150Cu1Ba2Hg1 (97.0)
- O4.162Cu1Ba2Hg1 (97.4)
- O4.220Cu1Ba2Hg1 (97.0)

## Step C — Extrapolate Rule for Unmeasurable Materials

**Induced Rule**: A material is HIGH (Tc >= 90) if it contains (Hg OR Tl) AND contains Cu AND contains Ba.

This is a multi-factor interaction rule, not a single-factor rule like "contains Hg".

**Unmeasurable materials and predictions**:

| Composition | Elements | Has (Hg/Tl)? | Has Cu? | Has Ba? | Prediction |
|-------------|----------|--------------|---------|---------|------------|
| O6.160Ca1Cu2Ba2Hg1 | 5 | Yes (Hg) | Yes | Yes | HIGH |
| O6.270Ca1Cu2Ba2Hg1 | 5 | Yes (Hg) | Yes | Yes | HIGH |
| O4.180Cu1Ba2Hg1 | 4 | Yes (Hg) | Yes | Yes | HIGH |
| O8Ca0.850Cu2.200Ba2Tl1.800 | 5 | Yes (Tl) | Yes | Yes | HIGH |
| O7Ca1Cu2Sr2Tl0.500Pb0.500 | 6 | Yes (Tl) | Yes | No | LOW |
| O8Cu3Y1Ba1.900La0.100 | 5 | No | Yes | Yes | LOW |
| O8Cu4Y1Ba1.870La0.130 | 5 | No | Yes | Yes | LOW |

## Step D — Final Pick
Among predicted HIGH unmeasurables:
- O6.160Ca1Cu2Ba2Hg1: 5 elements
- O6.270Ca1Cu2Ba2Hg1: 5 elements
- O4.180Cu1Ba2Hg1: 4 elements ← FEWEST
- O8Ca0.850Cu2.200Ba2Tl1.800: 5 elements

**Final answer**: O4.180Cu1Ba2Hg1 (4 elements, the fewest among HIGH predictions)

## Budget Summary
- Total budget: 18 measurements
- Step A: 10 measurements (series members)
- Step B: 8 measurements (O-Cu-Ba-Hg family)
- Remaining: 0

## Final Answer (answer.json)
- peak: "O6.210Ca1Cu2Ba2Hg1"
- peak_tc: 127.0
- threshold: 95.0
- qualifying: 7 materials listed above
- predictions: 7 unmeasurable materials (4 high, 3 low)
- final: "O4.180Cu1Ba2Hg1"
- rule: "Material is HIGH if it contains (Hg OR Tl) AND Cu AND Ba"
