# 执行状态与回收边界

2026-10-07。已完成：终态success/FINISHED，20fits与2888预测已完整回收并独立核验，科学结论见[report.md](report.md)。执行commit：`86543eca491814a19634529e27237d80f04083c4`，已push。输入SHA：`33c9d092f840e4f2c75eec562271abe00891de0362a6ec3a8ad9c78a7aeb1cfc`，722×4、原五折、1540658 bytes。OSS上传后下载回验SHA相同。只有这一项远端任务：`01a114f6d306db0a283dce1d4453aa33`。

- 准备与合成测试：`experiments/logs/task49_fresh_learned_prior_20261007/{prepare_v1,input_v1}/`。8项合成测试通过；0新provider forward。
- 远端操作：同目录`remote_v1/`，含config、input manifest、传输回验、doctor、队列、dry-run、实际提交、任务号、逐次状态。查询失败保留，不触发重复提交。
- OSS：`ai4sci-develop-storage/jingwu/benchmark/inputs/fresh_learned_prior_20261007_v1/`与`runs/fresh_learned_prior_20261007_v1/`。密钥仅在忽略的.env与临时0600配置中，不进入文档。
- remote_v1最初状态ready/READY，代码下载中；stdout首次返回no containers，第二次状态查询TLS中断。这些是启动/传输状态，未计为科学结果。后续以新状态与完整artifact为准。

独立核验器不导入trainer、preparer或原RewardHead，用NumPy重算best checkpoint的两头logits、validation-best forward、OOF选择与正确性、配对/LOO及go。逐epoch只能核对日志中的选型逻辑，未声称独立重执行4000步训练；GPU计时为原运行记录，未独立重测。

实现者另只读对照核验器和输出schema，未发现字段、argmax、epoch、BT方向或资源规则不匹配。本批完整结果、独立核验与下一步结论已在report.md记录；正文数字表已回填主稿且候选编译通过，正文恰好8页，原方法与确认主张未改。
