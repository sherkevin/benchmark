# task10 — 数据库权威性背书：交叉采用论证 + 修正既有报告错误

## 上游诉求（owner 原话）

> "你去看看别人的 benchmark 构建的时候用的是哪些数据库，这证明了这些数据库是有权威性的，然后我们就可以去借鉴着来用了。"

即：把数据源选型的论证方式从「我们认为这个库权威」改成「**这个库被 N 家经同行评议的 benchmark 采用过，权威性已被背书**」，从而在投稿时无需自证数据源合理性。

## 目标

1. 建立「数据库 × 被哪些 benchmark 采用 × venue」的交叉采用表，venue 全部经 DBLP 核准（ACTION_RULES §4：禁止凭记忆断言 venue）。
2. 修正 `research/benchmark数据库调研报告.md` 中经源码核验发现的 4 处错误。
3. 明确指出材料线背书强度不足这一真实缺口，给出补法。

## 约束

- ACTION_RULES §4：venue 一律以 DBLP 或官方 proceedings 为准；调研结论必须回链 `reference/` 真实采样，不凭二手转述。
- 开发规范：权威数据源必须显式标注真实 URL。
- 不得删除既有报告中的历史行，修正以「订正」形式就地标注。

## 交付物

- `research/benchmark数据库调研报告.md`（重写 §一为权威性背书章 + 修正 4 处错误）
- `research/权威数据库调研报告.md`（SuperCon 背书缺失提示）
- `task/task10/report.md`

## 待核项（本 task 未闭环，需回填）

- SciCode / PoseBench 的 venue 在 DBLP 检索无记录（DBLP 不完整收录非 CS 刊），需从官方 proceedings 补核。
- MOSES 的 Front. Pharmacol. 2020 同上（DBLP 仅收其 CoRR abs/1811.12823 预印本）。
