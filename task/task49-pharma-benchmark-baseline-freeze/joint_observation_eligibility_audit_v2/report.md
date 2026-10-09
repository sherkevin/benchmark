# Joint observation eligibility audit v2

日期：2026-10-05
状态：CPU qualification audit；不是方法效果实验，不打开 confirmation。

## 目的

确认现有 BBB provider panel 能否支持带显式缺失状态的 paired-observation joint reliability candidate。`system_success` 与 runtime status 分开统计：前者是缓存中“输出是否正确”的 legacy correctness 字段，后者由 `output_statuses` 定义运行成功。

## 输入与复现

- 输入：`task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json`
- SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`
- 722 个 molecule，33 个 provider，固定宽度三类字段。
- 脚本：`src/probe_headless/audit_joint_observation_eligibility.py`
- 命令：

```bash
python3 -m src.probe_headless.audit_joint_observation_eligibility \
  --input task/task49-pharma-benchmark-baseline-freeze/fingerprint_diagnostic_v1/inputs.json \
  --output task/task49-pharma-benchmark-baseline-freeze/joint_observation_eligibility_audit_v2/result.json
```

## 结果

| 检查 | 结果 |
|---|---:|
| 固定宽度 provider panel | 722 / 722 行通过 |
| 所有 33 个 provider runtime 成功的分子 | 716 / 722 |
| 至少一个 runtime failure 的分子 | 6 / 722 |
| provider-level runtime failure events | 144 |
| runtime-ok 但化学预测错误的 cells | 3,591 |
| provider pairs | 528 |
| pair runtime co-success 最小值 / 中位数 / 最大值 | 716 / 716 / 722 |
| pair co-success < 500 | 0 |
| confidence 字段 | 没有 |

144 个 error cells 集中在 6 个 molecule：`z001303`、`z001377`、`z001405`、`z006990`、`z071322`、`z072045`；每个含 24 个 B3clf 变体 error。`output_labels` 在这些 cell 为 null。

## 判定

1. complete-case joint fit 在这个冻结 panel 上**不再因覆盖率被阻塞**：716 行可用于全 panel paired observation；其余 6 行应保留为显式 `⊥`，不能删掉。
2. 这只是数据资格结论，不是联合模型效果结论。仍需 scaffold-disjoint held-out 预测、缺失校准、matched planner utility 和 online delayed-update 闸门。
3. `system_success=0` 的 3,591 个 runtime-ok cells 是化学错误，不是 runtime failure；它们可作为 `r_chem=0`，不能编码成 `⊥`。
4. 当前 cache 没有 confidence、propensity、真实 sequential call cost 或 release receipt，因此不能凭此文件宣称在线 paired-call 或 VOI 已验证。

旧的 `joint_observation_eligibility_audit_v1` 把 `system_success` 当作 runtime status，结果已作废；详见其 `INVALID_SEMANTICS.md`。
