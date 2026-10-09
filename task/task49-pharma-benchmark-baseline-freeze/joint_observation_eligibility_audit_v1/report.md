# Joint observation eligibility audit v1

日期：2026-10-05  
状态：CPU qualification audit；不是方法效果实验，不打开 confirmation。

## 目的

检查现有 BBB provider panel 是否足以支持共享对话中提出的 paired-observation joint reliability candidate。审查重点是：固定宽度记录是否真实、运行失败是否会造成 complete-case 偏差、provider pair 是否有可用的共同成功覆盖，以及当前输入是否带有 confidence 字段。

## 输入与可复现性

- 输入：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- 输入 SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- 722 个 BBB development molecules；33 个 provider；每行保存 `system_success`、`output_labels`、`output_statuses` 三个长度为 33 的字段。
- 执行脚本：`src/probe_headless/audit_joint_observation_eligibility.py`
- 运行命令：

```bash
python3 -m src.probe_headless.audit_joint_observation_eligibility \
  --input task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json \
  --output task/task49-pharma-benchmark-baseline-freeze/joint_observation_eligibility_audit_v1/result.json
```

## 结果

| 检查 | 结果 |
|---|---:|
| 固定宽度 provider panel | 722 / 722 行通过 |
| 所有 33 个 provider 都成功的分子 | 178 / 722 |
| 至少一个运行失败的分子 | 544 / 722 |
| provider-level failure events | 3,735 |
| provider pairs | 528 |
| pair co-success 最小值 | 349 |
| pair co-success 中位数 | 560 |
| pair co-success 最大值 | 648 |
| co-success < 400 的 pair | 27 |
| co-success < 500 的 pair | 43 |
| confidence 字段在该输入中 | 没有 |

机器结果和输入 hash 见 `result.json`；该数字是 722×33 个 provider 单元格中 `system_success=0` 的计数。

## 判定

1. **complete-case joint fit 阻塞。** 只保留 178 个全成功分子会丢掉 75% 的行，并且失败并非均匀缺失；例如 `deeppurpose_bbb_molnet_cnn` 的失败率明显高于 `admetlab_3_0`。这种过滤会把运行可用性和化学预测相关性混在一起。
2. **带显式缺失状态的 joint fit 仍是候选。** 运行时 `error` 是已观察到的事件，应编码为 $⊥$ 或由单独的 runtime hazard head 建模；不能把它填成 0/1，也不能使用未选 provider 的当前缓存输出。
3. **当前输入不能证明在线 paired-call 成本。** 它是历史 development panel，适合做 fit-only joint observation 资格诊断；它没有给出一个合法 sequential policy 的实际调用时间、propensity、delay 和 release 收据。
4. **confidence 不能从本文件推断。** 第一版候选如果需要 confidence，必须另做字段来源审计；否则联合观测模型只能使用结构化 output/status。

## 对方法候选的直接影响

第一版不要把模型写成只对所有 provider 都有二元输出的 $
P(o_C\mid x,e,y)$。应先固定观测域，例如 $o_a\in\{0,1,\u22a5\}$，其中 $⊥$ 表示该 provider 在该题运行失败或没有可用输出；再比较：

- `Independent Evidence`：给定 $x,e,y$ 的各 provider 条件独立，但允许各自有 $⊥$ 概率；
- `Joint Reliability`：增加小型共享 error mode，仍对 $⊥$ 和二元 output 使用同一观测合同；
- 两者使用同一 fit-only rows、同一 provider menu、同一缺失处理和同一 validation split。

只有在 held-out pair likelihood、缺失校准和 synthetic same-marginal/different-joint 测试均通过后，才有理由实现有限 horizon planner。当前不修改生效方法文档，不提交 A800。
