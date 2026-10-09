# Menu sensitivity audit v1

日期：2026-09-27

## 问题

当前 33 个具体 provider 中有 24 个来自同一个 B3clf 家族。需要确认主 benchmark 的难度和可学习性是否被同一家族的配置变体主导。审计保留 33-provider 主任务，同时固定一份不看标签和结果的 7-provider 家族代表菜单。

## 固定菜单

家族代表按仓库 provider registry 的公开语义预先固定为：`admet_ai_1_4_0`、`deeppurpose_bbb_molnet_morgan`、`graphb3`、`lgbm_bbb`、`b3clf_dtree_common`、`fpadmet`、`admetlab_3_0`。每个代表仍是一个具体 provider；不是把同一家族多个输出取最大值，也不使用 evaluation 标签挑代表。

## 评价

在与 fingerprint diagnostic 相同的 5-fold scaffold split 上，分别计算：

1. 训练折选出的 Best Fixed；
2. 代表菜单的 evaluator-only Oracle；
3. 每个代表 provider 一个 Morgan 1024-bit logistic correctness head，训练只用 fit 区，evaluation 选择最高分。

不调参，不读取 evaluation 标签选择菜单或模型。统计单位仍是分子，失败按 system success=0。结果只用于菜单敏感性和信息上限审查，不作为动态 JEV 效果。

## 进入标准

如果 7-provider 代表菜单的 oracle headroom 明显高于 33-provider 菜单，且线性 head 的 scaffold-heldout 表现不再低于 Best Fixed，则把双菜单作为 benchmark 设计候选；否则保留 33-provider 主菜单并把家族代表结果作为敏感性附录。任何情况下都不因为结果修改代表清单。

## 停止条件

若输入 hash、代表清单或 scaffold split 不一致，停止并不解释结果；不提交 Nebula，不读取 confirmation，不把菜单审计称为方法收益。
