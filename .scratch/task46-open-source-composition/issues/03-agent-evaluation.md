# 03：主 Agent 交互与三层评估

Type: task
Status: open
Blocked by: 01-adapter-contract, 02-dynamic-jev-wrapper

## Question

如何将动态 Jev 建议接入主 Agent，使 Agent 实际决定 call、verify、answer、abstain，并在 PTRB-v0/Tau²/Inspect 轨迹上报告三层归因？

## Acceptance

- base Agent、static JEV、dynamic JEV 三种输入可在同一 split/预算运行。
- 记录 Jev 建议、Agent action、override、原始工具结果和终止真值。
- 报告 final error、unsafe answer、abstention precision/recall、verification gain、calls/cost/latency/calibration。
