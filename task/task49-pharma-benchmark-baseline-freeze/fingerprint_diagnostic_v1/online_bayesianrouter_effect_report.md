# BayesianRouter online effect diagnostic — submission failure

## Status

This submission is invalid as a scientific run. It produced no training, selection, feedback, or evaluation events.

- Nebula task: 01a0defa9e9e1a811a3811764244928a
- Job: jingwu_task49_bt_online_001
- Queue/card: damo_genomic_gene_a800, A800-SXM4-80GB
- Submitted code: e453c0647f58b533bcd075245b7ec1d721920f1e
- Input SHA-256: a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf
- Scope card: online_bayesianrouter_effect_card.md

## What happened

The Nebula entry launcher executes the entry file directly. The submitted commit still imported helper modules with a package-relative import (from .pharma_fingerprint_diagnostic ...). The worker therefore stopped before the first application event with:

ImportError: attempted relative import with no known parent package

Nebula reported worker exit code 138 and marked the task failed after its retry budget was exhausted. The 138 status is a platform retry wrapper around the application failure; it is not an eviction-only scientific outcome.

## Evidence

Sanitized complete logs are under:

experiments/logs/task49_fingerprint_online_20260927/full_download_sanitized_v2/

The worker log contains the entry command and the import traceback. There are no config, split, epoch, selection, feedback, evaluation, or complete application events. The raw downloaded archive was removed after sanitization because the platform environment dump contained runtime credentials.

## Consequence

No BayesianRouter online-versus-frozen comparison can be reported. The pre-registered falsification test was not reached. This failure does not change the method decision.

The file-entry import fallback and deterministic CUDA initialization were subsequently fixed and exercised by the bounded A800 integration smoke recorded in experiments/logs/task49_import_smoke_20260927_v2/result.json. That smoke is an integration check only: two streams, one fold, one seed, delay 0, two epochs; it is not an efficacy result.

## Next executable step

Submit the unchanged pre-registered diagnostic once with the fixed pushed commit, then recover complete logs and apply the existing verification script. Do not open the confirmation split or change the protocol because of this infrastructure failure.
