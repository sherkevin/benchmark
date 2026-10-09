# task5/experiment — 测试记录索引

本目录记录 task5 为验证「分阶段(并行执行) vs 不分阶段(单步执行)」两种回答模式效果差异而做过的每一项测试,**一个测试一篇文档**。新文档请套 [`TEMPLATE.md`](TEMPLATE.md)(六节:测试时间/测试目的/测试结果/卡点排查方式/卡点排查结果/后续解决方案)。结论汇总见上级 `../CONCLUSION.md`,原始数据见 `../results.md`,12 份成稿见 `../runs/`,对外卡点成品见 `../block.md`。

实验时间 2026-06-04,全程 API 驱动(非浏览器),账号 sher,平台 `http://nobelclaw.aliyun-inc.com`。

| 编号 | 测试 | 一句话结果 |
|---|---|---|
| [0001](0001-api-decomposition-trigger.md) | API 能否触发「分阶段/不分阶段」分叉 | 能;`event: decomposition` 即分叉点,confirmed_tasks 走并行、否则走单步;但触发是随机的(8/12) |
| [0002](0002-platform-tools-availability.md) | 三领域预测工具是否可用 | 全停摆:DiffCSP 离线、无 cif、predict_admet 是 mock、半导体无带隙工具 → 两模式都退化为记忆+文献 |
| [0003](0003-single-vs-parallel-12-runs.md) | 6 题×2 模式完成率/质量对比 | 单步 6/6 出完整成稿;并行 0/6,全部在汇总前卡死 |
| [0004](0004-parallel-hang-root-cause.md) | 并行卡死的后端根因 | 多个子 agent 共享同一个 AsyncSession 并发提交 → DONE_SENTINEL 永不入队 → 消费循环死等 |
| [0005](0005-parallel-vs-single-and-block-investigation.md) | 本轮综合排查报告(套模板) | 单步 6/6、并行 0/6;4 个卡点逐个排查到根因 + 分层修复方案 |

> 模板:[TEMPLATE.md](TEMPLATE.md) — 复制为 `NNNN-<slug>.md` 后逐节填写。
