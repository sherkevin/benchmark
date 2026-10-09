# Controlled provider-version event positive control v1

日期：2026-09-29
状态：已完成；development positive control，不进入主 benchmark、confirmation 或 A800。

## 目的

普通 BBB 回放中 provider、菜单、模型版本和分子分布固定，在线方法没有明确的变化对象。本正控在同一份真实冻结 BBB 输出上预先注入一个 provider version degradation：每个 fold 的 fit-only Best Fixed provider 在 evaluation 中点之后变成不可用。policy 不会看到 version flag，只能从 selected provider 的 delayed failure feedback 学习。

## 协议

- 5 个 scaffold folds、2 个 seed、delay `{0,1,4}`，共 90 条 stream、38,988 条事件。
- 事件前使用冻结 panel；事件后只把目标 provider 的 selected terminal 改为 `failure`，其它 provider 和输出不变。
- 方法：Frozen direct head、fit-curvature scalar residual、matched direct Online-SGD。
- 事件 target 只由 fit 区 Best Fixed 决定，不看 evaluation reward。
- 独立分析从 raw terminal 和冻结输入重算 reward，并核对事件 cell、selected-only、provider identity 和 delay。

## 结果

| 方法 | delay | 总体 accuracy | 事件前 accuracy | 事件后 accuracy | 事件前目标 provider rate | 事件后 rate | 首次 failure 后 rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Frozen direct | 0 | 0.8699 | 0.8917 | 0.8485 | 0.0792 | 0.0715 | 0.1192 |
| Fit-curvature residual | 0 | 0.8872 | 0.8903 | 0.8842 | 0.0847 | 0.0317 | 0.0432 |
| Matched Online-SGD | 0 | 0.8893 | 0.8750 | 0.9036 | 0.0819 | 0.0069 | 0.0000 |
| Frozen direct | 1 | 0.8699 | 0.8917 | 0.8485 | 0.0792 | 0.0715 | 0.1211 |
| Fit-curvature residual | 1 | 0.8831 | 0.8875 | 0.8787 | 0.0861 | 0.0330 | 0.0479 |
| Matched Online-SGD | 1 | 0.8865 | 0.8764 | 0.8967 | 0.0958 | 0.0083 | 0.0000 |
| Frozen direct | 4 | 0.8699 | 0.8917 | 0.8485 | 0.0792 | 0.0715 | 0.1187 |
| Fit-curvature residual | 4 | 0.8865 | 0.8875 | 0.8855 | 0.0667 | 0.0317 | 0.0341 |
| Matched Online-SGD | 4 | 0.8948 | 0.8861 | 0.9035 | 0.0722 | 0.0137 | 0.0000 |

Fit-curvature residual 相对 Frozen 的事件后 accuracy 差值：

- delay 0：`+0.0357`，5-fold paired bootstrap 95% CI `[+0.0013,+0.0836]`；
- delay 1：`+0.0302`，CI `[-0.0014,+0.0754]`；
- delay 4：`+0.0371`，CI `[+0.0041,+0.0877]`。

目标 provider 的事件后 selection rate 相对 Frozen 的差值为 `-0.0398/-0.0385/-0.0398`；首次 post-event failure release 之后的差值为 `-0.0760/-0.0732/-0.0788`。事件注入实际发生在 50/90 条 stream 中，其他 stream 是 policy 没有再选目标 provider，因此没有伪造 failure cell。

## 判断

这个正控回答了一个限定问题：当确实发生 provider version degradation 时，fit-curvature residual 可以利用 selected failure feedback 降低对退化 provider 的选择，并改善事件后表现。它不能证明自然运行环境一定有这种漂移，也不能把正控结果写成主 benchmark 的动态收益。

结合静态 BBB 结果，当前证据更清楚地分成两部分：

1. 没有动态事件时，在线 residual 不应破坏 Frozen，但也没有稳定增益；
2. 有明确退化事件时，低维、fit-curvature 锚定的状态确实有学习对象。

因此后续实验重点应从“在静态回放上继续调更新公式”转为“建立真实或有来源的 provider version/menu/distribution event，并在事件前后做严格 held-out 评估”。本正控只作为机制 sanity check，不打开 confirmation。

## 证据

- 预注册：`preregistration.md`
- 原始事件：`experiments/logs/task49_composition_20260929/controlled_version_event_v1/run_v1/events.jsonl`
- 配置：`experiments/logs/task49_composition_20260929/controlled_version_event_v1/run_v1/config.json`
- event roster：`experiments/logs/task49_composition_20260929/controlled_version_event_v1/run_v1/event_records.json`
- 独立核验：`experiments/logs/task49_composition_20260929/controlled_version_event_v1/run_v1/verification.json`
- 独立分析：`experiments/logs/task49_composition_20260929/controlled_version_event_v1/analysis.json`
- 分析脚本：`src/probe_headless/analyze_controlled_version_event_v1.py`

关键文件 SHA-256：`events.jsonl=050ba7bef3c37957e214f45bc74f7b3d79f62ab45cf076a0c8b86da1324fe556`，`verification.json=ff0d0c4bad42f98f6f41b23dc2d718adeb83f8980aedfa7774fbadacae20dc94`，`analysis.json=d5119cd590b09046d16ad3d4869dd49971d0d97ff1cda4db22f06931c2fc253a`。
