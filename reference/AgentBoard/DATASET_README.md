---
license: gpl-2.0
configs:
- config_name: alfworld
  data_files:
  - split: test
    path:
    - data/alfworld/test.jsonl
- config_name: scienceworld
  data_files:
  - split: test
    path:
    - data/scienceworld/test.jsonl
- config_name: babyai
  data_files:
  - split: test
    path:
    - data/babyai/test.jsonl
- config_name: jericho
  data_files:
  - split: test
    path:
    - data/jericho/test.jsonl
- config_name: pddl
  data_files:
  - split: test
    path:
    - data/pddl/test.jsonl
- config_name: webarena
  data_files:
  - split: test
    path:
    - data/webarena/test.jsonl
- config_name: webshop
  data_files:
  - split: test
    path:
    - data/webshop/test.jsonl
- config_name: tool-query
  data_files:
  - split: test
    path:
    - data/tool-query/test.jsonl
- config_name: tool-operation
  data_files:
  - split: test
    path:
    - data/tool-operation/test.jsonl
language:
- en
tags:
- Embodied AI
- Game
- Web
- Tool
size_categories:
- 1K<n<10K
task_categories:
- text-generation
pretty_name: AgentBoard
---


<div align="center">
<img src="./assets/agentboard.png" style="width: 20%;height: 10%">
<h1> AgentBoard: An Analytical Evaluation Board of Multi-turn LLM Agents </h1>
</div>

This is the official dataset repository of [AgentBoard](https://github.com/hkust-nlp/agentboard). 

## 1. Data Overview
AgentBoard is composed of 9 diverse tasks which can be divided into 4 types, including **Embodied AI**, **Game**, **Web**, and **Tool**:


<table align="center">
  <tbody>
    <tr align="center" valign="bottom">
      <td>
        <b>Embodied AI</b>
      </td>
      <td>
        <b>Game</b>
      </td>
      <td>
        <b>Web</b>
      </td>
      <td>
        <b>Tool</b>
      </td>
    </tr>
    <tr valign="top">
<td>

- AlfWorld
- ScienceWorld
- BabyAI

</td>

<td>

- Jericho
- PDDL

</td>

<td>

- WebShop
- WebArena

</td>

<td>

- Tool-Query
- Tool-Operation

</td>

  </tr>
  </tbody>
</table>


And statistics of the evaluation data of 9 environments are as follows:

| | AlfWorld | ScienceWorld | BabyAI | Jericho | PDDL | WebShop | WebArena | Tool-Query | Tool-Operation |
|-------|----------|--------------|--------|---------|------|---------|----------|------------|----------------|
| **\#Environment** | 134 | 90 | 112 | 20 | 60 | 251 | 245 | 60 | 40 |
| **\#Turn** | 6 | 15 | 10 | 20 | 20 | 3 | 25 | 5 | 6 |
| **\#Action Space** | 13 | 21 | 8 | 150 | 8 | 2 | 12 | 15 | 16 |
| **\#Context Length** | 900 | 2800 | 1800 | 1500 | 2700 | 1200 | 15000 | 2100 | 4300 |
| **Progress Rate** | subgoal | subgoal | subgoal | subgoal | match | match | match | subgoal | subgoal/match |
| **\#Avg. Subgoals** | 3 | 5 | 4 | 6 | 6 | 4 | 6 | 5 | 5 |
| **Hard/Easy Cutoff** | 3 | 3 | 3 | 4 | 6 | 1 | 4 | 4 | 4 |

To help researchers quickly understand evaluation data of each task, we provide **Dataset Viewer** at Huggingface Dataset: [🤗 AgentBoard](https://huggingface.co/datasets/hkust-nlp/agentboard/).

> Note: Please download the dataset from the link provided below for the reason that the data in Dataset Viewer is not complete.

## 2. Download Link
You can download the whole evaluation data by running the following command:
```shell
wget https://huggingface.co/datasets/hkust-nlp/agentboard/resolve/main/data.tar.gz
```
Please uncommpress the file and move the data to `AgentBoard/data`.
```shell
cd AgentBoard
mkdir data
tar -zxvf data.tar.gz
```

The file structure of evaluation data is as follows:
<details>
<summary>
Click to expand the file structure
</summary>

```
data
├── alfworld
│   ├── alfred.pddl # additional data for alfworld
│   ├── alfred.twl2 # additional data for alfworld
│   ├── json_2.1.1  # additional data for alfworld
│   └── test.jsonl
├── babyai
│   └── test.jsonl
├── jericho
│   ├── test.jsonl
│   └── z-machine-games-master  # additional data for jericho
├── pddl
│   └── test.jsonl
├── scienceworld
│   └── test.jsonl
├── tool-operation
│   └── test.jsonl
├── tool-query
│   ├── academia  # additional data for academia tool
│   └── test.jsonl
├── webarena
│   └── test.jsonl
└── webshop
    └── test.jsonl
```

</details>

## 3. Data Fields
We take an instance from the `ScienceWorld` task as an example to illustrate the data fields of evaluation data.
```json
{
  "task": "scienceworld",
  "id": 0,
  "goal": "Your task is to find the animal with the longest life span.  The animals are in the 'outside' location.  Focus on the animal with the longest life span.",
  "subgoals": ["You move to the outside.", "You focus on the crocodile egg."],
  "difficulty": "easy",
  "additional_info": {"var": 5, "env_name": "lifespan-longest-lived"}
}
```
Details of the data fields are as follows:
| Field Name | Description |
|------------|-------------|
| `task` | The task name of the example, e.g. `alfworld`, `babyai`, `jericho`, `pddl`, `scienceworld`, `tool-operation`, `tool-query`, `webarena`, `webshop`. |
| `id` | The id of the example. |
| `goal` | The goal of the example. |
| `subgoals` | The subgoals of the example. |
| `difficulty` | The difficulty of the example, e.g. `easy`, `hard`. |
| `additional_info` | The additional information of the example, each example has its own additional information. |


## 4. Citation
```bibtex
@misc{ma2024agentboard,
      title={AgentBoard: An Analytical Evaluation Board of Multi-turn LLM Agents}, 
      author={Chang Ma and Junlei Zhang and Zhihao Zhu and Cheng Yang and Yujiu Yang and Yaohui Jin and Zhenzhong Lan and Lingpeng Kong and Junxian He},
      year={2024},
      eprint={2401.13178},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```