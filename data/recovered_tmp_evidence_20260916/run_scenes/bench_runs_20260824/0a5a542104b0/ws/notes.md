# Notes — predict m05 QED and pick a backup lead

## Task recap
- Job 1: predict QED of `m05` (SMILES `COc1ccc(cc1)C(=O)NCCCCCCCCCCC`), never synthesized → extrapolate from its OWN congeneric series (short-chain members only).
- Job 2: pick ONE pool molecule with measured QED ≥ 0.6, psa ≥ 40, NOT in m05's series.
- Budget: 17 measurements shared between both jobs.

## m05 structure
Scaffold: p-methoxybenzamide (COc1ccc(cc1)C(=O)N), N-alkyl chain = 11 carbons (CCCCCCCCCCC after N).
</think><tool_call>mcp__matlab__describe_structure<arg_key>mol_id</arg_key><arg_value>m05