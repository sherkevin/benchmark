# OdysseyBench: Evaluating LLM Agents on Long-Horizon Complex Office Application Workflows

OdysseyBench is a comprehensive benchmark and evaluation suite for task-oriented agent systems, supporting both the OdysseyBench+ and OdysseyBench-Neo tracks. This project provides tools for task generation, execution, validation, and in-depth evaluation of agent performance, with a focus on memory and retrieval-augmented generation (RAG) capabilities.

## 💼 Preparation

```
git clone https://github.com/microsoft/OdysseyBench.git

git clone https://github.com/zlwang-cs/OfficeBench.git /tmp/OfficeBench

find /tmp/OfficeBench/tasks/ -type d -name testbed -exec bash -c 'dest="OdysseyBench/tasks/${1#*/tasks/}"; mkdir -p "$dest"; cp -r "$1" "$dest/../"' _ {} \;

rm -rf /tmp/OfficeBench
```

## 🛠️ Setup


```
conda create -n odysseybench python=3.10
pip install -r requirements.txt
export OPENAI_API_KEY=OPENAI_KEY
```

---

## 📁 Tasks Directory Structure

- **/tasks/substasks_plus**: Tasks for OdysseyBench+
- **/tasks/chat_histories_plus**: Dialogues for OdysseyBench+

- **/tasks/substasks_neo**: Tasks for OdysseyBench-Neo
- **/tasks/chat_histories_neo**: Dialogues for OdysseyBench-Neo

- **/tasks/outputs/**: Results of task execution

- **/tasks/testbed/**: Files required for task execution

---

## 📊 Evaluation on OdysseyBench

### Configuration

Edit `config/base_config.yaml`:

```yaml
memory:
    mode: "use_rag"         # Options: raw_chat, use_rag, clean
    rag_mode: "summarysession" # summarysession, dialogsession, dialogutterance, summarychunk
    top_k: 5                # Used in 'use_rag' mode
```

- **Long-Context Evaluation:**  
  Set `mode: raw_chat` to include all dialogues in the prompt (ignores `rag_mode` and `top_k`).
- **RAG Evaluation:**  
  - For raw context: set `rag_mode` to `dialoguesession` or `dialogueutterance`.
  - For summary: set `rag_mode` to `summarysession` or `summarychunk`.

---

### Run Evaluations

#### OdysseyBench+

```bash
python run_all.py --tag OdysseyBench_plus
```

#### OdysseyBench-Neo

```bash
python run_all.py --neo --tag OdysseyBench_neo
```

---


## 🚀 Run HomerAgents+

```bash
python run_homeragents_plus.py --loops 5
```

## 🚀 Run HomerAgents-Neo

### 🪄 Generate Synthesized Tasks

```bash
python run_homeragents_neo.py
```


### 🧱 Quality Verification

- **Cross Validation**

Select the intersection of successfully executed tasks by task-description and task-intent + task-instruction:

```bash
python run_all.py --neo_clean --tag test-neo-ground-truth-memory
# Set 'mode' as 'clean' in configs/base_config.yaml

python run_all.py --neo_clean --tag test-neo--task_description
# Set 'Memory' as 'False' in configs/base_config.yaml
```

- **Evaluate Execution Performance**

```bash
sh evaluate_all.sh test-neo-ground-truth-memory o3 True
sh evaluate_all.sh test-neo-task_description o3 True
```


### 🧹 Data Cleaning & Formatting

- **Cross-validation selection:**
  ```bash
  python utils_clean/cross_validation.py
  ```
- **Uniform task format:**
  ```bash
  python utils_clean/clean_tasks.py
  ```
- **Uniform dialogue format:**
  ```bash
  python utils_clean/clean_dialogue.py
  ```

---

## 🏆 Generation Task Evaluation

```bash
python llm-as-a-judge.py
```

---

## 🤝 Contributing

Contributions are welcome! Please open issues or pull requests for improvements or questions.

---

## 📬 Reference

If you found this code useful, please cite the following paper:

```
@article{wang2025odysseybench,
  title={OdysseyBench: Evaluating LLM Agents on Long-Horizon Complex Office Application Workflows},
  author={Wang, Weixuan and Han, Dongge and Diaz, Daniel Madrigal and Xu, Jin and R{\"u}hle, Victor and Rajmohan, Saravan},
  journal={arXiv preprint arXiv:2508.09124},
  year={2025}
}
```

## Acknowledgements
```
This project builds on and incorporates material from
[OfficeBench](https://github.com/zlwang-cs/OfficeBench). See NOTICE.txt
for attribution details.
```
---
