---
license: cc-by-4.0
task_categories:
- question-answering
- text-generation
language:
- en
tags:
- biomedical
- open-ended-qa
- agentic-evaluation
- medical
- research-questions
pretty_name: OpenBioRQ
size_categories:
- n<1K
configs:
- config_name: full_core
  data_files: full_core_657.jsonl
- config_name: frozen_core
  data_files: frozen_core_423.jsonl
- config_name: rubrics
  data_files: rubrics.jsonl
- config_name: predictions
  data_files: predictions/*/predictions.jsonl
---

# OpenBioRQ — Open Biomedical Research Questions

📄 **Paper:** *OpenBioRQ: Unsolved Biomedical Research Questions for Agents* (Minbyul Jeong, 2026) ·
🤗 **Dataset:** [Minbyul/OpenBioRQ](https://huggingface.co/datasets/Minbyul/OpenBioRQ)

OpenBioRQ is a benchmark of **open-ended, currently-unresolved biomedical research
questions** extracted from the primary literature and clinical-trial records, refined
to be self-contained, and graded by per-question rubrics. It is built for **agentic
evaluation**: a model is given a question, may use retrieval/MCP tools, and its answer
is scored against an item-specific checklist by an LLM judge.

This release contains the **evaluation sets and rubrics** used in the paper — the
slices that the evaluation pipeline actually consumes. The full 12.5K construction
corpus is not part of this release.

## Files

| File | Rows | What it is |
|------|------|-----------|
| `full_core_657.jsonl` | 657 | The core evaluation set (one record per question, with `gold_answer`). |
| `frozen_core_423.jsonl` | 423 | The **frozen core** — the subset of the 657 that all three roster models (GLM-5.1, Qwen3.6, DeepSeek-V4) fail at T=0 (checklist score < 0.5). The primary discriminating hard split. |
| `rubrics.jsonl` | 657 | Grading rubric (`criteria` checklist) for each question, keyed by `task_id`. |
| `frozen_core_ids.json` | 423 | The `task_id` list defining the frozen core (for reproducible splitting). |
| `predictions/<model>/` | — | Per-model **predictions + judge verdicts** for all 11 leaderboard models (see below). |

`frozen_core_423` ⊂ `full_core_657`; every question has exactly one rubric in
`rubrics.jsonl` (joinable on `task_id`).

## Predictions (per-model, for full reproducibility)

`predictions/` ships the actual model outputs behind every leaderboard row, so the
numbers can be re-derived end to end. Each of the 11 models has a directory
`predictions/<model>/` with three files (all keyed/joinable on `task_id`):

| file | what it is |
|------|-----------|
| `predictions.jsonl` | The model's `model_answer` plus its **full agentic trajectory**: `tool_calls`, the round-by-round `trace` (messages + tool responses), `tokens`, `wall_time`. |
| `checklist.jsonl` | The frozen-rubric judge verdicts: `checklist_score`, `n_criteria`, and per-criterion `verdicts` (`{id, v∈{met,partial,not_met}, why}`). |
| `summary.json` | Recomputed `solve@0.5` and `avg_score` on **full_core_657** and **frozen_core_423**. |

All runs cover the full 657; **frozen-core (423) numbers are obtained by filtering to
`frozen_core_ids.json`** (no separate frozen run is shipped — the frontier `*_fullcore`
runs already contain the frozen-423 predictions verbatim).

| model | role | tools | full-657 solve@0.5 | frozen-423 solve@0.5 |
|-------|------|:-----:|-------------------:|---------------------:|
| `glm-5.1` | roster | ✓ | 26.6 | 0* |
| `qwen3.6` | roster | ✓ | 11.7 | 0* |
| `deepseek-v4` | roster | ✓ | 6.2 | 0* |
| `qwen3-235b` | held-out | ✓ | 3.5 | 2.1 |
| `glm-5` | held-out | ✓ | 26.1 | 16.5 |
| `qwen3.5-397b` | held-out | ✓ | 22.7 | 16.8 |
| `glm-5.1-no-tools` | ablation | ✗ | 30.7 | 19.9 |
| `gemini-3-pro` | frontier | ✓ | 35.6 | 26.0 |
| `opus-4.7` | frontier | ✓ | 48.8 | 38.0 |
| `gpt-5.5` | frontier | ✓ | 64.4 | 56.0 |
| `gpt-5.5-no-tools` | frontier | ✗ | 61.4 | 56.5 |

\* roster frozen-core is 0 **by construction** (the frozen core is the subset all three
roster models fail at T=0). All scores are from the GLM-5.1 checklist judge at T=0.

> **Reproduction.** Per model: `solve@0.5 = mean(checklist_score ≥ 0.5)` over
> `checklist.jsonl`; restrict the `task_id`s to `frozen_core_ids.json` for the frozen
> column. The roster / held-out / no-tool numbers reproduce the paper to ≤1 pt (rounding).
> **Honest judge-noise note:** the frontier *frozen-423* values here are re-judged from the
> 657-run and sit ~3 pp below the paper's published first-party headline
> (Gemini 28.8 / Opus 37.8 / GPT-5.5 59.6); this is the documented checklist-judge
> temperature drift (≤4.1 pp envelope), not a different set of predictions — the
> trajectories are identical. The paper reports the first-party headline; this release
> reports the recomputed value and states the gap.

> **Privacy.** Predictions are scrubbed: API keys, internal serving hostnames/IPs, and
> local paths are removed and the build is verified to contain none. Raw run `summary.json`
> / `per_task.csv` (which embedded the internal serving node) are **not** shipped;
> `summary.json` here is regenerated clean. Tool responses are public biomedical-API
> content (PubMed, ClinicalTrials.gov, etc.).

## Schema

**Question record** (`full_core_657.jsonl`, `frozen_core_423.jsonl`):

| field | description |
|-------|-------------|
| `task_id` | Unique question id (`PMID:…#k`, `NCT…#k`, or `<arxiv>#k`). Join key. |
| `self_contained_question` | The question as posed to the model (refined to stand alone). |
| `original_question` | The question as first extracted from the source. |
| `gold_answer` | Reference answer used for rubric grounding (not a single gold string for exact-match). |
| `question_type` | mechanism / treatment / methodology / diagnosis / epidemiology / prognosis / prevention. |
| `open_status` | `open` or `partially_answered`. |
| `why_open`, `status_reasoning` | Why the question is considered unresolved. |
| `taxonomy_l1/l2/l3` | 12-category biomedical taxonomy. |
| `clinical_domain`, `verification_venues`, `relevant_mcp_tools` | Metadata. |
| `difficulty`, `difficulty_clinical_knowledge`, `difficulty_research_depth`, `difficulty_multi_step_reasoning` | Difficulty annotations. |
| `source_id`, `source_url`, `source_title` | Provenance. |

**Rubric record** (`rubrics.jsonl`): `task_id`, `question`, `criteria` — a list of
weighted checklist items `{id, text, type, weight}`, where `type` ∈
`must_mention / must_acknowledge / must_ground / must_avoid` (5–8 items per question).
Score = Σ(weight·v)/Σ(weight) with v ∈ {met:1.0, partial:0.5, not_met:0.0}; the "solve"
threshold used in the paper is **0.5**.

## Composition (full_core_657)

- **Source:** PubMed/PMID 541, ClinicalTrials NCT 83, arXiv/other 33.
- **Type:** mechanism 328, treatment 202, methodology 57, diagnosis 36, epidemiology 21, prognosis 8, prevention 5.
- **Status:** open 433, partially_answered 224.
- **Top taxonomy_l1:** Clinical Medicine 145, Neuroscience & Psychiatry 120, Oncology 94, Infectious Disease & Immunology 63, Public Health 43, Surgical Sciences 43.

## Reference results (T=0, ten-tool harness, GLM-5.1 checklist judge)

solve@0.5 on the frozen core (423):

| Model | frozen-core solve@0.5 |
|-------|----------------------:|
| Gemini-3-Pro | 28.8% |
| Opus-4.7 | 37.8% |
| GPT-5.5 (no tools) | 55.6% |
| GPT-5.5 | 59.6% |

The frozen core is hard but not saturated — the strongest single agent leaves ~40%
unsolved — and tool access confers no measurable advantage (CIs overlap), making this a
discriminating set for open-ended biomedical reasoning.

## Intended use

Agentic / open-ended QA evaluation graded against the per-question rubric. Because
answers are checklist-graded (not exact-match), use an LLM judge with `rubrics.jsonl`
rather than string comparison. `gold_answer` grounds the rubric; it is not a single
canonical answer.

## Citation

If you use OpenBioRQ, please cite:

```bibtex
@misc{jeong2026openbiorq,
  title         = {OpenBioRQ: Unsolved Biomedical Research Questions for Agents},
  author        = {Minbyul Jeong},
  year          = {2026},
  howpublished  = {\url{https://huggingface.co/datasets/Minbyul/OpenBioRQ}},
  note          = {Dataset and benchmark}
}
```

## License

CC BY 4.0. Questions are derived from open-access literature and public clinical-trial
registries; provenance is retained in each record.
