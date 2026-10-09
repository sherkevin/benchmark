# BBB authoritative development manifest v1

日期：2026-09-29  
状态：生效的 development manifest；不是 confirmation，也没有调用 provider。

## 目的

把决议 `0005` 的 P-C1 冲突处理规则落实到一个可被后续 baseline 和方法 runner 使用的输入版本，同时保留旧 `fingerprint_diagnostic_v1/inputs.json` 作为历史实验收据。

## 规则

- 读取冻结的 BBB development input 和 TDC canonical conflict receipt。
- 对冲突 receipt 中的 10 个 canonical groups 做整体排除。
- 不修改任何标签，不按第一行、多数票或模型结果选择标签。
- 重映射 scaffold-disjoint folds；若冲突 row 出现在 fold 中则拒绝生成 manifest，而不是静默改变 split。
- 将 policy id、receipt hash、源输入 hash、排除行数和保留行数写入 manifest。

## 结果

| 项目 | 数值 |
|---|---:|
| TDC canonical conflict groups | 10 |
| 输入 development rows | 722 |
| P-C1 新排除 rows | 0 |
| 输出 development rows | 722 |
| provider 数量 | 33 |
| scaffold-disjoint folds | 5 |

输入中没有冲突 canonical row，是因为这 10 组已经在历史 unresolved 过滤阶段从当前 cache 排除；这次的意义是把“已经排除”从隐含来源升级为显式 P-C1 receipt。不能因此声称原始 TDC 文件没有冲突。

## 证据

- 输出：`experiments/logs/task49_gate_a_20260929/authoritative_bbb_manifest_v1/inputs.json`
- 构建结果：`experiments/logs/task49_gate_a_20260929/authoritative_bbb_manifest_v1/build_result.json`
- 输出 SHA-256：`94580e3ffb8a793674265c93e6d297239106d15965f6989f316f7f01fbff0f60`
- 冲突收据 SHA-256：`9e528562616ec9be1065830bc3104f1919f6da8418b0d00911303987386bd6a6`
- 源历史 input SHA-256：`a73f1cfcc0ccb7f3d607cbcfbd19d2dfacfb8539a775123af414a61c404e3bdf`

## 后续使用限制

该 manifest 只解决 truth 分母和冲突收据，不解决 baseline fairness。Greedy LinUCB 仍需补齐统一 v5 provenance、common feature strata、初始化角色和状态 hash 后才能进入正式效果回放。历史结果不得直接改名为 P-C1 结果；需要重新运行才能进入新主表。
