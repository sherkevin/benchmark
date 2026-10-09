# Endpoint qualification matrix v1

- 日期：2026-10-04
- 范围：只读核对 BBB、AMES、hERG 的 truth、provider coverage、endpoint equivalence、lineage 和 confirmation 条件；没有训练、预测或打开 confirmation。
- 机器收据：`matrix.json`
- 生成器：`src/probe_headless/build_endpoint_qualification_matrix_v1.py`

## 决定

当前 benchmark 的端点角色收敛为：

| endpoint | 当前角色 | 可审计分子 | provider | 主要硬缺口 |
|---|---|---:|---:|---|
| BBB | `development_primary` | 716 | 33 / 7 families | provider training membership、model-fresh snapshot、旧 panel overlap 后的 confirmation |
| AMES | `appendix_candidate` | 129 | 4 / 4 families | 当前池覆盖小、provider lineage 和独立 snapshot 未闭合 |
| hERG | `blocked_semantics` | 43 | 3 / 3 families | TDC Wang、FP-ADMET liability/blocker、ADMETlab assay/cutoff 尚不能证明同一端点 |

这份矩阵没有把 AMES 的 129 个可连接分子写成“已经通过的第二主 benchmark”，也没有把 hERG 的同名列当作共同真值。最终多端点 confirmation 保持关闭。

## 为什么这能推进 benchmark

此前 Gate A 的信息分散在数据合同、hERG 来源审计和 confirmation 候选清单里。现在每个端点都同时列出 truth 来源、当前池覆盖、provider 数量、family 数、equivalence、lineage 和 confirmation 状态。后续如果要补 AMES，只需针对表中的两项硬缺口补证据；hERG 则先解决 assay/cutoff 语义，再谈 provider 运行。

## 与验收标准的关系

这一步提升了 Benchmark 的可审计性，但没有增加 Gate A 分数：BBB 仍是 development primary，AMES 仍是 appendix candidate，hERG 仍 blocked；provider training membership、confirmation lineage 和冻结多端点矩阵仍未完成。
