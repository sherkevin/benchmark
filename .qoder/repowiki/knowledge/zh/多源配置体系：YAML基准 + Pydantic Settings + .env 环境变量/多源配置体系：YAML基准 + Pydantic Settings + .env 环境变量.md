---
kind: configuration_system
name: 多源配置体系：YAML基准 + Pydantic Settings + .env 环境变量
category: configuration_system
scope:
    - '**'
source_files:
    - docs/prd/molbench_repo/config/README.md
    - docs/prd/molbench_repo/config/baseline_molbench-ms-1.yaml
    - docs/prd/molbench_repo/eval/run_eval_bench.py
    - docs/prd/molbench_repo/env.template
    - task7/poc1_do_challenge/do-events-challenge2025/server/app/config/core/settings.py
    - task7/poc3_physicianbench/PhysicianBench/agent/llm_client.py
---

本仓库没有统一的“应用级”配置框架，而是按子项目各自采用轻量、可复现的配置方式，整体呈现三种互补模式：

1) YAML 基准配置（MolClaw / docs/prd/molbench_repo）
- 位置：docs/prd/molbench_repo/config/*.yaml，每个 benchmark 一个文件（baseline_molbench-ms-{1,2,3}.yaml、chemcot_mo_{edit,opt}.yaml、claude_template.yaml）。
- 结构约定：顶层分 bench / model / settings 三段，分别描述数据集与任务列表、LLM 模型名、seed/迭代次数等运行参数；README 明确“示例中不写 base_url/api_key，凭 OPENAI_API_KEY/OPENAI_BASE_URL 注入”。
- 加载入口：eval/run_eval_bench.py 通过 argparse --cfg 指定 YAML，用 yaml.safe_load 读取后根据 bench.dataset/bench.tasks 分发到具体 evaluator，并对缺失 preds 目录、未知 dataset/task 做严格报错。
- 配套模板：docs/prd/molbench_repo/env.template 集中列出所有 runner 需要的环境变量键（OPENAI_*、NANOBOT_MCP_*、SCP_HUB_API_KEY、OPENCLAW_BIN、BIOMNI_REPO_PATH），作为 .env 的权威清单。

2) Pydantic Settings + .env（task7/poc1_do_challenge/do-events-challenge2025/server）
- 位置：server/app/config/core/settings.py 定义 Settings(BaseSettings)，字段覆盖 SECRET_KEY、CORS、MongoDB URI、DATASETS_PATH、TOKEN 配额等；在模块顶部 load_dotenv()，并通过 @validator 把 MONGO_DATABASE_URI 由 host/user/password 拼装为 MongoDsn。
- 使用方式：各模块直接 from app.config.core import settings 单例导入，测试时通过 pytest fixture 覆写 settings 以注入测试值。
- 变量隔离：server/app/config/variables/.env_test 提供测试环境专用 .env 样例。

3) 运行时 LLM 后端选择（task7/poc3_physicianbench/PhysicianBench）
- 位置：agent/llm_client.py 内置 _BACKENDS 优先级表（OpenRouter > Anthropic > OpenAI），构造 LLMClient 时若未显式传入 api_key/base_url，则按顺序从对应 OPENROUTER_*/ANTHROPIC_*/OPENAI_* 环境变量解析并自动选择后端；任一 key 缺失则抛出 ValueError 提示缺失配置项。
- conftest.py 中通过 LLM_JUDGE_BACKEND 环境变量切换 judge 后端，并在未设置时回退到 OPENAI_API_KEY。

设计决策与约束
- 密钥与环境变量优先：所有示例 YAML 均不包含 secret，凭证一律通过 .env 或进程环境变量注入，避免泄露。
- 一基准一配置：MolClaw 下每个数据集一个 YAML，保证“一份配置即可复现实验”，评估脚本对未知配置做失败而非静默降级。
- 强类型校验：Server 侧统一用 pydantic-settings 声明 Settings，利用 validator 做 URL/DSN 组装与 CORS 字符串→列表转换，减少运行时错误。
- 后端可插拔：PhysicianBench 的 LLMClient 以枚举式 _BACKENDS 表驱动后端选择，新增后端只需追加一行 (name, key_env, url_env, default_url)。
- 环境变量命名规范：OPENAI_* 系列用于 OpenAI 兼容网关，NANOBOT_MCP_*、SCP_HUB_API_KEY、OPENCLAW_BIN、BIOMNI_REPO_PATH 等按功能域前缀分组，见 env.template。

开发者应遵循的规则
- 新增基准：在 docs/prd/molbench_repo/config 下新建同名 YAML，保持 bench/model/settings 三段结构，并把新变量写入 env.template。
- 新增服务端配置：在 server/app/config/core/settings.py 的 Settings 类中添加字段，必要时加 @validator 做格式校验，并在 .env_test 补充默认值。
- 新增 LLM 后端：在 PhysicianBench agent/llm_client.py 的 _BACKENDS 表中追加一行，并确保对应的 *_API_KEY/*_BASE_URL 环境变量被 env.template 收录。
- 永远不要硬编码 API Key/URL：示例代码只允许占位，真实值必须来自 .env 或环境变量。