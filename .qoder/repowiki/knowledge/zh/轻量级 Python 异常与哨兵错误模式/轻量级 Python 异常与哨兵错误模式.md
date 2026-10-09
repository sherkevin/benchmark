---
kind: error_handling
name: 轻量级 Python 异常与哨兵错误模式
category: error_handling
scope:
    - '**'
source_files:
    - autonomy_mvp/core/judge.py
    - autonomy_mvp/core/sim_agents.py
    - autonomy_mvp/core/tasks.py
    - deep_research_longhorizon/download_samples.py
    - deep_research_longhorizon/datasets/TheAgentCompany/sample_tasks/bm-classify-nationality/evaluator.py
    - deep_research_longhorizon/datasets/TheAgentCompany/sample_tasks/ds-answer-numerical-data-question/evaluator.py
---

本仓库未建立统一的错误处理框架或中间件，而是采用 Python 标准库的内置异常作为主要错误表达手段，并在关键路径上以 `raise ValueError` / `KeyError` / `RuntimeError` 等哨兵式抛出配合局部 `try/except Exception` 捕获的方式实现。核心特征如下：

1. **无自定义 Error 类型**：全仓未发现 `class *Error(Exception)` 形式的统一错误类定义；所有业务异常均直接 raise 内置异常。
2. **裁判与模拟器的“未知 kind”哨兵**：在 `autonomy_mvp/core/judge.py` 与 `sim_agents.py` 中，对 `stage.kind` 不在 `{ranking, value_set, selection}` 的情况直接 `raise ValueError(...)`，作为配置错误的快速失败信号。
3. **任务规格查找缺失键**：`tasks.TaskSpec.stage(name)` 找不到阶段时 `raise KeyError(name)`，沿用 dict 语义。
4. **脚本层 I/O 错误包装**：`deep_research_longhorizon/download_samples.py` 用 `raise RuntimeError(f"curl exit {p.returncode} for {url")` 把子进程退出码包装为运行时错误，其余网络/文件读取处使用 `try/except Exception as e` 做最宽泛兜底并记录日志。
5. **评测器样例的 try/except 包裹**：`TheAgentCompany/sample_tasks/*/evaluator.py` 中对每个解析步骤用 `try: ... except Exception as e:` 包裹，保证单条样本出错不中断整批评估。
6. **无 panic/recover、无中间件**：仓库为纯 Python 脚本/基准内核，不存在 Web 中间件、协程 recover 或类似机制。

开发者约定（从现有用法归纳）：
- 对“不可恢复的配置/契约违反”（如未知 stage.kind、缺失 stage name）应 raise 具体内置异常（ValueError/KeyError），让上层快速失败。
- 对外部依赖（HTTP、文件系统）的错误用 `try/except Exception` 包裹并记录，避免单点故障污染整批运行。
- 不要引入新的自定义 Error 基类——当前风格是“最小化抽象 + 内置异常直抛”，新增模块应保持一致。