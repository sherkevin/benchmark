---
kind: dependency_management
name: 多工具并行的 Python 依赖管理（Conda + uv + requirements.txt）
category: dependency_management
scope:
    - '**'
source_files:
    - docs/prd/molbench_repo/environment.yaml
    - task7/poc3_physicianbench/PhysicianBench/pyproject.toml
    - task7/poc3_physicianbench/PhysicianBench/uv.lock
    - task7/poc4_pharmabench/pyproject.toml
    - task7/poc1_do_challenge/do-events-challenge2025/server/requirements.txt
---

本仓库没有统一的顶层依赖声明，而是按子项目/POC 各自采用不同的 Python 包管理方案，形成「多工具并行」的碎片化格局：

- **MolClaw PRD 环境** (`docs/prd/molbench_repo/environment.yaml`)：使用 Conda `environment.yaml` 锁定完整运行环境（含 CUDA、OpenMM、PyTorch 等科学计算栈），并通过 `conda env create -f environment.yaml` 创建名为 `molclaw` 的环境。该文件同时声明了 conda 通道与 pip 子依赖，是仓库中唯一一份完整的可复现环境清单。
- **PhysicianBench POC** (`task7/poc3_physicianbench/PhysicianBench/pyproject.toml` + `uv.lock`)：以 pyproject 声明最小依赖（openai、requests、python-dotenv、pytest>=8），并用 uv 生成带 SHA256 校验的 `uv.lock` 锁文件，构建后端为 setuptools。
- **PharmaBench POC** (`task7/poc4_pharmabench/pyproject.toml`)：仅用 pyproject 声明 rdkit>=2024.3.5、openai>=1.0，未附带锁文件。
- **DO Challenge 服务** (`task7/poc1_do_challenge/do-events-challenge2025/server/requirements.txt`)：Flask 微服务使用固定版本的 `requirements.txt`，并由 Dockerfile 通过 `pip install -r requirements.txt` 安装。
- **下游数据集子仓库**：`deep_research_longhorizon/datasets/*` 中的多个第三方 benchmark 子目录各自保留其原始依赖方式（poetry、requirements.txt 等），本仓库仅聚合数据不接管其依赖。

**关键约定与现状：**
- 无顶层 `pyproject.toml` / `go.mod` / `package.json`，不存在跨模块共享的虚拟环境或 vendoring 策略。
- 各 POC 之间依赖版本互不协调（例如 PhysicianBench 与 PharmaBench 都依赖 openai 但约束不同），存在潜在的冲突风险。
- 未见私有 PyPI 源、GOPRIVATE、npm registry 等配置；所有包均来自官方 PyPI。
- 无自动化更新脚本或 CI 任务来同步 lock 文件，依赖更新由人工在各子项目中分别进行。

**开发者应遵循的规则：**
1. 新增 POC/子项目时，优先在该项目根目录放置 `pyproject.toml` 并配合 `uv lock` 生成 `uv.lock`，以保持与 PhysicianBench 模式一致。
2. 若涉及 CUDA/OpenMM 等系统级依赖，改用 `environment.yaml` 并在 README 中说明 `conda env create -f environment.yaml` 的安装步骤。
3. 避免在多个子项目中对同一库使用不一致的版本约束；如需共享依赖，考虑在顶层引入单一 `pyproject.toml` workspace 或使用 Conda 环境隔离。
4. 不要将 `.venv/`、`__pycache__/`、`*.egg-info/` 提交到仓库（已有 .gitignore 覆盖）。