# 当前四工具：学习型选择器的匹配训练对照

日期：2026-10-07。状态：执行前冻结；开发实验，不使用旧157 confirmation。来源：准备卡、四工具headroom、独立方法审查。对应三份标准的E1/E3/E5：补现代学习型控制并定位调用前排序瓶颈。

## 问题与主张

相同表示、网络、初始化与训练预算下，BT+CLS相对排序训练是否改善工具选择？非线性正确性训练本身是否改善现有corrected Frozen？本轮只比较固定训练方案，不将标准BT/MLP当新贡献，不声称完整BayesianRouter复现。最近邻为已审计BayesianRouter，来源commit `2ee98a87bb06d88625bcd5002848f5448b2476e1`，详见`../baseline_audit/bayesianrouter/SOURCE.json`；复用仓内`pharma_fingerprint_diagnostic.py::RewardHead/loss_for`。

## 数据与信息

722 BBB development molecules，固定四个checkpoint的原生BBB阳性概率。provider顺序固定：ecfp_logreg / ecfp_mlp / ecfp_random_forest / graph_message_passing。阈值>=0.5为BBB阳性，与独立truth比较得到正确性0/1，不能把阳性概率当正确性。全部菜单与分母保留，不按分歧筛训练集。

输入只来自已核`experiments/logs/task49_fit_score_reference_20261007/v1/`与原`fingerprint_diagnostic_v1/inputs.json`的truth/scaffold/五折索引；禁止读取旧33工具预测。现有722×65归一化Morgan64+截距去掉最后常数列，得到64维；不改指纹、不重跑provider。原5折fit/validation/evaluation与scaffold分离保持，validation不并入fit。完整输入及source hashes封存后传OSS，worker验证SHA后训练。

## 模型、损失与数学case

两方案完全同构：64→64带bias Linear、ReLU、两个64→4无bias头。score head用于argmax选择；CLS head只辅助训练。同fold/seed创建一次初始state，再分别复制；参数量4672。两个seed为0/1；每方案每折每seed一次，共20 fits。

设$s_{ip}$和$c_{ip}$为样本$i$、工具$p$的score/CLS logits，$r_{ip}$为工具正确性；$D$是同一分子上全部正确—错误工具有向配对。

Case：某分子的四工具正确性为$(1,0,1,0)$，其$D$有四对$(0,1),(0,3),(2,1),(2,3)$；两个head各输出四个实数。这里$s$不自动是正确率。

$$L_{BCE}=\operatorname{mean}_{i,p}\operatorname{BCEWithLogits}(s_{ip},r_{ip})+0.2\operatorname{mean}_{i,p}\operatorname{BCEWithLogits}(c_{ip},r_{ip}).$$

Case：上例四个score logit分别接受1/0/1/0监督，CLS亦然；两头使用全部fit分子、全部四工具单元格。

$$L_{BT}=\operatorname{mean}_{(i,a,b)\in D}\operatorname{softplus}(s_{ib}-s_{ia})+0.2\operatorname{mean}_{i,p}\operatorname{BCEWithLogits}(c_{ip},r_{ip}).$$

Case：上例比较正确工具0和错误工具1时，降低$\operatorname{softplus}(s_{i1}-s_{i0})$会提高正确工具的相对分数。2对2产生4pair，1对3产生3pair，故BT为全pair均值，非每分子等权。全对/全错行没有直接BT项，但CLS仍更新共享encoder。空pair函数返回可反传零；若整折fit没有pair，停止该fit并记不支持BT，不能换划分。主损失替换同时改变监督、权重、梯度尺度，本轮不单独归因于某一项。

## 固定训练与选型

AdamW lr=0.001、weight_decay=0.01，其余使用PyTorch默认；full batch，float32，200个epoch，固定200次更新。CUDA deterministic、TF32关闭，CUBLAS_WORKSPACE_CONFIG=:4096:8；不做超参搜索。训练只接收fit tensor和fit entity白名单。

每次完成更新后计算validation score-head argmax正确数；最大者为best epoch，同分取最早。两方案validation loss仅记录，不参与选择/破平局。若validation无mixed分子，两者统一取epoch200并记录选择无信息；因为全对/全错行的top1恒定，有mixed时全体与mixed-only正确数的epoch排序一致。所有200epochs仍完成。best state冻结后才构造evaluation tensor和结果。离线fit是全反馈监督，不能误称selected-only在线训练。

## 运行与证据

单个Nebula A800任务，1 worker×1 GPU，pytorch280，15CPU/240GB；项目damo_genomic_gene、队列damo_genomic_gene_a800。0新provider调用、0外部LLM。训练超时1200秒、入口总预算1500秒；失败不换参数继续跑。

输入：`oss://ai4sci-develop-storage/jingwu/benchmark/inputs/fresh_learned_prior_20261007_v1/`；输出：`.../jingwu/benchmark/runs/fresh_learned_prior_20261007_v1/`。固定已推送代码SHA、输入SHA、来源SHA。逐epoch训练/验证loss、验证正确数、时间和选中epoch；20个best checkpoints、10个共享初始checkpoints、2888条OOF记录、环境/GPU/内存与失败全留存。输出在本地盘形成紧凑archive和manifest再顺序写OSS，stdout只小进度；回收hash核验并独立复算forward、epoch选择和全部指标。所有输出目录不可覆盖。

## 评价与下一步

argmax平局按冻结provider顺序取首个。任一fit失败/非有限loss/超时均使整批incomplete，不能删除折后触发go。每seed总分母722、mixed分母147；两seed不是独立数据集。不把两个seed汇成1444个独立观测。报告所有5折、overall/mixed top1、选中工具、rescued/harmed、每折净差、5个leave-one-fold-out(LOO)、训练/推理成本；另报fit-only、raw Frozen和corrected Frozen锚点。

1. 学习型选择器继续线：同一模型家族在两seed中分别对corrected Frozen至少+1pp，且每seed五个LOO差全部>0。
2. BT优先线：BT通过第一条，且两seed对BCE均至少+1pp，每seed BT−BCE五个LOO全部>0。
3. 若两家族通过第一条但BT未过第二条，优先BCE；仅一者过第一条则使用该家族。若两者均未过第一条，停止这个固定Morgan64训练方案，不扫学习率/正则/seed/损失系数找正结果，不推翻全部非线性表示或论文目标。

LOO按剩余分子正确数差除以剩余分子数，不等权平均折率；1pp判据使用未舍入值。best checkpoints和逐epoch日志只支持独立复算best forward与“按记录哪轮应被选中”，不等于重执行所有200轮。1pp是资源投入线，LOO是描述性稳定性，不是显著性。该批补静态控制，不证明在线收益、自然漂移或新机制创新。BT score共同平移不可识别；独立CLS head不校准score head。未来接入概率残差前须另固定分数语义，不能直接sigmoid或沿用类别权重修正式。新方法确认须另外未读数据，当前157不重新用作选型后确认。
