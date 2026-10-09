# Provider-Graph Relay v1：held-out residual alignment（修正版收据）

状态：**authoritative corrected receipt；通过资源闸门；没有 replay/effect 结果。**

上一版 `../` 收据在随机 control 生成器中混用了 provider index 与 ID，某个 source 出现重复 target，因此不作为有效比较；旧文件保留作失败现场。修复后，随机 control 保留每个 source 的 writable out-degree 与全图 target in-degree 多重集，并拒绝 self、fallback、重复 target。

| fold | graph mean $g$ | corrected random mean $g$ | graph minus random |
|---:|---:|---:|---:|
| 0 | 2.47045 | 1.74148 | +0.72897 |
| 1 | 2.12750 | 1.55475 | +0.57276 |
| 2 | 1.55852 | 1.00492 | +0.55360 |
| 3 | 2.19913 | 1.58802 | +0.61112 |
| 4 | 1.98397 | 1.16141 | +0.82256 |

5/5 fold 的 graph mean 为正，5/5 fold 高于修正后的 random control，达到原预登记资源闸门。edge-level JSONL、配置和 hashes 在本目录；本收据只允许进入 fold0 CPU replay，不允许写成方法效果。
