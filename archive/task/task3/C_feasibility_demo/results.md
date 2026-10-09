# 介入回路可行性演示 —— 真实数据运行结果

- 数据:MS-3 全 25 个靶点(每 60 候选,真实 pChEMBL)。
- agent=带技能参数的**模拟排序器**(非真实 LLM);介入=逐步缩小其噪声(透明模型)。
- 裁判=Deliverable B 的 checkpoint(与 ground truth 低难度比对)。

## 介入-成功率曲线(整体通过率 vs 介入预算)
| agent (sigma) | b=0 | b=1 | b=2 | b=3 | b=4 | b=5 | autonomy@0 | 达标介入数(≥80%) |
|---|---|---|---|---|---|---|---|---|
| strong (0.4) | 100% | 100% | 100% | 100% | 100% | 100% | 100% | 0 |
| medium (1.2) | 92% | 100% | 100% | 100% | 100% | 100% | 92% | 0 |
| weak (3.0) | 64% | 84% | 96% | 100% | 100% | 100% | 64% | 1 |

**这就是 benchmark 的签名输出:一条曲线 + autonomy@0 + 达标介入数,而非单一标量。**
- 越强的 agent:autonomy@0 越高、需要的介入越少 → 曲线左上;弱 agent 曲线右下。
- 三个 agent 的 autonomy@0 与达标介入数**明显不同**,说明该机制能区分自主性、定位'差多少介入能救回'。
- '能力性介入'才计数(此处即筛选求助);若是 git push 之类许可性中断,按 PRD 统一 auto-approve、不计。

---

# 单实例 trace —— 任务 CK2α (target=Casein kinase II subunit alpha), agent=weak(sigma=3.0), seed=2026
# 候选 60,真实活性 10 个;裁判手握真值 pChEMBL,只做'是否偏离'的比对。
# checkpoint 阈值: {'hits@3_min': 1, 'auroc_min': 0.6, 'ef@10pct_min': 2.0}

[autonomy@0(零介入)] top5=['C24·非活', 'C51✓活性', 'C23✓活性', 'C58·非活', 'C16·非活']
           hits@3=2 ef@10%=2.0 auroc=0.584 ndcg@10=0.2616 → 裁判: FAIL ❌ auroc=0.584<0.6
           ↪ 触发最低必要介入: D1 指向:你的 top 里没有像样的结合分子,重新评估。
[介入#1] top5=['C51✓活性', 'C23✓活性', 'C24·非活', 'C58·非活', 'C16·非活']
           hits@3=2 ef@10%=3.0 auroc=0.742 ndcg@10=0.487 → 裁判: PASS ✅

→ 该 agent 在第 1 次介入后通过筛选 checkpoint。autonomy@0=未通过;达标介入数=1。

