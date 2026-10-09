# 独立事件审计 v2

日期：2026-10-07。软件证据核验；未执行新效果实验。

## 本轮改动

旧版把以下划线开头的键当作内部元数据，导致 `_truth_proxy` 可以绕过 exact allowlist。本轮先增加该 mutation，观察到 **1 failed, 17 passed**，再删除内部字段注入及下划线键过滤。修复后的全部事件键都参加 allowlist 和 canonical digest 检查。旧报告和原始实验日志保留。

同时补上反馈 reward 与 terminal correctness 的绑定、0/1 的严格整数类型、terminal/decision tick 绑定、真实 decision 顺序、live release 位置和到期时钟、末尾队列排空顺序。当前封存 schema 只接受 `provider_status="ok"` 的已评分结果，不能用它宣称运行失败、unknown 或 refusal 的能力覆盖。

## 结果

| 验证 | 数量 | 结果 |
|---|---:|---|
| 单测 | 1 个 clean smoke + 17 个 mutation | 18 passed |
| 指定真实 smoke | 8 streams，含 D=0/1/4 和多个学习器 | verified |
| natural 全量 | 270 streams / 127,170 events | verified，0 issues |
| controlled stress 全量 | 150 streams / 70,650 events | verified，0 issues |

执行命令、开始时间、环境、Git commit、实际 checker/test 文件 hash 写入 `execution_config.json`；退出码、耗时和日志路径写入 `execution.jsonl`。全量 JSON 同时记录原始 events/config/summary hash 和 `verifier_sha256`。`natural_smoke_8streams.json` 是增加来源 hash 字段前的真实 smoke；最终证据以本目录两个 full audit 为准。

## 证明边界

审计器不导入生产 runner 或 policy 类。它验证事件合同、可见字段、顺序、绑定、时钟、opaque commitment 连续性和摘要，不能由 hash 推出缺失的 learned-state bytes，也不证明 Python 进程隔离或完整随机状态的独立重执行。

论文 `make submission` 已改成每次直接读取原始事件并执行当前 checker，再核对 270/150 条完整 stream；不再仅凭一份旧的绿色审计 JSON 放行。该目标仍只是声明切片的本地构建预检，三份完整论文标准的状态另见 `scientific_gate_review_20261006.md`。

本轮关闭这一具体漏检后，主线转到开发集残差机制归因。除非发现影响主张的新错误，不继续扩展通用审计设施。
