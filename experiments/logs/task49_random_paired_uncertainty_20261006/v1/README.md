# Random paired uncertainty v1

这是对已核验 Random development receipt 的只读 scaffold-cluster paired bootstrap。输入为 Random 30 条 stream、common-stratum Frozen 30 条 stream 和同一份 722-row input manifest；方法、provider cache、truth、confirmation 均未改变。

- 运行脚本：`src/probe_headless/bootstrap_random_vs_frozen_v1.py`
- 输出：`random_vs_frozen.json`
- bootstrap：10,000 draws，seed `20261006`
- 估计量：每个 molecule 平均 2 seeds × 3 delays 的 Random−Frozen correctness，再按 scaffold cluster 重采样
- 结果：`-0.0567624521`，95% CI `[-0.0758720959,-0.0382199940]`
- `confirmation_read=false`，`new_scientific_predictions=false`
