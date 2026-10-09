# Notes for Superconductor Tc Study

## Observations
- None of the compositions in `data/measurable.txt` are available for measurement (no physical samples exist).
- The task requires predicting Tc values for unmeasurable materials (`data/unmeasurable.txt`) based on an induced rule.

## Steps Completed
1. **Induced a rule**: Materials with oxygen content >= 6.0 and containing Hg, Cu, and Ba are predicted as high Tc. Others are low Tc.
2. **Predicted high/low** for all unmeasurable compositions (see `answer.json`).
3. **Selected final material**: `O6.080Ca1Cu2Ba2Hg1` (fewest elements among predicted high Tc materials).

## Final Answer
- Written to `answer.json`.