# AAMAS 2027 题目与摘要审查

日期：2026-09-29
状态：工作稿审查，不是最终投稿定稿，也不替代三份生效评价标准。

## 1. 本轮判断

旧题目 `Selecting and Verifying Scientific Tools with Delayed Outcome Feedback` 把四个实现层概念并列放进题目：选择、验证、科学工具、延迟反馈。它准确，但像系统说明书，读者看不出论文真正要解决的核心问题，也把可迁移的方法过早锁在 scientific tools 上。`verifying` 还会让审稿人预期有完整的验证保证；当前只有开发集上的 confidence-gated escalation 证据，不能承担这个承诺。

当前工作题目改为：

> **After the Decision: Learning to Adapt from Delayed Outcomes**

标题前半句提供记忆点，后半句说明研究问题；工具增强 agent、医药回放和 provider 选择放在摘要与正文中。这个题目不宣称 dynamic environment、real-time learning、continual tool lifecycle 或 universal tool use，也不把 JEV、online head、33 providers 这类实现细节写成贡献。

## 2. 标题形式的证据

我们核对了 AAMAS 2025 官方论文目录，以及 GraphRouter、RouteLLM 和 MixLLM 的一手页面。可复用的规律不是“必须起一个 fancy 名字”，而是标题先给读者一个可复述的行为或问题，再用副标题落到技术对象：

| 来源 | 标题例子 | 可复用动作 |
|---|---|---|
| [AAMAS 2025 proceedings](https://www.ifaamas.org/Proceedings/aamas2025/forms/contents.htm) | `GraphMASAL: A Graph-based Multi-Agent System for Adaptive Learning`；`AdaCred: Adaptive Causal Decision Transformers with Feature Crediting`；`ReSCOM: Reward-Shaped Curriculum for Efficient Multi-Agent Communication Learning` | 概念名/行为在前，技术范围在后；副标题具体而不堆实现细节 |
| [AAMAS 2025 proceedings](https://www.ifaamas.org/Proceedings/aamas2025/forms/contents.htm) | `Can you see how I learn? Human Observers' Inferences about Reinforcement Learning Agents' Learning Processes` | 问句可用于提出行为问题，但需要副标题和摘要迅速给出可检验对象 |
| [GraphRouter, ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/41b6674c28a9b93ec8d22a53ca25bc3b-Abstract-Conference.html) | `GraphRouter: A Graph-based Router for LLM Selections` | 先写核心行为，再写方法机制；不把数据集或部署细节塞进主标题 |
| [RouteLLM](https://doi.org/10.48550/arXiv.2406.18665) | `RouteLLM: Learning to Route LLMs with Preference Data` | 用 `Learning to ...` 直接命名学习问题，把具体数据来源放在后面 |
| [MixLLM](https://arxiv.org/abs/2502.18482) | `MixLLM: Dynamic Routing in Mixed Large Language Models` | 以 dynamic routing 概括问题，但不能由标题反推更强的动态环境主张 |

这组样本支持“after decision/outcomes”作为题目主轴；它不支持把标题写成 `dynamic JEV` 或 `scientific tools` 的实现清单。

## 3. 候选题目与取舍

| 候选 | 记忆点 | 技术清晰度 | 当前证据风险 | 处理 |
|---|---:|---:|---:|---|
| `After the Decision: Learning to Adapt from Delayed Outcomes` | 9/10 | 7/10 | 低 | 当前工作题目 |
| `After the Decision: Outcome Feedback for Adaptive Agent Selection` | 8/10 | 8/10 | 低 | 若需要在摘要页更早显式点出 selection，可替换 |
| `Learning from Consequences: Adaptive Selection under Delayed Feedback` | 8/10 | 8/10 | 低 | 最技术化的备选 |
| `When Decisions Speak Back: Learning from Delayed Outcomes` | 9/10 | 6/10 | 中 | 修辞较强，需完整方法和结果兜底 |
| `Can an Agent Learn Which Choice to Trust? Adaptive Selection from Delayed Outcomes` | 8/10 | 7/10 | 中 | 问句容易让论文看起来像行为研究，暂不采用 |

当前不使用 `After the Call`：它有记忆点，但在跨领域读者中可能被理解为电话调用；`After the Decision` 更稳，也更接近可迁移的方法问题。

## 4. 摘要的行为结构

当前摘要按以下顺序写，每一部分只承担一个动作：

1. **现实问题**：tool-augmented agent 在同一请求的多个 provider 中做选择；
2. **信息缺口**：只有被选 provider 的结果可见，且正确性反馈可能延迟；
3. **研究对象**：把问题命名为 outcome-driven adaptation；
4. **可复现实例**：33-provider pharmaceutical replay、reference labels、scaffold-disjoint splits、失败语义和反馈延迟；
5. **协议贡献**：分开 pre-call selection、post-call verification 和 state update，并统一记录指标；
6. **当前证据**：开发 BBB stream 上 selected-only online update 尚未超过 fit-only fixed selector；confidence-gated escalation 是另一个动作，不能冒充动态 selector；
7. **边界与意义**：selection、verification、confirmation 分开验收，协议服务于后续能从部署后果学习的 agent 方法。

这版故意没有写 `dynamic JEV`、`real-time`、`continual`、`universal` 或最终提升数字。动态模块和 confirmation 还没有冻结，摘要不应先于证据承诺它们。

## 5. 后续替换条件

题目可以在方法冻结后只做一次替换：如果最终贡献明确落在 selection，而不是更一般的 post-decision adaptation，可使用备选 `After the Decision: Outcome Feedback for Adaptive Agent Selection`。摘要只有在以下证据齐全后才可改成投稿版：

- 方法的状态、反馈释放和更新公式已经冻结；
- 与 Best Fixed、静态强 router、matched online/replay、contextual bandit 和 verification control 的主表完成；
- 至少一组 model-fresh confirmation 结果可复核；
- 摘要中的 headline result 有分母、成本和适用范围，不把 development replay 写成最终确认。
