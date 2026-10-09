# Laya 输入完整性诊断（运行前登记）

日期：2026-09-26。范围：纯 tokenizer/序列审计，不加载权重、不训练、不读正确性标签、不提交 GPU。

要排除的问题：33 个具体 provider 的差别或分子 SMILES 在默认 token 预算下被截断，导致后续“表示不够好”的解释不成立。最近的参照是 Laya 自己的默认序列实现及 README 中对高候选数的限制说明，不把改长输入视为算法贡献。

固定源码 commit `846593571c0bff9344fd8a80b951dd61f4e39c3f`，tokenizer/checkpoint revision `1c5edc17a7acd8701df6fc341c0d179f1c62c982`。读取 checkpoint 的 tokenizer 原件；运行源码中未修改的 serialize_state、render_criterion、render_options、build_sequence 四个函数。AST 提取只为不加载无关 torch 模型依赖，不重写其算法。

数据只读 730 分子的 entity_id、canonical_smiles，以及审计表的 provider_id 和 endpoint，不读工具预测、正确性或参考标签。每个端点菜单按 provider_id 排序；state 只含 SMILES 和 endpoint。工具名称采用原始 id，不增加准确率描述。相同名称的意义是否充分，属于下一轮工具卡审查，完整性通过不代表语义已足够。

固定三个配置 `(max_len, head_max_len)`：官方 `(512,192)`，扩展 `(1024,512)`、`(2048,1024)`。三种端点各对全部 730 个输入审计。每个配置保留原始 token、marker、每个工具实际保留文本、instruction 和 state 保留长度及哈希，不能只报均值。

判据：所有 marker 保留、每个原始工具 id 完整、完整指令和 SMILES/endpoint 保留、序列不超 max_len。另报候选文本碰撞及固定 48-token option cap 的影响。默认配置若失败，选三个配置中最小的全通过者；都失败则停止表示 pilot 并修输入合同。采用扩展预算须标作输入适配，并单独计量时延，不能直接套默认 checkpoint 的效果/校准声称。

这是确定性全量检查，不报告泛化置信区间。失败 traceback、环境版本、源文件哈希、git HEAD、完整命令写入配置和事件；输出在 `experiments/logs/task49_laya_input_audit_20260926/`。人工复核至少一个碰撞或截断实例。下一步只由输入是否完整决定，不参考预测效果挑配置。
