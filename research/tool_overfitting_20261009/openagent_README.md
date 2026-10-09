<p align="center">
  <img src="assets/logo.png" width="180" alt="OpenAgent logo">
</p>

<h1 align="center">OpenAgent</h1>

<h2 align="center">Can Agents Generalize to the Open World?</h2>

<p align="center">
  <strong>Unveiling the Fragility of Static Training in Tool Use</strong>
</p>

<p align="center">
  <a href="https://wuwm109.github.io/"><strong>Weiming Wu*</strong></a> ·
  <a href="https://openreview.net/profile?id=~Song-Lin_Lv1"><strong>Song-Lin Lv*</strong></a> ·
  <a href="https://openreview.net/profile?id=~Rui_Zhu15"><strong>Rui Zhu</strong></a> ·
  <a href="https://openreview.net/profile?id=~Zi-Jian_Cheng1"><strong>Zi-Jian Cheng</strong></a> ·
  <a href="https://www.lamda.nju.edu.cn/guolz/"><strong>Lan-Zhe Guo</strong></a>
</p>

<p align="center">
  Nanjing University · National Key Laboratory for Novel Software Technology
  <br>
  * Equal contribution
</p>

<p align="center">
  <img alt="Paper coming soon" src="https://img.shields.io/badge/Paper-Coming%20Soon-b31b1b?style=for-the-badge">
  <a href="https://wuwm109.github.io/OpenAgent-Page/">
    <img alt="Project page" src="https://img.shields.io/badge/Project%20Page-OpenAgent-1f77b4?style=for-the-badge&logo=githubpages&logoColor=white">
  </a>
  <a href="https://github.com/LAMDA-NeSy/OpenAgent">
    <img alt="GitHub repository" src="https://img.shields.io/badge/GitHub-Code-24292e?style=for-the-badge&logo=github&logoColor=white">
  </a>
</p>

Official implementation of **OpenAgent**, a controlled sandbox benchmark for testing whether tool-use agents trained in a
static environment can generalize when user queries, tool schemas, observations, and task domains change at inference time.

## 🚀 TL;DR

- **Task:** OpenAgent studies tool-use agents under open-world environment shifts rather than only static train-test matches.
- **Benchmark:** We build controlled sandbox tasks where query wording, tool definitions, observations, and domains can be changed independently.
- **Diagnosis:** The evaluation is organized into four tiers: Perception, Interaction, Reasoning, and Internalization.
- **Code:** This repository includes data generation, sandbox tools, scenario configs, evaluation runners, smoke tests, and reusable scripts.

## 📋 Table of Contents

- [Open Environment Shifts](#-open-environment-shifts)
- [Four-Tier Sandbox](#-four-tier-sandbox)
- [Repository Contents](#-repository-contents)
- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [Dataset Generation](#-dataset-generation)
- [Sandbox Evaluation](#-sandbox-evaluation)
- [Data Format](#-data-format)
- [Citation](#-citation)

## 🔄 Open Environment Shifts

| Shift | What changes at inference time | What it probes |
| --- | --- | --- |
| **Query shift** | Rewritten, ambiguous, underspecified, or redundant user requests | Intent abstraction beyond surface phrasing |
| **Action shift** | Renamed tools, noisy schemas, distractor tools, and changed verification codes | Tool grounding beyond memorized names |
| **Observation shift** | Missing values, non-standard feedback, runtime errors, and tool redirection | Feedback parsing and recovery |
| **Domain shift** | Transfer from the POI sandbox to an unseen medical-domain tool environment | Structural transfer across domains |

## 🧭 Four-Tier Sandbox

![OpenAgent four-tier hierarchy](assets/four_tier.png)

OpenAgent groups the shifts into a reusable four-tier diagnostic hierarchy:

| Tier | Focus | Representative scenarios |
| --- | --- | --- |
| **Perception** | Parse user intent and tool semantics | query rewrite, tool-name noise, semantic renaming, distractor tools |
| **Interaction** | React to environment feedback | missing information, mode changes, tool errors, redirection |
| **Reasoning** | Adapt the execution graph and calculation rules | unavailable tools, altered tool logic |
| **Internalization** | Transfer the learned task structure | domain transfer to medical tools |

## 🧩 Repository Contents

| Module | Purpose |
| --- | --- |
| `datagen/` | Build POI tool-use training and test trajectories |
| `eval/` | Run configurable four-tier open-world evaluation scenarios |
| `tools/` | Execute POI and medical-domain sandbox tools |
| `examples/` | Provide a tiny local CSV for smoke checks |
| `scripts/` | Provide simple command-line entry points |
| `tests/` | Run model-free checks for the data and tool pipeline |

## ⚙️ Installation

```bash
git clone https://github.com/LAMDA-NeSy/OpenAgent.git
cd OpenAgent

python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

On Windows PowerShell, activate the environment with:

```powershell
.\.venv\Scripts\Activate.ps1
```

If you want to use LLM-based query rewriting during data generation:

```bash
cp .env.example .env
# Fill OPENAI_API_KEY, OPENAI_BASE_URL, and REWRITE_MODELS if needed.
```

## ⚡ Quick Start

Run the smoke test first. It generates a small dataset from `examples/sample_database.csv` and checks that the sandbox
tools can execute the generated calls.

```bash
python tests/smoke_test.py
```

Generate a demo test set:

```bash
bash scripts/generate_sample_data.sh
```

The output is written to:

```text
outputs/demo/demo_test.json
```

## 🧪 Dataset Generation

Generate test data from a POI CSV:

```bash
python datagen/generate.py \
  --split test \
  --csv data/database_test_en.csv \
  --output data/testset/baseline.json \
  --samples 50 \
  --single-samples 20 \
  --seed 7
```

Generate training data with optional LLM rewriting:

```bash
python datagen/generate.py \
  --split train \
  --csv data/database_en.csv \
  --output data/train.json \
  --samples 800 \
  --rewrite
```

Expected CSV columns:

```text
name,location,type,adname,tel,cost1,cost2
```

`location` should use `longitude,latitude` format. Telephone numbers are normalized to eight digits.

## 🛠️ Sandbox Evaluation

Run one scenario:

```bash
python eval/run_eval.py \
  --model_path /path/to/model \
  --scenario layer1_name_noise \
  --variant 1 \
  --data_root data \
  --output_root outputs
```

Run multiple scenarios across GPUs:

```bash
python eval/run_all.py \
  --models /path/to/model1 /path/to/model2 \
  --scenarios layer1_name_noise layer2_tool_error layer3_error_tool layer4_domain_medical \
  --variants 1 2 3 \
  --data_root data \
  --output_root outputs \
  --gpu_ids 0 1 2 3 \
  --min_memory_mb 50000
```

Available scenario configs live in `eval/configs/`:

| Config | Tier | Shift |
| --- | --- | --- |
| `layer1_name_noise` | Perception | Syntactic tool-name noise |
| `layer1_name_semantic` | Perception | Semantic tool-name paraphrase |
| `layer1_name_wo_semantic` | Perception | Non-semantic tool rename |
| `layer1_query_rewrite` | Perception | User-query rewrite |
| `layer1_add_related` | Perception | Related tool distractors |
| `layer1_mode_change` | Perception | Verification-code change |
| `layer2_information_loss` | Interaction | Missing values |
| `layer2_mode_change` | Interaction | Parameter-mode change |
| `layer2_query_condition` | Interaction | Query-condition change |
| `layer2_tool_error` | Interaction | Tool execution error |
| `layer3_error_tool` | Reasoning | Required tool unavailable |
| `layer4_domain_medical` | Internalization | Domain transfer |

Outputs are saved as:

```text
outputs/
  results/<scenario>/<model>_<scenario>_<variant>.json
  metrics/<scenario>/<model>_<scenario>_<variant>.json
```

## 📦 Data Format

Each generated example has this structure:

```json
{
  "id": 0,
  "source": "distance",
  "conversations": [
    {"from": "human", "value": "Please calculate the distance ..."},
    {"from": "gpt", "value": "<tool_call>{\"tool\": \"search_map_coordinates\", \"args\": {\"name\": \"alpha_cafe\"}}</tool_call>"},
    {"from": "observation", "value": "{\"location\": \"116.300100,39.900100\"}"},
    {"from": "gpt", "value": "<answer>123.4</answer>"}
  ],
  "tools": "[...]",
  "gt_tools": ["search_map_coordinates"],
  "ground_truth": "123.4",
  "meta": {"total_steps": 2}
}
```

## 📝 Citation

```bibtex
@inproceedings{wu2026openagent,
  title     = {Can Agents Generalize to the Open World? Unveiling the Fragility of Static Training in Tool Use},
  author    = {Wu, Weiming and Lv, Song-Lin and Zhu, Rui and Cheng, Zi-Jian and Guo, Lan-Zhe},
  booktitle = {Proceedings of the 43rd International Conference on Machine Learning},
  year      = {2026}
}
```
