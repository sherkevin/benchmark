---
configs:
- config_name: Expert
  data_files:
  - split: all_examples
    path: tasks/expert.jsonl
- config_name: Auto
  data_files:
  - split: all_examples
    path: tasks/auto_gen.jsonl
- config_name: Masked
  data_files:
  - split: all_examples
    path: tasks/masked.jsonl
language:
- en
pretty_name: >-
  SUPER: Evaluating Agents on Setting Up and Executing Tasks from Research
  Repositories
license: apache-2.0
size_categories:
- 1K<n<10K
---

Paper: https://huggingface.co/papers/2409.07440
 
See GitHub repository here for details: https://github.com/allenai/super-benchmark