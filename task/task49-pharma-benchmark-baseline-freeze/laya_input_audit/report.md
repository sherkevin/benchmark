# Laya 输入检查：默认菜单预算不适用于当前 BBB 面板

2026-09-26，按运行前登记的三套配置检查 730 个分子、三个菜单，共 6,570 条序列。使用真实 checkpoint tokenizer 和固定 commit 的原始 build_sequence；没有加载权重，没有访问标签。

| 菜单 | 默认 512/192 | 扩展 1024/512 | 扩展 2048/1024 |
|---|---|---|---|
| BBB，33 provider | 0/730 完整；每条 31 个工具 id 被截断 | 730/730 完整 | 730/730 完整 |
| hERG，3 provider | 730/730 完整 | 730/730 完整 | 730/730 完整 |
| AMES，4 provider | 730/730 完整 | 730/730 完整 | 730/730 完整 |

配置写作总长度/菜单预算。所有配置均保留 730 个分子的完整 state；问题出在 BBB 候选文本。默认配置的 24 个 B3clf 变体都只剩 `b3clf`，ADMET-AI 两个版本只剩 `admet_ai`，DeepPurpose 三个编码器也发生文本碰撞。位置仍可能区分这些选项，因此不能进一步声称 embedding 必然相等；但版本与变体信息已经从输入丢失。

按预登记规则，下一轮采用通过检查的最小配置 `(1024,512)`，固定完整 33-provider 菜单。BBB 实际最长序列为 664 token。这是输入适配，不是模型创新，不证明冷启动准确性，也不继承原模型默认输入下的概率校准。若加入更长工具描述或新候选，要重新做这项检查。

## 复现与证据

```bash
uv run --python 3.11 --with 'transformers==4.57.1' python \
  task/task49-pharma-benchmark-baseline-freeze/laya_input_audit/audit.py \
  --out experiments/logs/task49_laya_input_audit_20260926/new_run
```

原始逐样本 token、marker、保留文本和哈希在 `experiments/logs/task49_laya_input_audit_20260926/v1/raw.jsonl`；配置、摘要、人工复核案例在同目录。源码、数据和 tokenizer 的哈希均在 config 中。tokenizer 由固定 revision 的 HF 镜像取得，首次直连失败已保留；没有伪造 tokenizer 或用替代模型冒充 Laya。

这一步消除了“输入截断被错当作表征不足”的混淆。下一项有意义的行动是加载已核验权重，提取无标签的候选 marker，再比较化学指纹与 Laya 的可学习性。不能仅凭这项工程修正推断动态 JEV 的方法有效。

为核对 checkpoint 使用的 Transformers 5.0.0，另在该版本重跑相同检查，结果目录为 `v2_transformers5`。逐条 token 和 marker 与 4.57.1 一致，见同级 `version_comparison.json`；这次复核只解决运行库版本差异，没有新增模型效果实验。
