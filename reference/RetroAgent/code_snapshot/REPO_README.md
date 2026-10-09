# RetroAgent

RetroAgent is an LLM agent for multi-step retrosynthesis planning. It decomposes a target molecule into commercially available building blocks by searching over an AND-OR graph of molecules and reactions, driven entirely by tool calls. The graph, the single-step reaction model, building-block lookup, and molecular scoring all live in an HTTP tool server that forms the agent's harness; the policy is a language model that reasons over the returned observations and issues the next tool call. RetroAgent is trained end-to-end with reinforcement learning (GSPO) on the Qwen3-4B-Instruct-2507 base model, using the [slime](https://github.com/THUDM/slime) framework (Megatron + SGLang) on a single 8xH200 node, with a reward shaped around search-budget efficiency rather than final success alone.

- Paper: *RetroAgent: Harnessing LLMs to Search Over Structured Memory for Agentic Retrosynthesis Planning* (COLM 2026)
- Weights and data: https://huggingface.co/SXKDZ/RetroAgent
- Code: https://github.com/SXKDZ/RetroAgent

---

## Table of Contents

1. [Method Overview](#method-overview)
2. [Installation](#installation)
3. [Data Preparation](#data-preparation)
4. [Tool Server](#tool-server)
5. [Training](#training)
6. [Evaluation](#evaluation)
7. [Zero-shot and Inference with Released Weights](#zero-shot-and-inference-with-released-weights)
8. [Repository Structure](#repository-structure)
9. [Citation and License](#citation-and-license)

---

## Method Overview

RetroAgent frames retrosynthesis as a search problem over a structured memory that the tool server maintains:

- **AND-OR graph memory.** Molecules are OR-nodes (`M1, M2, ...`): a molecule may be decomposed in several ways, and any one successful decomposition suffices. Reactions are AND-nodes (`R1, R2, ...`): every reactant of a reaction must be solved for the reaction to count. `OPEN` / `SOLVED` status propagates automatically up the graph; a route is complete when the root molecule becomes `SOLVED`. The graph is a per-session `GraphMemory` held inside the tool server, not in the model's context.
- **Tools.** The agent perceives and acts only through tool calls. Single-step disconnections are proposed by an ML template model (`predict_templates`), applied to the graph via `expand`, and molecules are scored and looked up with chemistry tools. The agent calls `get_frontier` to see which `OPEN` molecules still need to be decomposed, so it always knows what remains to be solved.
- **Reward.** A rollout that finds a complete route earns a base reward of `+1`, reduced by soft penalties on the search budget (number of single-step model calls) and route depth, plus small per-turn penalties for invalid or failed tool calls. This trains the agent to solve targets with fewer, better-chosen expansions rather than brute-forcing the search. The full reward specification is in the [Training](#training) section.

A rollout is a multi-turn tool-calling episode: the model emits a `<tool_call>` block, the tool server executes it against the target's session, and the observation is injected back as a non-trainable user turn. Training optimizes the policy with GSPO (sequence-level clipping) under the slime RL framework.

---

## Installation

The runtime is split in two:

- The tool server runs on the host (native Python). It loads the building-block index, reaction templates, the single-step ML model, and the SCScore model, and serves them over HTTP.
- Training and inference run inside the `slimerl/slime:v0.3.0` Docker container (which ships Ray, Megatron-LM, slime, and SGLang). The container reaches the host tool server via `--network=host` at `127.0.0.1:8100`.

The heavy training/inference dependencies (`torch`, `sglang`, `ray`, `megatron`) come from the Docker image, so they are not in `requirements.txt`.

### 1. Docker image

```bash
docker pull slimerl/slime:v0.3.0
```

Requires an NVIDIA GPU host with `nvidia-container-toolkit`. The released configuration targets a single 8xH200 node.

### 2. Host Python environment (for the tool server + data prep + eval driver)

```bash
pip install -r requirements.txt
```

The single-step retrosynthesis model lives in `external/desp/` (`RetroPredictor`); no extra install step is needed beyond having the repository on `PYTHONPATH`. Run all host commands from the repository root.

### 3. Data files and weights

The trained weights are on the HuggingFace release ([SXKDZ/RetroAgent](https://huggingface.co/SXKDZ/RetroAgent)). The runtime data files are provided separately (see [Data Preparation](#data-preparation) for the exact list and sources): building blocks are from eMolecules, and the reaction templates and single-step model weights are from DESP/Retro\*. Place all of them under `datasets/`.

---

## Data Preparation

### Required runtime files (`datasets/`)

**Tool-server runtime (loaded once per process):**

| File | Purpose |
|------|---------|
| `datasets/processed/building_blocks.parquet` | Building blocks (`smiles`, `idx`); exact canonical-SMILES lookup |
| `datasets/idx2template_retro.json` | RetroPredictor reaction-template dictionary |
| `datasets/model_retro.pt` | RetroPredictor single-step model weights |
| `datasets/scscore/model.ckpt-10654.as_numpy.json.gz` | SCScore model weights |

**Target sets (evaluation):**

| File | Dataset key |
|------|-------------|
| `datasets/uspto_190_targets.txt` | `uspto-190` |
| `datasets/chembl_1000.pkl` | `chembl-1000` |

**Training / eval parquets and route sources (needed to (re)build training data):**

| File | Purpose |
|------|---------|
| `datasets/train_routes/routes_train.pkl`, `routes_val.pkl` | USPTO-Full reference routes used to sample training targets |
| `datasets/slime_retro_train.parquet` | Prompt parquet consumed by training |
| `datasets/slime_retro_eval_uspto190.parquet` | In-training eval prompts |

### Building the training parquet

`prepare_data.py` samples target molecules from the USPTO-Full route dataset, bakes the system + user prompt (with tool schemas resolved via `apply_chat_template`) into each row, and writes a parquet. It fetches tool schemas over HTTP, so the tool server must already be running (see [Tool Server](#tool-server)).

```bash
# Training data: routes with depth > 4 (as in the paper) -> datasets/slime_retro_train.parquet
python -m slime_retro.prepare_data depth_min=5 depth_max=30

# In-training eval prompts
python -m slime_retro.prepare_data output=datasets/slime_retro_eval_uspto190.parquet targets=uspto-190
```

Targets that appear in any evaluation set (`uspto-190`, `chembl-1000`) are automatically excluded from the training pool. Each parquet embeds its `prepare_data_config` in the schema metadata for provenance.

---

## Tool Server

The tool server (`tools/tool_server.py`) launches `num_instances` FastAPI/uvicorn processes on consecutive ports from `base_port`. Each process loads its own copy of the building-block checker, templates, `RetroPredictor`, and SCScore model, and serves per-session `GraphMemory`. Clients assign new sessions round-robin across instances, then pin each session to its instance (session affinity).

```bash
# Launch 16 instances on ports 8100-8115 (run on the HOST, outside Docker)
python -m tools.tool_server base_port=8100 num_instances=16

# Single instance for local development
python -m tools.tool_server base_port=8100

# Health / inspection
curl http://127.0.0.1:8100/metrics
curl http://127.0.0.1:8100/tool_schemas
```

Training and evaluation reach the server via `RETRO_TOOL_SERVER_PORT` (default `8100`) and `RETRO_TOOL_SERVER_INSTANCES` (default `16`); `num_instances` must be at least `RETRO_TOOL_SERVER_INSTANCES`.

### Tools exposed to the agent

The agent interacts with the environment through 8 tools: 4 memory tools that query and modify the AND-OR graph, and 4 chemistry tools that provide heuristic guidance.

| Tool | Description |
|------|-------------|
| `predict_templates(molecule_id)` | ML single-step candidates `P1..Pk`; cycle-causing and duplicate predictions are pre-filtered, results cached per molecule |
| `expand(molecule_id, prediction_id)` | Apply prediction(s) as a reaction node; comma-separated IDs allowed (`"P1,P3"`) |
| `get_frontier()` | List `OPEN` molecules still needing decomposition; also signals when the root is solved |
| `inspect_node(node_id)` | Node status, parents, and children |
| `get_molecule_info(smiles)` | Molecular weight, logP, atom / ring counts |
| `search_building_blocks(smiles)` | Commercial-availability check (exact canonical match) |
| `calculate_synthetic_accessibility(smiles)` | SA score (1-10) |
| `calculate_scscore(smiles)` | SCScore synthetic complexity (1-5) |

Route completion is detected automatically: the server sets `route_solved` on the tool result when the root molecule becomes `SOLVED`. There is no explicit "finish" tool.

---

## Training

### One-time checkpoint conversion (HF -> Megatron)

slime trains from a Megatron `torch_dist` checkpoint. Convert the HuggingFace weights once:

```bash
# Downloads Qwen/Qwen3-4B-Instruct-2507 to models/<name> and writes models/<name>_torch_dist
bash slime_retro/scripts/convert_chkpt.sh
```

### Launch training

The single entry point is `scripts/train.sh`. It runs preflight checks (Docker up, tool server reachable, training parquet and checkpoint present), pins the released defaults (Qwen3-4B, GSPO, KL 0.001, reward penalties), and then execs `slime_retro/scripts/train.sh`.

`slime_retro/scripts/train.sh` runs a single `docker run` (`slimerl/slime:v0.3.0`, `--network=host`, repo bind-mounted at the same path) that execs the per-model script `slime_retro/scripts/models/<MODEL>.sh`, which sets architecture / parallelism / SGLang options and then `source`s `common.sh`. `common.sh` assembles all slime/Megatron arguments, starts a Ray head node, and submits `python3 /root/slime/train.py` with RetroAgent's custom rollout generator (`slime_retro.generate.generate`) and reward (`slime_retro.generate.reward_func`).

Before launching, make sure (1) the tool server is up on `:8100`, (2) `datasets/slime_retro_train.parquet` exists (see [Data Preparation](#data-preparation)), and (3) `models/<name>_torch_dist` exists (from `convert_chkpt.sh`).

```bash
# Default: Qwen3-4B-Instruct-2507, GSPO
bash scripts/train.sh

# Switch algorithm to GRPO (GSPO is the default)
ADV_ESTIMATOR=grpo bash scripts/train.sh

# Turn off off-policy TIS and dynamic sampling
OFF_POLICY=0 DYNAMIC_SAMPLING=0 bash scripts/train.sh

# Sampling-based in-training eval (Qwen3 recommends T=0.6 over greedy)
EVAL_TEMPERATURE=0.6 EVAL_TOP_K=20 bash scripts/train.sh
```

Checkpoints are saved to `checkpoints/slime_retro_<model_short>/iter_NNNNNNN`, with in-training eval on USPTO-190. To export a checkpoint back to HuggingFace format (for release or SGLang serving):

```bash
bash slime_retro/scripts/export_hf.sh --checkpoint-dir=checkpoints/slime_retro_.../iter_0000179
```

### GPU layout and hyperparameters

The released config targets a single 8xH200 node in colocated mode (SGLang inference and Megatron training share all 8 GPUs):

| Setting | Value |
|---------|-------|
| Actor GPUs / mode | 8, colocated (`COLOCATE=1`) |
| Tensor parallel | 2 (+ sequence-parallel; PP/CP/EP = 1) |
| SGLang engines | 1 per 2 GPUs; `mem-fraction-static 0.8`, context length 65536 |
| Rollout batch / group | 32 prompts x 8 samples |
| Off-policy TIS | on by default (2 steps/rollout; global batch 128) |
| Dynamic sampling | on by default (DAPO-style, over-sampling 48, drops zero-reward-std groups) |
| Optimizer | Adam, lr `1e-6` constant, weight decay `0.01`, betas `(0.9, 0.98)` |
| KL | `--use-kl-loss --kl-loss-coef 0.001 --kl-loss-type low_var_kl` |
| GSPO clip | `--eps-clip 3.5e-4` (sequence-level) |

For a host with fewer than 8 GPUs, set `ACTOR_NUM_GPUS` (the Ray head and Megatron args pick it up automatically).

### Reward function

For a rollout, with `N` = number of `predict_templates` calls and `D` = deepest molecule-node depth:

- **Route not completed:** `-1.5` if the trajectory produced zero valid tool calls (pure overthinking / format failure), otherwise `-1.0`.
- **Route completed:**

  ```
  reward = clip(-1, 1,  1.0
                       - delta_N        # budget penalty on single-step model calls
                       - delta_D        # route-depth penalty
                       + tool_penalty_sum)
  delta_N = min(0.3, 0.02 * max(0, N - 20))   # penalty-free up to 20 calls, then 0.02/call, cap 0.3
  delta_D = min(0.3, 0.02 * max(0, D - 20))   # penalty-free up to depth 20, then 0.02/step, cap 0.3
  ```

  `tool_penalty_sum` accumulates `-0.05` per turn with a failed tool call and `-0.05` per turn with no parseable tool call.

### Key `RETRO_*` environment variables

All are read in `slime_retro/config.py` and injected into the training container / Ray workers by `train.sh` and `common.sh`.

| Variable | Default | Meaning |
|----------|---------|---------|
| `RETRO_MAX_TOTAL_TOKENS` | `56000` | Per-episode token budget (must be < SGLang context 65536 minus max response 8192) |
| `RETRO_PREDICT_TEMPLATES_TOP_K` | `20` | Candidate reactions returned per `predict_templates` |
| `RETRO_EXPAND_MODE` | `multi` | `single` (one prediction id) or `multi` (comma-separated ids); ablated in the paper (RQ3) |
| `RETRO_REWARD_DEPTH_THRESHOLD` / `_RATE` / `_CAP` | `20` / `0.02` / `0.3` | Route-depth penalty knobs |
| `RETRO_REWARD_TOKEN_THRESHOLD` / `_RATE` / `_CAP` | `999999` / `0.0` / `0.0` | Token-length penalty (off by default) |
| `RETRO_TOOL_SERVER_PORT` | `8100` | Host tool-server base port |
| `RETRO_TOOL_SERVER_INSTANCES` | `16` | Number of tool-server instances |

Fixed (non-env) episode limits: `MAX_TURNS=90`, `MAX_CONSECUTIVE_FAILURES=5`, `MAX_ROUTE_DEPTH=30`.

Training-script overrides (passed as env to `train.sh`): `MODEL`, `ADV_ESTIMATOR` (`gspo` / `grpo`), `OFF_POLICY`, `DYNAMIC_SAMPLING`, `COLOCATE`, `ACTOR_NUM_GPUS`, `ROLLOUT_NUM_GPUS`, `KL_COEF`, `ENTROPY_COEF`, `EVAL_TEMPERATURE`, `EVAL_TOP_K`.

---

## Evaluation

### Iterative planning protocol

Evaluation mirrors the training rollout exactly (same tool server, same `<tool_call>` parsing, no context pruning). For each `(target, experiment)`, rollouts are run sequentially until a complete route is found or a search budget is exhausted:

- **Budget** = the number of unique molecule SMILES on which `predict_templates` was called across rollouts. Molecules cached from earlier rollouts cost 0, so the budget counts only new single-step calls.
- **`num_exp`** repeats the whole iterative process per target; every metric is reported as mean +/- std across experiments.
- Two consecutive rollouts that add zero new budget short-circuit a target.

Following Retro-R1, each run reports two metrics (both written to `summary.json`):

- **Pass@1** — fraction of targets solved in a *single trajectory* (solved on the first rollout). This is computed automatically on every run; it does not require `max_rollouts=1`.
- **Success rate at budget N** — fraction of targets solved within `N` unique single-step model calls, at the milestones `50,100,200,300,400,500` (configurable via `milestones=`).

Setting `max_rollouts=1` restricts the search itself to a single trajectory per experiment (so the budget curve collapses onto Pass@1); leave it unset (budget-limited) to also get the full budget curve.

All evaluation runs through a single module, `slime_retro.eval`. It accepts one or more SGLang URLs, feeds every `(target, experiment)` work item into one shared queue that load-balances across replicas, and persists per-target results to `<log_dir>/results/expN_tgtNNN.json` for resume. A single URL is a one-replica pool.

### Serve the checkpoint

```bash
# Serve the trained checkpoint on SGLang (Docker). 8xH200 example: 4 replicas, TP=2.
# Point --model-path at your exported HF checkpoint directory.
docker run --rm --gpus '"device=0,1"' --network=host --ipc=host --shm-size=16g \
  -v "$PWD:$PWD" -w "$PWD" slimerl/slime:v0.3.0 \
  python3 -m sglang.launch_server --model-path /path/to/retroagent_hf --tp 2 \
  --mem-fraction-static 0.85 --context-length 131072 --port 30000 --host 0.0.0.0 --trust-remote-code
# Repeat on device=2,3 -> port 30001 ; device=4,5 -> 30002 ; device=6,7 -> 30003

# Start the tool server on the host
python -m tools.tool_server base_port=8100 num_instances=16
```

### Run the evaluation

The one-command entry point is `scripts/eval.sh` (preflight-checks the tool server + SGLang, then runs `slime_retro.eval` with the reported protocol: budget 500, `num_exp=10`, greedy decoding at temperature 0):

```bash
# USPTO-190 (in-distribution)
bash scripts/eval.sh

# ChEMBL-1000 (out-of-distribution)
TARGETS=chembl-1000 bash scripts/eval.sh

# pass@1 (single rollout per experiment)
bash scripts/eval.sh max_rollouts=1

# multi-replica pool across 4 served ports
SGLANG_URL=http://localhost:30000,http://localhost:30001,http://localhost:30002,http://localhost:30003 bash scripts/eval.sh
```

Or call the module directly for full control:

```bash
python -m slime_retro.eval \
  sglang_url=http://localhost:30000 \
  targets=uspto-190 max_budget=500 num_exp=10 temperature=0 \
  log_dir=logs/eval/uspto190_trained
```

Sampling defaults: `temperature=0` (greedy; use 0.6 only for the untrained zero-shot baseline), `top_k=20`, `top_p=1.0`, `max_response_len=8192`, `max_total_tokens=120000`, `max_turns=120`. Results are written to `<log_dir>/summary.json` (milestone mean/std) and per-target `results/` files for resume. Each rollout also produces `route.json` (the solved route + diagnostics) and `transcript.json` under `<log_dir>/eval_<exp>/target_<idx>/rollout_<idx>/`. To visualize a solved route:

```bash
python -m analyzers.visualize path/to/route.json --fmt mermaid   # writes route.mmd
python -m analyzers.visualize path/to/route.json --fmt image     # RDKit grid PNG (needs rdkit)
```

---

## Zero-shot and Inference with Released Weights

Zero-shot uses the exact same harness as the trained model. The only difference is which weights SGLang serves; the eval scripts never assume a trained checkpoint. This is how the paper's zero-shot numbers were produced (not a separate API-based harness).

```bash
# Serve the stock (untrained) model on SGLang
docker run --rm --gpus '"device=0,1"' --network=host --ipc=host --shm-size=16g \
  -v "$PWD:$PWD" -w "$PWD" slimerl/slime:v0.3.0 \
  python3 -m sglang.launch_server --model-path Qwen/Qwen3-4B-Instruct-2507 --tp 2 \
  --mem-fraction-static 0.85 --context-length 131072 --port 30000 --host 0.0.0.0 --trust-remote-code

# Same tool server, same eval command; just point at the untrained model
python -m tools.tool_server base_port=8100 num_instances=16   # (host)

python -m slime_retro.eval sglang_url=http://localhost:30000 \
  targets=uspto-190 max_budget=500 num_exp=10 temperature=0.6 \
  model_name=Qwen/Qwen3-4B-Instruct-2507 log_dir=logs/eval/uspto190_zeroshot
```

To run the trained RetroAgent, serve `SXKDZ/RetroAgent` instead of `Qwen/Qwen3-4B-Instruct-2507` and drop the `model_name` override (it defaults to the Qwen3-4B tokenizer, which is compatible). Solved routes, per-target search budgets, and rendered transcripts appear under `<log_dir>/`.

> Note: the iterative eval CLI accepts dataset keys (`uspto-190`, `chembl-1000`) rather than an arbitrary single SMILES. To plan a one-off target, add it to a targets file / dataset key and run with `num_targets=1`.

---

## Repository Structure

| Path | Description |
|------|-------------|
| `scripts/train.sh` | Main training entry point (preflight checks -> `slime_retro/scripts/train.sh`) |
| `scripts/eval.sh` | Main evaluation entry point (preflight checks -> `slime_retro.eval`) |
| `slime_retro/scripts/train.sh` | Docker launcher (`docker run` -> `models/<MODEL>.sh` -> `common.sh` -> `train.py`) |
| `slime_retro/scripts/models/` | Per-model arch/parallelism/SGLang configs + shared `common.sh` |
| `slime_retro/scripts/convert_chkpt.sh`, `export_hf.sh` | HF <-> Megatron `torch_dist` conversion |
| `slime_retro/scripts/setup.sh` | Fresh-host bootstrap (deps, HuggingFace download, Docker image) |
| `slime_retro/generate.py` | Multi-turn rollout generator + `reward_func` (slime custom callbacks) |
| `slime_retro/config.py` | Centralized episode / reward / tool-server config (reads `RETRO_*` env) |
| `slime_retro/utils.py` | Reward computation, tool-call parsing, observation building |
| `slime_retro/prompts.py` | System / user prompt builders |
| `slime_retro/prepare_data.py` | Build the training / eval prompt parquets from USPTO-Full routes |
| `slime_retro/eval.py` | The single iterative-planning eval driver (budget milestones + pass@1, multi-replica pool, resume) |
| `slime_retro/eval_worker.py` | Per-episode eval engine (`eval_single_target`), used by `eval.py` |
| `slime_retro/transcript.py` | Per-episode transcript + `route.json` writer |
| `tools/tool_server.py` | FastAPI HTTP tool server, per-session `GraphMemory`, `/tool_schemas` endpoint |
| `tools/tool_client.py` | Async `ToolClient` (round-robin + session affinity) |
| `tools/memory.py` | AND-OR `GraphMemory` with automatic status propagation |
| `tools/ml_retro.py` | Single-step template prediction via `RetroPredictor` |
| `tools/search.py` | Building-block search and molecule info |
| `tools/heuristics.py`, `tools/scscore.py` | SA score, SCScore, complexity heuristics |
| `tools/tools.py` | `Tool` wrapper; auto-generates OpenAI tool schemas from type hints + docstrings |
| `data/loaders.py` | Load building blocks, target sets, and USPTO-Full reference routes |
| `data/building_blocks.py` | `BuildingBlockChecker` (FAISS + exact canonical-SMILES lookup) |
| `utils/smiles.py` | SMILES canonicalization |
| `analyzers/route.py` | Route analysis + `save_route_json` (solved route + diagnostics) |
| `analyzers/visualize.py` | Standalone, off-pipeline route visualizer (`route.json` -> Mermaid/image) |
| `external/desp/` | Vendored `RetroPredictor` single-step retrosynthesis model |

---

## Citation and License

If you use RetroAgent, please cite:

```bibtex
@inproceedings{zhu2026retroagent,
  title     = {RetroAgent: Harnessing {LLMs} to Search Over Structured Memory for Agentic Retrosynthesis Planning},
  author    = {Zhu, Yanqiao and Gan, Jingru and Sun, Xiaoqi and Sun, Fang and Shi, Yidan and Islam, Md Mofijul and Shang, Chao and Gao, Wenhao and Coley, Connor W. and Sun, Yizhou and Wang, Wei},
  booktitle = {Conference on Language Modeling (COLM)},
  year      = {2026}
}
```

### License

See [`LICENSE`](LICENSE) for the RetroAgent code license (MIT). The single-step retrosynthesis model vendored under `external/desp/` is derived from its upstream project and remains subject to that project's original license and attribution (see [`NOTICE`](NOTICE)). The building-block and reaction-template data, released via [SXKDZ/RetroAgent](https://huggingface.co/SXKDZ/RetroAgent), are distributed under the terms stated on the dataset card; please review those terms before redistribution.
