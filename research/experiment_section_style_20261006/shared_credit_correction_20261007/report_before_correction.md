# Fresh mechanism ablation — report

## Execution

The matched ablation used the sealed provider prediction receipt,
confirmation manifest, and five-fold fit-prior cache. It ran locally on CPU;
no provider inference, LLM call, or new GPU job was used. Both conditions have
the same 210 streams (7 methods × 5 folds × 2 seeds × 3 delays) and 98,910
events.

## Results versus Frozen

Values are pooled correctness differences in percentage points. Intervals are
10,000 deterministic bootstrap draws over 124 Bemis--Murcko scaffolds; rescue
and harm count individual confirmation decisions.

| method | natural Δ | natural CI | natural rescue/harm | controlled Δ | controlled CI | controlled rescue/harm |
|---|---:|---:|---:|---:|---:|---:|
| MIRA-Local | 0.00 | [−1.09, +1.43] | 82 / 82 | **+14.18** | **[+9.83, +19.18]** | 780 / 112 |
| MIRA-Shared | −0.25 | [−1.65, +2.90] | 114 / 126 | +7.13 | [+4.42, +11.48] | 486 / 150 |
| MIRA-Relative | 0.00 | [0.00, 0.00] | 0 / 0 | +10.15 | [+6.48, +14.44] | 520 / 42 |
| matched Online-SGD | −0.59 | [−1.59, +0.91] | 84 / 112 | +10.66 | [+6.35, +16.09] | 720 / 218 |
| Greedy LinUCB | −0.08 | [−0.54, +0.53] | 24 / 28 | +7.77 | [+4.96, +10.48] | 414 / 48 |
| BayesianRouter | −0.28 | [−0.69, +1.28] | 92 / 105 | +6.90 | [+4.04, +10.80] | 492 / 167 |

The direct mechanism contrast is the important row: MIRA-Local minus
MIRA-Shared is +0.25 pp in the natural stream with scaffold CI
$[-2.44,+1.34]$, and **+7.05 pp** in controlled drift with scaffold CI
$[+3.11,+9.98]$. Thus the controlled recovery is not explained by simply
pooling released rewards; attaching credit to the selected provider matters in
this contract. The natural result remains a stationary-safety null.

Across delays, MIRA-Local's controlled gain is +13.63, +14.52 and +14.39 pp
for $D=0,1,4$; the matched shared-credit gains are +7.13 pp at each delay.
These are paired descriptive estimates from the preregistered stream, not a
claim about natural provider nonstationarity.

## Interpretation and limits

This is the first current-fresh mechanism evidence that separates selected
provider credit from a pooled feedback signal. It supports retaining the
provider-local residual as the method's mechanism. It does not identify
family-shared versus provider-independent residuals: all four providers are
singleton families. It also does not add a post-call verifier, refusal policy,
natural provider/version drift, hERG/AMES endpoint, or end-to-end agent result.

The raw receipt and derived analysis are separate. The runner uses the same
production event contract and update clock. A second implementation now
replays the Shared-Credit streams from the sealed prior and manifest: both
conditions cover 30 streams, 4,710 decisions, 9,420 available state hashes,
and have zero score error. A score or reward mutation is rejected. The
replay checks Shared-Credit state/action integrity; it does not rerun provider
inference or the offline fit.

## Evidence hashes

- Runner: `src/probe_headless/run_fresh_mechanism_ablation_v1.py`, SHA256
  `8198b8756cb25240b7ff665f1f2e2c280c55ec4881991539e17a19752ac3ba2b`.
- Shared-credit policy: `src/probe_headless/fresh_shared_credit_policy.py`,
  SHA256 `7998d0f5ba735d766a5bbfca6d5201dfc776b09ae82fdaba827e29b68a205c1d`.
- Analyzer: `src/probe_headless/analyze_fresh_mechanism_ablation_v1.py`,
  SHA256 `978d9989ffc8d0f8fb5e11ef37ab33c2c5837f27813b573cc5a7d5a908071a90`.
- Natural raw events SHA256
  `fb044f629e339143fec6ad82ffaf9a0dfbd048586f298864b9cb0ecf0a6b9d5b`;
  summary SHA256 `569895c960247a341cee7341886c72a181ed93fa8d49d335e81f28b2c43c8c2c`;
  analysis v2 SHA256 `b242956b170d5cfde1e73b481291a300f9a73252f6b2e8bbe32a0e019cfe27e1`.
- Controlled raw events SHA256
  `8af050756386d1daa0058f3b58bf9b32ec93797ab8aab736bb8f077e2ffc4da9`;
  summary SHA256 `3c67fe7a6f3d4538e862425982030fc055864fbb7074a2b3bcf63fcde0ec73d4`;
  analysis v2 SHA256 `249349a48c70597e498178014f6108a064e3644aee06222a5249ce1d0d6c3b88`.
- Shared-Credit replay checker: `src/probe_headless/verify_fresh_shared_credit_replay_v1.py`,
  SHA256 `6bfdbc2a73b079756b59360e2760795c890f698e5649d827eb97264df90e495e`.
- Natural Shared-Credit verification SHA256
  `65d15907242d3791cba44553271a3509fcd1c2ea7311e47fabfd0a76e055bc80`;
  controlled verification SHA256
  `c807d86dc1680c1e3105591c55aff3c1cd4069afa54c69ad9c62f5603524ba71`.
