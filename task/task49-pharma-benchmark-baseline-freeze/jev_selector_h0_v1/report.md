# H=0 双标签 JEV 选择器 CPU 回放 v1

日期：2026-10-05
状态：**已完成；独立恢复通过；否证 H=0 直接选择候选。**

## 这次实验回答什么

这里把正式 label contract 接到最干净的冷启动选择器上：一个 64 维共享编码器，两个
33-way head，分别预测 `r_chem` 和 `r_run`。选择分数为

`q_a(x)=sigmoid(c_a(x)) sigmoid(u_a(x))`。

`r_chem` 在 runtime error 时是 null 并被 mask；`r_run` 对所有 provider cell 监督。决策
时只输入分子 Morgan-1024 指纹和完整 provider 菜单，不读取当题任何 provider 输出。

这不是在线回放：没有探索、延迟队列、后验更新、验证调用、拒答或 VOI。它只检验新标签
接口能不能提供一个可靠的 H=0 cold-start selector。

## 固定协议与证据

- 722 个 BBB development 分子、33 个 provider、5 个 scaffold-disjoint outer folds；每折
  fit/validation/evaluation 为 `444/133/145` 或 `492/86/144` 等输入中冻结的划分。
- fit 区训练 200 epochs，validation objective 为 masked correctness BCE + runtime BCE，
  AdamW `lr=1e-3`、`weight_decay=1e-2`、hidden-64、seed slots `0,1`，不扫参。
- Best Fixed 每折只从 fit 区选择；evaluation 只由 evaluator 用完整缓存计算结果。
- 十个 fold/seed 槽位全部完成。逐 epoch 事件 2,000 条，10 个 checkpoint 和 1,442 条
  evaluation 预测记录保存；独立 NumPy verifier 重算两头、乘积 score、argmax、固定工具
  和 oracle，`status=verified`。

机器收据：

- 原始输入 SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`；
- `summary.json`：`acecedb16b191ae01de08cc7e75dfb47e51924a8ed96961da2e7135601e0984a`；
- `verification.json`：`7392616042a0b73017567ea2c1cb5d257418d00c5a1972a57cfaeda4c4f0b915`；
- 目录：`experiments/logs/task49_jev_selector_h0_20261005/v1/`。

## 结果

| 方法 | OOF accuracy | 相对同折 Best Fixed |
|---|---:|---:|
| H=0 双标签 JEV selector | **0.887883** | **−2.769 pp** |
| Best Fixed（fit-only） | 0.915575 | — |
| Oracle（evaluator 上界） | 0.997230 | — |

双标签 selector 在 10 个 fold/seed 槽位中全部低于同折 Best Fixed；逐槽位差为
`−0.694` 到 `−4.861 pp`。已有同容量 direct `r_sys` MLP 的两个 seed OOF 均值约
`0.89335`，因此当前双头乘积还低约 `0.55 pp`。这不是统计显著性结论；seed slots 只是
确定性协议槽位，不是独立重复。

## 裁定

预登记的否证条件成立：**不能把“两个 label head 的概率乘积直接 argmax”作为主方法的
cold-start selector，也不能仅凭这条结果申请 A800。** 失败原因并不等于 label contract
错误。标签接口、null mask、runtime/chemical 分离和独立恢复均通过；问题在于未经校准的
乘积分数会让 selector 过早离开 fit-only incumbent，产生错误切换。

因此下一候选保留两个 head，但改变它们的职责：

1. fit-only 双头只提供 incumbent-relative 的校准证据和 runtime hazard；
2. 选择层采用 incumbent-safe switch，只有下界超过 incumbent 或故障 hazard 触发时才切换；
3. selected-only delayed feedback 只更新释放后的轻量 residual/ledger；不把 H=0 静态 argmax
   误写成动态收益。

这个负结果只否定直接乘积选择器，不能否定 MIRA-Relay 的安全切换、延迟更新和受控事件
目标。后续实验必须与 Frozen、Matched Online-SGD 和 Best Fixed 同表，并报告 rescued/harmed、
runtime failure、调用成本和 calibration。
