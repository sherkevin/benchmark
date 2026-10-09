# Joint-success residual JEV CPU gate v0

状态：候选预注册草案；当前只完成 kernel 合同测试，尚未跑全量效果。

## 方法边界

每个 provider 有一个冻结的 query-specific end-to-end success logit
$\ell_{a,0}(v)$，以及一个 online scalar residual $c_a$。目标是
$S=1$ 当且仅当工具有效返回且结果正确。失败可直接给 $S=0$，scored 的错误结果也
给 $S=0$，scored 的正确结果给 $S=1$，unknown 不更新。该候选不使用
$u(v)[q h_1+(1-q)h_0]$，因此不依赖 availability/property 条件独立。

## 固定输入和信息边界

- `fingerprint_diagnostic_v1/inputs.json`，722 BBB development rows、33 providers、
  5 scaffold-disjoint folds。
- 每个 fold/seed 只用 fit/validation 训练冻结 direct system-success head；evaluation
  只允许 selected provider 的 status/label，correctness 到 release tick 才能进入更新。
- delays 固定 `{0,1,4}`；每个 event exactly once，terminal 不含 truth、完整 panel 或
  correctness；记录 decision/release tick、provider、version、state hash 和支持数。
- 5 scaffold folds × 2 seeds × 3 delays；所有候选共享输入、warm start、菜单、顺序、
  feedback 和 failure/unknown 语义。

## 匹配对照

1. Frozen joint-success head；
2. Joint residual PAR，曲率锚定递推；
3. 相同 direct success head 的 fixed-step scalar update；
4. 相同 selected status 的 direct Online-SGD provider head；
5. train-only Best Fixed 与 evaluator-only Oracle 作为控制。

不把 Frozen 或 Best Fixed 的差距解释成方法收益；主要比较 paired per-scaffold
system-success accuracy，另报 valid-return accuracy、Brier/log loss、provider
coverage、unknown 比例和 decision/update p50/p95。

## 通过和停止条件

- G1：独立回放能逐事件重算选择、延迟、target system-success、scalar precision 和
  offset；篡改 release tick、provider 或 unknown payload 必须被拒绝。
- G2：offset 全零时逐字节等于 Frozen；只关闭学习时所有 version/state 不变。
- G3：至少每个 fold 报告 selected support 和 update counts；零支持 provider 不作
  在线结论。
- G4：joint residual 在至少两个 delay 上超过同一冻结起点的 fixed-step/direct
  matched control，或在预先锁定的更新成本/恢复一致性指标上有明确优势。
- G5：如果论文使用“动态”一词，必须另有预登记的 provider/version/menu 或分布事件；
  单靠延迟回放不构成环境动态证据。

任一 G1/G2 失败即停止。若只超过 Frozen、没有超过 matched direct baseline 或
Best Fixed，结论限于低成本在线校准对照；不调 learning rate、precision、warm start
或 delay 追分，不打开 confirmation，不提交 A800 效果任务。
