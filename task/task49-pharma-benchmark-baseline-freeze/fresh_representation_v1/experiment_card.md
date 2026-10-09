# 四工具表示对照：Morgan64零填充与Morgan1024

2026-10-07；执行前冻结。仅722个development分子，旧157个confirmation保持只读。目的：固定BCE训练，检验提供更细的分子输入能否改善调用前选择。上一轮同Morgan64的BCE/BT均未胜Corrected Frozen，见[报告](../fresh_learned_prior_v1/report.md)。

## 1. 本轮只回答一个问题

相同网络实现、完整初始state和训练规则下，1024位输入是否同时超过64位零填充控制及Corrected Frozen？两组匹配登记参数，**不匹配有效容量**，不能单独归因为hash碰撞。高维Morgan、MLP、BCE不是本项目新算法贡献。最近邻是上一轮已核验的BCE+CLS；现代baseline出处及适配沿用其[实验卡](../fresh_learned_prior_v1/experiment_card.md)，本轮不是完整BayesianRouter复现。

## 2. 来源与权限

- 旧输入：`experiments/logs/task49_fresh_learned_prior_20261007/input_v1/inputs.json`，SHA `33c9d092f840e4f2c75eec562271abe00891de0362a6ec3a8ad9c78a7aeb1cfc`。
- 旧shared-initial：同日志目录下`remote_output_v1/extracted/training/checkpoints/fold{fold}_seed{seed}_initial.pt`，逐文件和state hash由`training/checkpoint_manifest.json`绑定；只取10个initial，不取best权重。
- 旧运行commit `86543eca491814a19634529e27237d80f04083c4`；核验`verification_v1/verification.json`。
- 722行native probabilities、truth、SMILES、scaffold、5折fit/validation/evaluation与Frozen锚点沿用旧来源链；不读旧33-provider标签，不挑难例。
- Provider固定顺序：ecfp_logreg、ecfp_mlp、ecfp_random_forest、graph_message_passing。

正确性标签为 $r_{ia}=\mathbf1\{\mathbf1[p_{ia}\geq0.5]=y_i\}$。

例：$i$是输入表中的一个分子，$a$是ecfp_mlp；native BBB阳性概率$p_{ia}=0.7$、公开真值$y_i=0$，故$r_{ia}=0$。0.7不等于工具正确率。完整fit面板可用于离线训练；validation仅用于选checkpoint，不并入fit；evaluation不参与训练或选型。

## 3. 表示与准备检查

固定RDKit2025.9.2、NumPy1.26.4；两组用同一`rdFingerprintGenerator.GetMorganGenerator(radius=2, fpSize=d, includeChirality=True)`，仅$d\in\{64,1024\}$不同。记录generator全部默认项；SMILES解析不新增标准化。保存原始布尔位与归一化结果，不使用旧AllChem实现或旧1024缓存。

逐行精确检查 $b^{64}_{ij}=\bigvee_{k=0}^{15}b^{1024}_{i,j+64k}$，$j=0,\ldots,63$。

例：1024位坐标1和65都激活，折叠后64位坐标1仅为一个1。该检查只确认构造关系，不证明被合并的信息对选择有用。

分别归一化 $u_i^d=b_i^d/\sqrt{\sum_j b_{ij}^d}$。

例：两个原始1对应两个$1/\sqrt2$。全零、非有限或解析失败时停止，不删除分子。重建$u^{64}$与旧封存64维输入最大绝对误差须$\leq10^{-12}$；OR折叠必须精确相同。任一不符停止准备，不训练。

两组输入为 $x_i^{\rm pad}=[u_i^{64};0_{960}]$ 和 $x_i^{\rm high}=u_i^{1024}$。

例：64维的0.5在补零后仍为0.5，不重新缩放；两组都是1024维单位范数。方法ID为`morgan64_zero_pad`、`chiral_morgan1024`。

## 4. 网络与初始化

两组共同1024→64带bias Linear、ReLU、两个64→4无bias头。score head路由，CLS head只辅助。登记参数均66,112；补零组只有4,672个参数实际参与预测。

每fold/seed从旧shared-initial扩展 $W^{+}=[W;U]$，旧hidden bias和两head逐字复制。

例：$W$是旧64×64第一层矩阵；$U$是64×960新矩阵；Torch CPU独立Generator seed=`2026100700 + 10*fold + seed`、float32、Uniform[-1/8,1/8]。两组加载同一完整state，保存10份initial及tensor hash；该常量不是搜索维度。

补零初始函数满足 $W^+[u^{64};0]+b=Wu^{64}+b$。

例：旧$W=(2,-1)$、输入$(0.6,0.8)$，扩展权重$(2,-1,7,9)$与输入$(0.6,0.8,0,0)$都输出0.4。避免1024默认fan-in缩小初始化。允许GPU矩阵尺寸导致的微小浮点差，不要求旧训练逐位复现。

## 5. 固定训练与选型

损失 $L=\operatorname{mean}_{i,a}\operatorname{BCEWithLogits}(s_{ia},r_{ia})+0.2\operatorname{mean}_{i,a}\operatorname{BCEWithLogits}(c_{ia},r_{ia})$。

例：分子正确性$(1,0,1,0)$同时监督score输出$s$与CLS输出$c$；全部logit为0时$L=1.2\log2$。全对/全错行仍入fit，不能只取mixed。

- AdamW lr0.001、weight_decay0.01，其余PyTorch2.8.0默认；full batch、float32、200次更新；5fold×seed0/1×2组=20fits。
- CUDA deterministic、TF32关闭、CUBLAS_WORKSPACE_CONFIG=:4096:8。
- 每步后validation score-head top1正确数最大，平局最早；loss只记录，不破平局。无mixed时两组固定epoch200。例：epoch3和7同为80个正确，选3，即使7的loss更低。全部200次更新仍执行。
- best冻结后才构造evaluation张量。argmax平局按provider顺序：$(0.2,0.4,0.4,0.1)$选择第二个，CLS不路由。
- 单Nebula A800、1worker×1GPU、15CPU/240GB、pytorch280；项目damo_genomic_gene、队列damo_genomic_gene_a800；训练预算1200秒、入口1500秒。0新provider/LLM调用。

## 6. 指标与资源决策

每组每seed分母722，mixed分母147；两seed不当1444个独立样本。报告全体/mixed top1、逐折、逐分子选择、两组配对，以及各自对fit-only/raw/Corrected Frozen的配对。保留rescued、harmed、训练/推理时间、显存和artifact大小。

配对差 $\Delta(A,B)=(\operatorname{rescued}(A,B)-\operatorname{harmed}(A,B))/722$。

例：A救回12个而新增4错，差为8/722，超过1pp。这是计算例子，不是结果。

LOO差 $\Delta_{-f}(A,B)=\sum_{i\notin f}(r_i^A-r_i^B)/(722-n_f)$。

例：移除145分子的fold后按剩余577分子计算，不等权平均其余折的准确率。所有阈值用未舍入值。

1024组进入在线prior研究必须同时：两seed各自比补零组与Corrected Frozen均≥1pp；这两个差值在每seed的五个LOO均>0；整批回收及独立核验通过。1pp是资源线、LOO是描述性稳定性检查，不是显著性检验。

只胜补零未胜Frozen，尚无任务升级价值；只胜Frozen未胜补零，不支持增维信息是来源。其余不达标停止此固定构造，不扫宽度/投影/初始化/lr/正则；不据此否定所有高维表示。成功仅支持下一步在线研究，不直接打开投稿gate。

## 7. 留痕与失败处理

准备先config，后特征/initial；保存命令、环境、来源、全部hash、耗时、OR与64复现检查。启动前冻结新commit、输入manifest、10个initial、本文/review hash及唯一OSS目录。

保留20best+10initial checkpoints、4,000epoch记录、2,888OOF、全失败。目录不可覆盖；任一fit失败/超时/非有限/分母或绑定缺失则整批incomplete，不删折算go。回收后独立NumPy复算best两头forward、动作、奖励、配对/LOO/go及epoch日志选型；不声称重执行全部优化。旧证据不改写。

必要工程检查仅覆盖初始化复制/补零恒等、特征复现/折叠、权限和指标。通过即执行冻结批；后续新方法确认须另取未读数据。
