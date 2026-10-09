# PAR-JEV CPU gate v0

状态：候选审查卡；没有效果结论，不打开 confirmation，不提交 A800。

## 先决审查

1. `audit_par_jev_availability.py` 只读 frozen BBB 输入的 fit rows，逐 fold、逐
   provider 报告 $n_{a,y}$、有效返回数和 $|\hat u_{a,1}-\hat u_{a,0}|$。如果
   支持不足，或差异显示 $V\perp Y\mid v$ 不是可接受的工作假设，PAR 的
   `u(v)[q h_1+(1-q)h_0]` 只能作为受限近似，不能扩展成一般全概率结论。
2. 所有 PAR 结果必须标明 `assume_availability_independent=True`；不允许通过删掉
   参数检查来绕过这个边界。

## CPU 实验合同

- 输入：`fingerprint_diagnostic_v1/inputs.json` 的 722 个 BBB development rows。
- 5 个 scaffold-disjoint outer folds、2 个 seeds、delay `{0,1,4}`；fit/validation
  只用于冻结 logits 和选择 validation-best，evaluation 只走 selected-only replay。
- 冻结的每题 logits、provider menu、selected output、feedback status、release tick、
  state version 和 state hash 必须写入 JSONL。terminal 不能带 truth、未选 panel 或
  correctness；correctness 只在 release 时出现。
- 候选 PAR 只保存 q、每个 provider 的 h(0)/h(1) 和 u 的 scalar offset/precision。
  选择评分是因子化分数；释放更新严格按 kernel contract。

## 匹配对照

至少包括：Frozen q/h/u、u-only、q-only、h-only、q+h、PAR q+h+u，以及使用同一
selected status 反馈、同一表示、同一 delay 和同一 warm start 的 direct system-success
offset。Best Fixed 和 Oracle 只作控制，不参加可部署排名。

## 必须通过的闸门

- G1：独立 verifier 拒绝错 provider、重复 release、未来字段、unknown 的 q/h 更新、
  过期 version 和未声明的因子化假设。
- G2：标量 synthetic replay 与声明的曲率锚定递推逐事件一致；同时记录它与正则化
  batch logistic 解的差异，不能把二者写成相等。
- G3：全零 offset 时逐字节等于冻结 q/h/u；只开启 u 时不改变 q/h 状态。
- G4：每个 provider、每个 truth 条件和 status 的 selected support 被报告；零支持
  单元不得给出在线 provider 结论。
- G5：同一 stream、同一 delay 的 matched controls 能拆出 u、q、h 和概率积分的作用，
  主指标同时报告 system-success、valid-return accuracy、Brier/log loss、coverage
  以及决策/更新 p50/p95。
- G6：如果要使用“动态”表述，必须有预登记的 provider/version/menu 或分布事件；
  当前顺序审计没有证明漂移，不能仅凭 delay 或更新次数称为环境动态。

## 停止条件

G1 或 G2 失败即停止。若 PAR 在两个以上 delay 不超过同探索 Frozen，或只超过 Frozen
而没有超过 matched direct baseline/Best Fixed，结论限于候选校准对照；不调参追分、不
打开 confirmation。失败结果和支持不足原样保留。
