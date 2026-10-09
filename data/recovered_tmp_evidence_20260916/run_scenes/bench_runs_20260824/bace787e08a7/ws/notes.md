# QED Prediction for m05

## Search Strategy

1. **Initial search**: Used scaffold substring `COc1ccc(cc1)C(=O)NC` to find all congeners in the same series as m05.
2. **Results**: Found 6 total matches across 2 pages:
   - Page 1: m05 (target), m15, m17, m22
   - Page 2: m24, m28
3. **Budget used**: 2 units for search (pages 1 and 2)

## Measured QED Values

Measured all available congeners (excluding m05 which returns probe_unavailable):

| Molecule | Chain Length | SMILES | QED |
|----------|--------------|--------|-----|
| m15 | C2 (ethyl) | `COc1ccc(cc1)C(=O)NCC` | 0.7621 |
| m24 | C3 (propyl) | `COc1ccc(cc1)C(=O)NCCC` | 0.7919 |
| m28 | C4 (butyl) | `COc1ccc(cc1)C(=O)NCCCC` | 0.7518 |
| m17 | C5 (pentyl) | `COc1ccc(cc1)C(=O)NCCCCC` | 0.7498 |
| m22 | C6 (hexyl) | `COc1ccc(cc1)C(=O)NCCCCCC` | 0.7377 |
| m05 | C11 (undecyl) | `COc1ccc(cc1)C(=O)NCCCCCCCCCCC` | Unknown (to predict) |

**Budget used**: 5 measurements = 5 units

## Trend Analysis

Looking at the QED values vs. chain length:

1. **C2 to C3**: Increase from 0.7621 to 0.7919 (+0.0298)
2. **C3 to C4**: Decrease from 0.7919 to 0.7518 (-0.0401)
3. **C4 to C5**: Small decrease from 0.7518 to 0.7498 (-0.0020)
4. **C5 to C6**: Decrease from 0.7498 to 0.7377 (-0.0121)

The pattern shows:
- Peak QED at C3 chain (propyl)
- Gradual decrease from C3 to C6
- The decrease appears to be stabilizing or continuing slowly

## Additional Searches

Performed additional searches to check for intermediate chain lengths:
1. Searched `COc1ccc(cc1)C(=O)NCCCC` - found same molecules (m05, m17, m22, m28)
2. Searched `COc1ccc(cc1)C(=O)NCCCCCC` - found only m05 and m22

**No molecules with chain lengths between C6 and C11 were found** in the collection.

## Trend Fitting

Given the observed data:
.### Initial observations:
- The QED peaks at C3 (0.7919)
- There's a significant drop from C3 to C4 (-0.0401)
- From C4 onward, the decline is more gradual

### Pattern analysis:
The data suggests a nonlinear relationship. Possible explanations:
1. Optimal chain length for drug-like properties is C3 (propyl)
2. Longer chains increase lipophilicity (alogp increases consistently) which may eventually reduce QED
3. The rotatable bonds count increases linearly with chain length, potentially reducing QED

### Mathematical fitting:
Given the limited data points (5) and clear nonlinear trend, several approaches could work:
1. **Polynomial fit**: Could capture the peak at C3 and subsequent decline
2. **Exponential decay**: After the peak at C3, QED appears to decay
3. **Linear extrapolation**: From C4-C6, the slope is approximately -0.006 per carbon

### Key consideration:
The task warns against assuming a straight line and against simply quoting the nearest measured molecule. The C6 molecule (0.7377) is the nearest measured, but we need to extrapolate beyond the measured range (C6 to C11).

## Extrapolation Method

### Analyzing the trend:
The QED shows a complex nonlinear relationship with chain length:
1. C2 to C3: Increase (+0.0298)
2. C3 to C4: Significant decrease (-0.0401)
3. C4 to C5: Minimal decrease (-0.0020)
4. C5 to C6: Moderate decrease (-0.0121)

### Examining molecular descriptors:
All congeners share the same scaffold, so differences come from the alkyl chain:

| Chain | Carbons | alogp | rotb | mw | QED |
|-------|---------|-------|------|----|-----|
| C2    | 2       | 1.445 | 3    | 179.22 | 0.7621 |
| C3    | 3       | 1.835 | 4    | 193.25 | 0.7919 |
| C4    | 4       | 2.225 | 5    | 207.27 | 0.7518 |
| C5    | 5       | 2.615 | 6    | 221.30 | 0.7498 |
| C6    | 6       | 3.005 | 7    | 235.33 | 0.7377 |
| C11   | 11      | 4.956 | 12   | 305.46 | Unknown |

Key observations:
1. alogp increases linearly by ~0.39 per carbon
2. rotb increases linearly by 1 per carbon
3. QED peaks at moderate lipophilicity (alogp ~1.8)
4. Beyond optimal point, increasing lipophilicity and rotatable bonds reduce QED

### Rate analysis:
From C4 to C6 (representing post-peak region):
- alogp increase: 0.78 over 2 carbons
- QED decrease: 0.0141
- Rate: 0.0181 QED decrease per 0.39 alogp increase

From C6 to C11:
- alogp increase: 1.951 (5 × 0.39)
- Using C4-C6 rate: (1.951/0.39) × 0.0181 = 5 × 0.0181 = 0.0905 decrease
- Prediction: 0.7377 - 0.0905 = 0.6472

### Alternative approach - rotatable bonds:
From C4 to C6:
- rotb increase: 2
- QED decrease: 0.0141
- Rate: 0.00705 per rotb

From C6 to C11:
- rotb increase: 5
- Predicted decrease: 5 × 0.00705 = 0.03525
- Prediction: 0.7377 - 0.03525 = 0.70245

### Considering the curvature:
The C5-C6 decrease (-0.0121) is larger than C4-C5 (-0.0020), suggesting the decline might accelerate with longer chains as properties move further from optimal range.

## Final Prediction

After considering:
1. The clear peak at C3 followed by decline
2. Increasing alogp and rotb with chain length
3. The nonlinear nature of the trend
4. Two extrapolation approaches yielding 0.647-0.702 range
5. The acceleration suggested by C5-C6 being steeper than C4-C5

**Predicted QED for m05 (C11 chain): 0.675**

This value:
- Reflects significant extrapolation beyond measured range (C6 to C11)
- Accounts for accelerating decline suggested by data
- Is not simply the nearest measured value (0.7377)
- Respects the nonlinear trend observed
- Falls between the alogp-based (0.647) and rotb-based (0.702) predictions

## Budget Summary
- Search: 4 units (4 search calls)
- Measurements: 5 units (5 measurements)
- **Total used**: 9 units
- **Remaining**: 3 units