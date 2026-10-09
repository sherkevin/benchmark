---
kind: logging_system
name: 基于标准库 logging 的分散式日志系统
category: logging_system
scope:
    - '**'
source_files:
    - task7/poc1_do_challenge/do-events-challenge2025/server/app/config/core/logger.py
    - task7/poc3_physicianbench/PhysicianBench/agent/llm_client.py
    - task7/poc3_physicianbench/PhysicianBench/agent/mini_agent.py
    - task7/poc3_physicianbench/PhysicianBench/tools/fhir_api_functions.py
    - docs/prd/molbench_repo/environment.yaml
---

本仓库未实现统一的日志框架或集中式日志配置，各子项目各自使用 Python 标准库 `logging` 模块进行散点式记录，缺乏跨模块共享的 logger 初始化、统一格式与分级策略。

**已发现的使用模式：**
- **DO Challenge POC 服务** (`task7/poc1_do_challenge/do-events-challenge2025/server/app/config/core/logger.py`)：唯一一处显式配置了 RotatingFileHandler，将 INFO 写入 `logs/app.log`、ERROR 写入 `logs/error.log`，并附带控制台输出；但其他模块直接 `import logging; logger = logging.getLogger(__name__)` 后自行调用，并未引用该配置文件，导致实际运行时多数模块仍走默认 root logger。
- **PhysicianBench POC** (`task7/poc3_physicianbench/PhysicianBench/agent/*.py`)：每个文件独立 `logger = logging.getLogger(__name__)`，仅做 `info/warning/error/debug` 调用，无任何 handler 配置，依赖全局 `logging.basicConfig`（在 `usage/ai_agent.py` 中设置 level=INFO）生效。
- **TheAgentCompany 样本任务 evaluator**：同样以 `import logging` + `getLogger(__name__)` 方式散用，无集中配置。
- **MolClaw 环境声明** (`docs/prd/molbench_repo/environment.yaml`) 中声明了 `loguru==0.7.3`，但该依赖未被任何代码实际引入，属于遗留或未迁移状态。

**结构化字段与级别约定：**
- 日志消息多为纯字符串拼接，未见 JSON 序列化或固定字段 schema；仅在 DO Challenge 的 `db.py` 中出现一次 `logger.info({'message': ...})` 的字典传参，但未配合 JSON formatter，实际输出仍是格式化后的字符串。
- 日志级别使用随意：连接信息、重试提示混用 `info`/`warning`/`error`，没有按模块或子系统划分的级别策略。

**结论：** 仓库不存在统一的 logging_system。日志是各 POC 子项目的“各自为政”式标准库 `logging` 用法，缺少中央初始化、统一 sink、结构化字段规范与级别治理，不具备可移植性与可观测性工程化能力。