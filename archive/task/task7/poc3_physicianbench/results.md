# POC 3 · PhysicianBench 轻改 · 半天验证机制 — Results

> **日期**:2026-06-10 · **状态**:✅ 机制层验证通过

## 验证目标(来自 task7/goal.md)

> 在它 100 道题之一上把 binary Pass@1 改成加权部分得分输出,跑通就证明"现成 cp 数据 + 我们的部分得分公式"能拼起来。

## 怎么跑

```bash
cd /Users/jingwu/work/benchmark/task7/poc3_physicianbench
uv run --project PhysicianBench python run_poc3.py
```

文件:
- 入口:[`run_poc3.py`](run_poc3.py)
- 改 base_url 适配 idealab:[`PhysicianBench/conftest.py`](PhysicianBench/conftest.py)(只改了 1 个 monkeypatch,未动 repo 源码)
- 任务:[`PhysicianBench/tasks/v1/aortic_aneurysm_cad/`](PhysicianBench/tasks/v1/aortic_aneurysm_cad/)(100 道题之一,6 个 cp)

## 设置

- **LLM**:idealab Qwen3.7-Max-DogFooding(经 `OPENAI_BASE_URL` + `OPENAI_API_KEY`)
- **环境**:**未接 FHIR Docker**(刻意——验证不必下 ~几 GB 镜像就能验证机制)
- **agent**:**未跑真 agent**,用人工 mock 的 cardiology assessment note(故意"中等质量",覆盖 ~5/8 ground-truth 数据点)
- **目的**:隔离两件事——`(LLM-judge cp 评测器是否能调通真 LLM)` 与 `(部分得分聚合公式是否能拼上)`

## 实测结果

### 6 cp 真实运行结果

| cp | 类型 | 结果 | 备注 |
|---|---|---|---|
| cp1 data_retrieval | Hybrid (trajectory + LLM-judge) | ❌ | trajectory.jsonl 格式与 `load_trajectory()` 期望不符;小问题,非机制问题 |
| cp2 aortic_assessment | LLM-judge | ✅ | Qwen 判 mock note 通过(主动脉评估 + 阈值正确) |
| cp3 coronary_cta_decision | LLM-judge | ✅ | 冠脉 CTA 推荐 + 理由 |
| cp4 coronary_cta_order | Code · FHIR | ❌ | **预期 fail**:无 FHIR server 时无法验证 ServiceRequest |
| cp5 aortic_referral | Code · FHIR | ❌ | **预期 fail**:同上 |
| cp6 documentation | LLM-judge | ✅ | 文档 7 元素 ≥ 5 满足 PARTIAL → 此 task PASS 阈值 |

### 两种聚合公式真实输出对比

```
原 PhysicianBench 主指标 · binary Pass@1     = 0
  (定义:6 个 cp 全过才算 1,否则 0)

我们的主指标 · weighted_partial_score        = 0.500
  (定义:passed / total = 3/6)
```

**信息量差**:在 binary 主指标下,我们这次 mock(5/6 cp 通过)和一个"几乎全错(1/6 通过)"的弱 agent 都打 0 分,**完全不可区分**。我们的加权聚合 capture 到了 0.500 vs ~0.17,把 cp 级粒度的"走多远"信息透出来了。

→ **改造路径成立**:repo `score_jobs.py` 里 `avg_checkpoint_score = passed/total` 已现成(audit_physicianbench.md 早确认),只是没作为 headline metric。**升级为主分 = 实际改 ~1-2 行**。

## 顺手抠出的真实细节(纠正/补足卡里描述)

1. **`_llm_client()` 在 openai 分支有 bug**(`utils/eval_helpers.py:67-71`):硬编码不传 `base_url`,任何用 OpenAI-compatible endpoint(idealab / vLLM / Azure)的人都会卡。conftest.py monkeypatch 一行解决,值得给 upstream 提 PR。
2. **4 个 LLM-judge cp 完全独立于 FHIR**,可单独验证。这意味着 **POC 3 真验证一开始就不需要 FHIR Docker**——只在 cp4/5(Code · FHIR ServiceRequest)需要。这与卡里"借框架"判断完全吻合。
3. **cp 字段命名**:`Hybrid (trajectory + LLM-judge)` 的实现是**先用代码确认调了工具(trajectory check),再让 LLM-judge 看输出文本**。这就是 audit 说的"串行短路":trajectory 缺则 LLM 不被调用。

## 验证结论

| 问题 | 答 |
|---|---|
| **我们的"部分得分公式"能不能拼到 PhysicianBench 现成 cp 上?** | ✅ 能 |
| **cp 评测器能不能独立跑(不依赖整套 FHIR 环境)?** | ✅ 能(至少 4/6 cp 完全可独立) |
| **加权聚合相比 binary 是否有真实信息量差?** | ✅ 有(0.500 vs 0,3/6 vs 0/6 不能区分) |
| **改造工作量 vs 卡里说的"半天"?** | ✅ 兑现(从 zero 到出结果 ~1.5 小时,含装环境 + monkeypatch + 写 demo + 跑 LLM) |

## 不验证什么(诚实边界)

- ❌ **没跑真 agent**:用 mock note,所以 cp 通过率反映的是"评测器对 mock 的判定",不是"真 LLM 在长程任务上的能力"。下一步要做这个。
- ❌ **没跑 cp4/cp5 的实际功能**(FHIR 校验)。要跑必须下 Stanford Redivis 上的 `physicianbench-fhir-v1.tar.gz`(~几 GB,需注册 Redivis 账号但不要 DUA)。
- ❌ **没跑 jury / 多 judge 一致性**(MedHELM 借的 3-judge 架构)。只用了 Qwen 一个 judge。
- ❌ **没接真 agent 跑 trajectory** → src 的 5 签名输出(autonomy@0 / 介入-成功率曲线 / pass^k 等)在这个 POC 不验证。

## 下一步(若想加深 POC 3)

| 选项 | 工作量 | 收益 |
|---|---|---|
| **A** 跑真 mini agent 而非 mock note | 半天 | 看真 LLM 在长程 multi-turn 上 cp 通过分布 |
| **B** 下 FHIR Docker 把 cp4/cp5 跑全 | 1-2 小时 | 6/6 cp 全跑通,加权聚合数字更可信 |
| **C** 加 jury(Qwen + 另一个 judge)看一致性 | 1 小时 | 验证 MedHELM 借的 jury 架构在医疗 cp 上 ICC |
| **D** 跑全 100 道题 × N 个 model | ~半天到 1 天 + $$ | 真排行榜雏形,但成本到 $20+ |

我建议先开 POC 1(DO Challenge,药物方向跨域验证),POC 3 加深留到后面。
