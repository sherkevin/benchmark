# v2_statehash_run2 aggregation

streams: 120

| method | mean accuracy | paired delta vs Frozen | CI95 |
|---|---:|---:|---|
| frozen | 0.733758 | — | — |
| greedy_linucb | 0.811465 | 0.077707 | [0.050793650793650794, 0.10722617743702081] |
| matched_online_sgd | 0.840340 | 0.106582 | [0.060485651214128036, 0.15454545454545454] |
| mira_context_reliability | 0.875584 | 0.141826 | [0.09612379321824321, 0.1902970108281226] |
