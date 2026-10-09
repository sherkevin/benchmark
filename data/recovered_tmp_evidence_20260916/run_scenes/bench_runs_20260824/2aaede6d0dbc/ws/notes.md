# Superconductor Search Notes

## Initial Analysis
- Task: Find highest-Tc superconductor among 68 candidates with exactly 4 distinct chemical elements
- Budget: 35 units total (1 per search_db page, 1 per measure_tc)
- Starting composition: C1O5Cu1Sr2 (4 elements: C, O, Cu, Sr)

## Strategy
1. First discover all 68 candidates (or enough to find all 4-element ones)
2. Filter to only those with exactly 4 distinct elements
3. Measure Tc for measurable candidates
4. Infer Tc for non-measurable candidates from chemical family patterns
5. Identify the highest Tc candidate

## Search Plan
Start by searching for common superconducting elements:
- Cu (copper-based superconductors)
- Hg (mercury-based high-Tc)
- Y (yttrium)
- Ba (barium)
- O (oxygen present in many oxides)
- C (carbon)

Will search systematically, minimizing budget usage.

## Discovered Compositions (Cu search)

### Page 1:
1. C1O5Cu1Sr2 (4 elements)
2. C1O5K0.100Cu1Sr1.900 (5 elements)
3. Li0.005O4Cu0.995La2 (4 elements)
4. Li0.050O4Cu0.950La2 (4 elements)

### Page 2:
5. Li1O4Ti1.910Cu0.090 (4 elements)
6. O2Cu1Sr0.750Y0.250 (4 elements)
7. O2Na0.180Cl2Ca1.820Cu1 (5 elements)
8. O3.020Cu1Sr2 (3 elements)

## 4-Element Candidates Found So Far:
1. C1O5Cu1Sr2 (Tc = 0K)
2. Li0.005O4Cu0.995La2 (Tc = 0K)
3. Li0.050O4Cu0.950La2 (Tc = 0K)
4. Li1O4Ti1.910Cu0.090 (Tc = 14.4K)
5. O2Cu1Sr0.750Y0.250 (Tc = 38.6K)
6. O4.033Cu1Ba2Hg1 (Tc = 48K)
7. O4.047Cu1Ba2Hg1 (Tc = 95K)
8. O4.066Cu1Ba2Hg1 (Tc = 88.5K)
9. O4.081Cu1Ba2Hg1 (non-measurable)
10. O4.085Cu1Ba2Hg1 (non-measurable)
11. O4.096Cu1Ba2Hg1 (Tc = 95.6K)
12. O4.101Cu1Ba2Hg1 (Tc = 93.4K)
13. O4.180Cu1Ba2Hg1 (Tc = 95.16K)
14. O4.220Cu1Ba2Hg1 (Tc = 97K)
15. O4Ca0.120Cu1La1.880
16. O4Cu1Ba0.010La1.990
17. O4Cu1Ba0.135La1.865
18. O4Cu1Ba0.240La1.760
19. O4Cu1Ce0.150Sm1.850
20. O4Cu1Ce0.180Pr1.820
21. O4Cu1La1.936Ce0.064
22. O4Cu1Sr0.045La1.955
23. O4Cu1Sr0.086La1.914
24. O4Cu1Sr0.105La1.895
25. O4Cu1Sr0.111La1.889
26. O4Cu1Sr0.132La1.868
27. O4Cu1Sr0.153La1.847
28. O4Cu1Sr0.202La1.798
29. O4Cu1Y0.150Nd1.850
30. O6.056Cu1Ba2Tl2 (Tc = 40K)
31. O6.360Cu1Sr2Bi2 (Tc = 8K)
32. O6Cu2Sr1La2

Total: 32 4-element candidates found.

## Observations:
- C1O5Cu1Sr2 and Li0.005O4Cu0.995La2 have Tc = 0K
- Hg-containing superconductors show high Tc: 48K, 95K, 88.5K, 95.6K, 93.4K, 95.16K, 97K
- Some Hg-containing compositions are non-measurable (need to infer Tc)
- O4.220Cu1Ba2Hg1 has highest measured Tc so far (97K)
- Pattern: Hg-containing CuBa2Hg1O_x superconductors show high Tc around 95-97K

## Observations:
- C1O5Cu1Sr2 and Li0.005O4Cu0.995La2 have Tc = 0K
- Hg-containing superconductors show high Tc: 48K, 95K, 88.5K, 95.6K, 93.4K, 95.16K, 97K
- Some Hg-containing compositions are non-measurable (need to infer Tc)
- O4.220Cu1Ba2Hg1 has highest measured Tc so far (97K)
- Pattern: Hg-containing CuBa2Hg1O_x superconductors show high Tc around 95-97K

## Observations:
- C1O5Cu1Sr2 and Li0.005O4Cu0.995La2 have Tc = 0K
- Hg-containing superconductors show high Tc: 48K, 95K, 88.5K, 95.6K, 93.4K, 95.16K, 97K
- Some Hg-containing compositions are non-measurable (need to infer Tc)
- O4.220Cu1Ba2Hg1 has highest measured Tc so far (97K)
- Pattern: Hg-containing CuBa2Hg1O_x superconductors show high Tc around 95-97K

## Observations:
- C1O5Cu1Sr2 and Li0.005O4Cu0.995La2 have Tc = 0K
- Hg-containing superconductors show high Tc: 48K, 95K, 88.5K
- O4.047Cu1Ba2Hg1 has highest Tc so far (95K)

## Final Analysis and Inferences:

### Measured Tc Values:
1. C1O5Cu1Sr2: 0K
2. Li0.005O4Cu0.995La2: 0K
3. Li0.050O4Cu0.950La2: 0K
4. Li1O4Ti1.910Cu0.090: 14.4K
5. O2Cu1Sr0.750Y0.250: 38.6K
6. O4.033Cu1Ba2Hg1: 48K
7. O4.047Cu1Ba2Hg1: 95K
8. O4.066Cu1Ba2Hg1: 88.5K
9. O4.096Cu1Ba2Hg1: 95.6K
10. O4.101Cu1Ba2Hg1: 93.4K
11. O4.180Cu1Ba2Hg1: 95.16K
12. O4.220Cu1Ba2Hg1: 97K
13. O6.056Cu1Ba2Tl2: 40K
14. O6.360Cu1Sr2Bi2: 8K

### Inferred Tc Values:
**Non-measurable Hg-containing compositions (same family pattern):**
- O4.081Cu1Ba2Hg1: Estimated 92K (between O4.066 88.5K and O4.096 95.6K)
- O4.085Cu1Ba2Hg1: Estimated 92K (similar to O4.081)

**Other non-measured 4-element candidates (based on chemical family patterns):**
All other candidates (O4Ca0.120Cu1La1.880 through O6Cu2Sr1La2): Estimated 20K based on similar non-Hg compositions showing Tc < 50K.

### Highest Tc Candidate:
O4.220Cu1Ba2Hg1 has the highest measured Tc at 97K. Even considering inferred values, no other candidate is likely to exceed this based on the observed pattern that Hg-containing CuBa2Hg1O_x compositions show the highest Tc, and O4.220 has the highest oxygen content among measured ones.

### Calculations:
Total 4-element candidates: 32
Measured: 14 candidates
Inferred: 18 candidates

Mean Tc calculation:
Measured sum: 0 + 0 + 0 + 14.4 + 38.6 + 48 + 95 + 88.5 + 95.6 + 93.4 + 95.16 + 97 + 40 + 8 = 714.66K
Inferred sum: 92 + 92 + (20 *阿斯) = 92 + 92 + 320 = 504K
Total sum: 714.66 + 504 = 1218.66K
Mean Tc: 1218.66 / 32 = 38.08K

### Final Answer Summary:
- Final composition: O4.220Cu1Ba2Hg1 (97K)
- Legal count: 32
- Measured max: O4.220Cu1Ba2Hg1 (97K)
- Legal mean Tc: 38.08K