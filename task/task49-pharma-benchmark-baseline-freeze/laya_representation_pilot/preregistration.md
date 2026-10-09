# Laya 表示提取 pilot：单卡、无标签、无训练

日期：2026-09-26。阶段：输入已通过，准备单条 Nebula A800 任务。

这是后续表示/学习诊断的数据准备。唯一主张：固定 Laya checkpoint 在未截断的真实分子输入上，能导出与其原始 logits 对齐的 candidate-marker 表示。最近参照为原始 Laya scorer；不在本批报告“优于 baseline”或在线学习有效。

使用 730 个现有 BBB 开发池输入，完整 33-provider 菜单按 provider_id 排序。仅加载 entity_id、SMILES、endpoint 和 provider_id。输入文件不含预测、正确性、参考标签、训练曝光信息或失败分子名单。菜单名称只是当前可复核的身份，不称作完整语义工具卡。checkpoint revision `1c5edc17a7acd8701df6fc341c0d179f1c62c982`；模型 SHA-256 `891102d372688fc2a094dac56a384bc537b87c63f21f9f3dac0be2b7cbc8d86c`；源码 commit `846593571c0bff9344fd8a80b951dd61f4e39c3f`。

输入采用已通过无标签检查的 max_len=1024、head_max_len=512。预算 1 worker × 1 A800，batch=8，seed=0，bf16 autocast，eval/inference 模式，无梯度和更新。权重与 tokenizer 先从本机放到项目允许的 OSS 路径；容器不访问外网、不拉新模型。任务最多运行 30 分钟，出错停止并保留日志，不另起并发任务。

输出每个请求的 33×1024 marker、原始 33 logits、token 哈希及 row→shard 映射。marker 是原模型 scorer 之前的张量，通过 forward hook 取得，不改变原 forward；把 marker 重新送入原 scorer 必须复现 logits。choice softmax 不解释为正确率。原始数组保存到压缩 npz，逐请求 JSONL 与环境/配置/失败信息同步写盘；输出文件均登记 SHA-256。

否定条件：权重 hash 不符、strict load 失败、marker 缺失或非有限、回送 scorer 的 logits 不一致、漏行/重行、输入发生截断，都使提取失败，不用零向量填补。实际卡型必须是 A800，缺 GPU 不回退 CPU 冒充远程测量。

本批同时在第一个输入上做 5 次预热和 20 次单请求 forward 时间测量，报告原始时间序列及 p50/p95；另报 tokenizer/传输和 batch 时间、峰值显存。20 次仅定位量级，不代表服务负载时延，更不代表在线更新时间。

完成后才进入另卡登记的表示比较：指纹、Laya 线性/非线性 head、train-only Best Fixed 和直接性质预测控制。按同一 scaffold 划分与初始化权限，在 BBB development 上判断固定表示是否有可学习信号，不能把它冒称确认集或新算法。本批不决定最终 backbone，也不重开已有最终确认数据。
