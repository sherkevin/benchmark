# 四工具表示对照：未通过升级线

2026-10-07。单A800真实训练已完成，完整回收及独立核验通过；保持旧157 confirmation只读。

## 本轮问题

相同网络实现、完整initial、BCE+CLS训练和checkpoint选择下，chiral Morgan1024是否同时超过64零填充与Corrected Frozen？两组同名义参数，不匹配有效容量；不将任何正负差归因为唯一的hash碰撞。

## 结果

| 方法 | Seed0正确数 / accuracy | Seed1正确数 / accuracy | mixed正确数 /147 |
|---|---:|---:|---:|
| Corrected Frozen |579 /80.19%|579 /80.19%|108 /108|
| Morgan64零填充 |567 /78.53%|570 /78.95%|96 /99|
| Chiral Morgan1024 |562 /77.84%|571 /79.09%|91 /100|

每seed共享722个分子，不能把两seed作为1444个独立观测。

| 1024对照 | Seed | 净差pp | 救回 / 新增错误 | 五fold净正确差 |
|---|---:|---:|---:|---|
| zero_pad |0|-0.6925|4 /9|1, -4, -1, -2, 1|
| zero_pad |1|+0.1385|19 /18|1, -1, 0, -1, 2|
| corrected_frozen |0|-2.3546|13 /30|-5, -6, -4, -1, -1|
| corrected_frozen |1|-1.1080|12 /20|3, -6, 0, 0, -5|

1024相对Corrected Frozen两seed的五个LOO全部为负；相对零填充seed0五个全负，seed1为0、正、正、正、负。两seed均未满足双对照≥1pp且LOO全正的资源线。零填充组的两seed正确数与上轮64原BCE一致；这不是逐位训练轨迹复现的证明。

## 决定

停止这套固定增维构造，不扫宽度、初始化或正则；不升级为在线prior。它不支持“只提高位数就解决当前排序问题”，也不能据此证明1024无有用信息或否定所有学习型表示。当前仍未找到相对强Frozen稳定改善选择的可用新机制。

后续应先回到反馈结构与监督效率，查已有truth-recovered共享性质头和direct-property控制的历史证据，避免重复造原型。已有初步数学核查见prior_work_check.md；尚未冻结新实验，不把它当新颖性证明。

## 实际执行与核验

- Task `01a1153c076660894f3390124224a569`，success/FINISHED；执行commit `84c61207d7cccece592d0f5879e8122ef4ed2b7d`。单NVIDIA A800-SXM4-80GB，PyTorch2.8.0、NumPy1.26.4，float32 deterministic，TF32 off。
- 20fits×200steps=4000epoch，2888OOF、10initial+20best，40manifest文件全部回收。训练入口14.665秒；平台启动/传输另计；0新provider/LLM调用。
- 独立NumPy核验verified、0issues；logit最大差1.85e-07，best validation loss差9.25e-08；按epoch记录复核选型，不重执行4000步。
- 15项定向合成测试、CPU feature重建、OR与64seal检查通过；失败的任务列表、DNS/TLS和path-style探测均保留，不归为科学失败。无科学retry。

## 证据

- 冻结card、math_review、code_review、execution.md。
- 日志：`experiments/logs/task49_fresh_representation_20261007/`，原始archive、逐样本、模型及输入均保留。
- summary SHA `9037250f050edca080725669c9825ccbf274bd56d3970acf93d86ef063b4c7ba`。
- verification SHA `2094fe11f8d93aad9e6ad09d57155ee5972fe213217827c1e438aef43173de13`。
- archive SHA `3616c42499646242317c1c644d0ae954755635f67c015eba5fbfaa4b42412328`，14709409bytes。

## 回看验收

当前四工具的表示控制证据更完整；没有得到足以升级的新机制，跨baseline共同成本/state合同及新未读确认仍缺。三份科学标准继续HOLD，不改变判据分母。论文新7节结构仍在独立候选编译，主稿保留已验正文8页。
