# task-07 批次⑤ 设计意图文档（dh open 30 道 + spare 3 道）

批次：task-07 手写 150 道 · 批次⑤（Task #50，最后一批）；类型4
（discovery_haystack，open 发现类，ADR-0018）；药物 15 + 材料 15，
原地覆盖现役 dh 30 道（旧批 E5 脚本产物，非手写）；lineage=
"handwritten-batch5"。另按 leader 裁定补造 spare_dh_material 3 道
（spare 不对称处置 M13 → 3+3 对称）。机器栏中 visible_set/g_b/
quality_gates 由 task07_batch5_write.py 按五步法**只算不造**回填，
本文档锁定人定选型（族/段/gt/n/d/side/direction/opener/δ/文献）。

## 0. 批级设计决策（人定）与口径记录

### 0.1 leader 两项裁定（2026-08-10，原文要点留痕）
- **spare 方案 A**：补造 3 道 spare_dh_material（3+3 对称）。spare
  的存在目的就是 E6 采样期顶替废道的备用能力；正式 30 道先造、spare
  后造；spare 文件名保持 spare_dh_* 模式（corpus 守卫 a) 单层 glob
  case_*.json 天然排除，150 计数不受影响，落盘后实测 pytest a) 确认）；
  register_dh_gb.py 计数断言 33→36；账本留痕三要素（为何补=E6 采样期
  材料侧无 spare 顶替能力、补什么=spare_dh_material_01~03、查重口径=
  fixed 120+spare 全排除+gt 全换人）。spare 3 道同按手写标准造，质量
  不因"备用"降级。
- **契约字段口径**：材料侧沿用 task-06 判例（grid_key=[family,tc_band,
  element_count] + s_feasible_size=len(g_b)）；药物侧沿用 task-03 契约
  不新增 grid_key（无先例裁定不自造口径）；**全 30 道统一补
  s_feasible_size=len(g_b)**（E6 整数契约，现役药物 15 道无此字段，
  本批覆盖时补齐）；全 30 道 lineage="handwritten-batch5"。

### 0.2 查重与换人策略（保守口径，leader 批准）
- 排除集 = fixed 120（cs 60 + co 60）+ spare 3（drug，从严含）= 123，
  config_pool 枚举期实扫排除（task07_batch5_config_pool.py 调
  active_fixed_and_spare_gts）。
- 现役 dh 30 将被本批覆盖、**不入分母**；但自加纪律：**gt 全换人，
  零沿用旧 dh gt**（枚举器单列旧 dh gt 30 个并标记 OLDGT，选型全避开；
  段成员含旧 gt 者不回避——覆盖后该实体回归普通库成员身份，且 gt 换人
  已是实质防复用）。
- 预检机制化（task-06 经验 §3）：药物侧候选 gt 全做全库同组成普查
  （预筛③，"部分满足窗口"确定性废弃，单列不计判废）；材料侧 UCI 族
  组成级去重、3DSC 主键族单实体，普查一次即过。

### 0.3 容量实测（task07_batch5_capacity_probe.py，已报 leader）
- snapshot 实算 92,153 实体 / 240,467 性质行。
- 材料侧：可用族 28/31，可行配置合计 280（需求 15+3，充裕）。
- 药物侧：药物实体（有 logS）2,272；同 backbone 组（成员≥4）373；
  含可行配置 331 组 / 7,576 配置（需求 15，充裕）。
- 配置池落盘 src/case_build/task07_batch5_config_pool.json：材料 28 族
  / 280 配置；药物过预筛 288 / 预筛③废 2,047；gt 全换人后旧 dh gt
  可沿用数（仅作留痕）材 52 药 6 —— 全部放弃，零沿用。

### 0.4 五步法与预筛（ADR-0018 + task-03/04/06 经验）
①选 gt 放 B → ②同族单调段 n 个放 A → ③混 K=25 干扰项（窗口外确定性
选料，种子=case_id；材料 primary-only、药物同 backbone 全排除防泄漏）
→ ④双质量门（门1=E1 穷举接口 per_case_view.py enumerate 返回 0；
门2=anchor+edit_op+evidence 线索可达）→ ⑤落盘白名单+梯度+G_B。
确定性预筛三条件：①段成员不落窗（δ=0.25，选型期保证）；②门1 过门
概率（干扰项窗口外确定性选料，天然规避）；③同组成普查部分落窗废弃
（药物枚举期 2,047 道剔除）。本批投 30+3 过 30+3，判废 0、预检剔除
单列（药物侧 2,047 预筛③废，材料侧 0——UCI 组成级去重+3DSC 主键唯一）。

### 0.5 δ 口径
全批 δ=0.25 默认值；唯 m13 收窄 δ=0.05（族顶间距 0.12<0.25，task-06
判例口径，§3 m13 窗口按 ≤2.53 落盘）。只收窄不放宽纪律保持，无放宽。

### 0.6 族源与 provenance（换源即换引文纪律，task-06 §2）
- 材料 14 道 UCI 族 → provenance 逐道用 UCI 引文（UCI ML Repository
  #464 unique_m.csv，Hamidieh 2018，DOI 10.24432/C53P47）。
- 材料 1 道 3DSC 族（case_dh_material_14，Cs_xWO_3 钨青铜）→
  provenance 用 3DSC 引文（Sommer et al. 2023，DOI
  10.1038/s41597-023-02721-y），与 task-04 旧 3DSC 道同口径。
- 药物 15 道 → 6 种溶解度数据源/估算文献轮换（provenance 逐道落盘）：
  ESOL（Delaney 2004，DOI 10.1021/ci034243x）/AqSolDB（Sorkun 2019，
  10.1038/s41597-019-0151-1）/Yalkowsky（J Pharm Sci 1980，
  10.1002/jps.2600690814）/Huuskonen（JCICS 2000，10.1021/ci9901338）/
  TDC（Huang et al. arXiv:2102.09548，无 DOI）/Wiki-pS0（Avdeef 2020，
  10.5599/admet.766）；d01~d15 按上列顺序循环轮换（d01=ESOL、
  d02=AqSolDB、…、d07=ESOL 第二轮起）。

### 0.7 M9a opener 规划（算术题不是运气题，task-06 §5）
覆盖后 open 语料 = dh 30 + spare 6 = 36 道。旧 spare 3 道沿用
Identify/Propose/Discover（不动存量）；本批新 33 道（30 正式+3 spare）
引入 7 种全新 opener（Reveal/Unmask/Hunt down/Zero in on/Trace/
Expose/Forecast）+ 复用旧 11 种（Identify/Propose/Discover/Pinpoint/Uncover/
Detect/Locate/Single out/Characterize/Isolate/Unearth），**开头两词互异
18 种 / 36 道 = 0.5 ≫ 0.3 门槛**，为后续扩题留足余量。逐道分配见
§2/§3/§4 opener 栏（批内 33 道：旧 11 种全覆盖 + 新 7 种，余量道复用）。

opener 终版分配表（§2/§3/§4 已同步）：
- 药物 d01~d15：Reveal/Identify/Unmask/Detect/Propose/Discover/Pinpoint/
  Locate/Single out/Characterize/Isolate/Unearth/Uncover/Hunt down/Trace
- 材料 m01~m15：Identify/Unmask/Detect/Propose/Discover/Pinpoint/Locate/
  Single out/Characterize/Isolate/Unearth/Uncover/Reveal/Zero in on/
  Hunt down
- spare sp1/sp2/sp3：Expose/Forecast/Forecast（sp3 换族后 opener 改
  Forecast：Ga 掺 YBCO 氧掺增高 Tc 外推语境，见 §4 换族留痕）。
  互异种数算术：旧 11（含旧 spare 3 道沿用）+ 新 7 = 18 种，
  18/36 = 0.5。

### 0.8 element_count 口径
材料侧 grid_key 第三位锚定 E1 注册口径（互异 [N*] label 数，
build_property_counts.element_count_of），写盘前硬断言 token 数==互异
数（Task #38 加固延续）；药物侧禁用 element_count（任务书口径）。

### 0.9 可见集族泄漏防护（task-04 case_dh_drug_09/14 教训）
干扰池排除以组成判据扫全库：材料侧 same_backbone（组成除至多一个元素
外与 base 一致）全排除 + 写盘断言"可见集内同 backbone 实体数==n"；
药物侧同 backbone 组全排除 + 同组成 gt 实体排除 + 同样写盘断言。

## 1. 文献清单（P1~P13 沿用批①②已 Crossref 核实表；本批新增 P17~P20）

- P1 Ba-La-Cu-O 1986（ZfPhysB, 10.1007/BF01303701）
- P2 YBCO 93K 1987（PRL, 10.1103/PhysRevLett.58.908）
- P3 cuprate 综述 Nature 2015（10.1038/nature14165）
- P4 LaOFeAs JACS 2008（10.1021/ja800073m）
- P5 铁基综述 NatPhys 2010（10.1038/nphys1759）
- P6 CeCu2Si2 PRL 1979（10.1103/PhysRevLett.43.1892）
- P7 heavy-fermion RMP 1984（10.1103/RevModPhys.56.755）
- P8 K3C60 Nature 1991（10.1038/350600a0）
- P9 Chevrel ASM 1990（10.31399/asm.hb.v02.a0001112）
- P10 layered perovskite Nature 1994（10.1038/372532a0）
- P12 MgB2 Nature 2001（10.1038/35065039）
- P13 ML tc npjCompMat 2018（10.1038/s41524-018-0085-8）
- P17 LaFeAsO_{1-x}H_x 氢取代超导体 JACS 2008（10.1021/ja800073m
  同系脉络：H 取代 O 位提升 Tc，见 P4 母体；引文为 P4 母体文献，
  氢化脉络陈述仅限"沿单元素计数轴外推"的数据语义，不涉具体数值）
- P18 MgB2 中 Al 取代压 Tc（Slusky et al. "Loss of superconductivity
  with the addition of Al to MgB2 and a structural transition in
  Mg1-xAlxB2"，**Nature 410:343-345 (2001)，DOI 10.1038/35066528**）。
  【勘误（2026-08-10 三维评审修复轮 Ryan P1-2）】初版误记为 PRL 2001
  10.1103/PhysRevLett.87.087002——该 DOI 实为 Eom et al. 另一篇 MgB2
  PRL，与 Slusky 文无关；已改 Nature DOI 并 Crossref 重验一致
  （Nature 410:343-345, 2001，Slusky/Rogado/Regan et al.）。
- P19 Chevrel 相 M_xMo_6Se_8 化学计量与 Tc 关系综述（沿用 P9 母引文，
  本批 3DSC 族 Mo_6Se_8 未用；Chevrel 表述仅出现在族名注释）
- P20 Cs_xWO_3 钨青铜 Tc 随 Cs 含量非单调（3DSC 引文 DOI
  10.1038/s41597-023-02721-y 数据出处；文献脉络：钨青铜超导在低/高
  掺杂两端出现，本批取低掺侧单调段外推）
- 数据源引文（provenance 字段逐道落盘）：UCI DOI 10.24432/C53P47
  （DataCite 体系，Crossref 不收录，DataCite API 已验）；3DSC DOI
  10.1038/s41597-023-02721-y；药物侧 6 源轮换见 §0.6（均经 Crossref
  重验，2026-08-10 修复轮留痕）。

> 说明：dh open 道的 provenance 只锚定**数据源**（同 task-03/04/06
> 判例），query 不出现真实材料名（匿名域纪律，self_lint REAL_MATERIAL_
> NAMES 拦截）；P17~P20 用于本文档的科学合理性自答留痕。

## 2. 逐道设计 · 药物 15 道（case_dh_drug_01~15，原地覆盖）

> **【终态回填更正（2026-08-10 三维评审修复轮 Ryan P1-1 = Mark W1）】**
> 本节初版为**初选版**，与落盘终选不符。终选经确定性重选产生
> （工具 `src/case_build/task07_batch5_reselect_drug.py`，已上冻结硬闸；
> 回填提交 b9a3576），两处根因：① config_pool 枚举器原版未按变元
> 计数去重（同计数同分异构体当不同梯度点），首轮 13/15 道段内计数
> 重复退化——镜像 task-03 try_build（每 count setdefault 取首）修复后
> 重刷池；② d 口径初判误用位置索引距离，真实口径=**计数轴绝对差**
> （task-03 try_build），初判可保留的 d12/d13 实算 d=3 超界——故全
> 15 道统一重选。本节以下内容为 write.py DRUG_CASES 终选终态逐道
> 回填（gt/窗口/n/d/side/direction/opener/文献锚），尾附更正节。

共性：K=25、δ=0.25、evidence_k=3、g_b 由同组成普查全满足者构成、
lineage=handwritten-batch5、s_feasible_size=len(g_b)。
backbone/var 为匿名元素标签口径（[N*] 标签数字）；文献锚括号内为
该道 provenance 实际数据源（6 源轮换，见 §0.6）。

### d01（case_dh_drug_01）｜opener: Reveal
- 配置：backbone={'19':8,'22':1}，var=16；n=5 d=1 top minimize；
  （side=top：gt 在段顶沿计数升序侧）
  段 z001374(0.62), z003632(0.56), z006799(0.55), z001184(-0.62),
  z018571(-1.28)；anchor=z018571；gt=z001542（logS=-1.97）；窗口
  logS <= -1.72。iso=1 单实体（|g_b|=1）。
- 自答：沿标签 16 计数轴 logS 递减（0.62→-1.28），外推 1 步至
  -1.97 落窗；edit_op=add_atom 标签 16 ×1。
- 文献锚：ESOL（P-数据源，6 源轮换取首）；同系增溶趋势为经验规律，
  题面只给数据。

### d02（case_dh_drug_02）｜opener: Identify
- 配置：backbone={'19':14,'22':4}，var=16；n=5 d=2 bottom maximize；
  段 z003910(-0.36), z002964(-0.73), z011457(-1.2), z018415(-2.35),
  z064701(-3.81)；anchor=z003910；gt=z044151（0.82）；窗口 >= 0.57。
  iso=1。
- 自答：沿标签 16 计数轴 logS 递减，底端反向外推 2 步至 0.82 落窗；
  长段（n=5）d=2 二跳题。
- 文献锚：AqSolDB（6 源轮换）。

### d03（case_dh_drug_03）｜opener: Unmask
- 配置：backbone={'19':16,'22':1}，var=16；n=4 d=1 top minimize；
  段 z061421(-1.65), z018590(-1.85), z003777(-2.69), z010246(-3.47)；
  anchor=z010246；gt=z011114（-4.01）；窗口 <= -3.76。iso=1。
- 自答：沿标签 16 计数轴 logS 递减，外推 1 步至 -4.01 落窗。
- 文献锚：Yalkowsky（6 源轮换）。

### d04（case_dh_drug_04）｜opener: Detect
- 配置：backbone={'19':12}，var=16；n=4 d=2 top minimize；纯标签 19
  单元素 backbone 稀疏梯度；段 z065585(-3.23), z065596(-3.27),
  z001319(-3.38), z043951(-3.57)；anchor=z043951；gt=z010207（-4.89）；
  窗口 <= -4.64。iso=1。
- 自答：沿标签 16 计数轴 logS 递减，外推 2 步至 -4.89 深溶窗；
  d=2 二跳题。
- 文献锚：Huuskonen（6 源轮换）。

### d05（case_dh_drug_05）｜opener: Propose
- 配置：backbone={'19':5,'22':3,'81':1}，var=16；n=4 d=1 bottom
  maximize；段 z003209(0.27), z003491(-0.94), z003478(-1.81),
  z031415(-2.74)；anchor=z003209；gt=z060857（1.04）；窗口 >= 0.79。
  iso=1。
- 自答：沿标签 16 计数轴 logS 递减，底端反向外推 1 步至 1.04 落窗
  （正 logS 高溶窗）。
- 文献锚：TDC（6 源轮换，arXiv 引文无 DOI）。

### d06（case_dh_drug_06）｜opener: Discover
- 配置：backbone={'19':15,'22':3,'81':1}，var=16；n=3 d=2 top
  minimize；段 z037081(0.83), z003139(-1.33), z034534(-4.09)；
  anchor=z034534；gt=z002799（-4.72）；窗口 <= -4.47。iso=1。
  大跨度梯度（段内 4.9 logS 单位）。
- 自答：沿标签 16 计数轴 logS 递减，外推 2 步至 -4.72 落窗。
- 文献锚：Wiki-pS0（6 源轮换）。

### d07（case_dh_drug_07）｜opener: Pinpoint
- 配置：backbone={'19':11,'22':2,'81':1}，var=16；n=3 d=2 bottom
  maximize；段 z003648(0.38), z034716(0.12), z014958(-1.24)；
  anchor=z003648；gt=z000016（0.96）；窗口 >= 0.71。同组成普查
  iso=2 全满足 → g_b 双实体（s_feasible_size=2，多实体判法留痕）。
- 自答：沿标签 16 计数轴 logS 递减，底端反向外推 2 步至 0.96 落窗。
- 文献锚：ESOL（6 源第二轮）。

### d08（case_dh_drug_08）｜opener: Locate
- 配置：backbone={'19':18,'22':1}，var=16；n=3 d=1 bottom maximize；
  段 z021363(-2.7), z010943(-2.77), z062571(-3.01)；anchor=z021363；
  gt=z000019（-2.32）；窗口 >= -2.57。同组成普查落窗 9 实体 →
  |g_b|=9（本批最大可行集留痕）。
- 自答：沿标签 16 计数轴 logS 递增，底端反向外推 1 步至 -2.32 落窗。
- 文献锚：AqSolDB（6 源第二轮）。

### d09（case_dh_drug_09）｜opener: Single out
- 配置：backbone={'16':10,'22':3}，var=19；n=3 d=2 bottom minimize；
  段 z023616(-2.4), z011907(-1.26), z018371(-1.19)；anchor=z023616；
  gt=z012484（-3.12）；窗口 <= -2.87。iso=1。
- 自答：沿标签 19 计数轴 logS 递增，底端反向外推 2 步至 -3.12 落窗。
- 文献锚：Yalkowsky（6 源第二轮）。

### d10（case_dh_drug_10）｜opener: Characterize
- 配置：backbone={'16':6,'22':2}，var=19；n=3 d=2 top maximize；
  段 z062812(-1.77), z062751(-1.33), z006751(-1.05)；anchor=z006751；
  gt=z003921（-0.77）；窗口 >= -1.02。同组成普查落窗 4 实体 →
  |g_b|=4。
- 自答：沿标签 19 计数轴 logS 递增，外推 2 步至 -0.77 落窗。
- 文献锚：Huuskonen（6 源第二轮）。

### d11（case_dh_drug_11）｜opener: Isolate
- 配置：backbone={'16':6,'19':6}，var=22；n=3 d=1 top minimize；
  段 z000309(-0.04), z000013(-0.2), z012259(-1.05)；anchor=z012259；
  gt=z001457（-2.85）；窗口 <= -2.6。iso=1。
- 自答：沿标签 22 计数轴 logS 递减，外推 1 步至 -2.85 落窗。
- 文献锚：TDC（6 源第二轮）。

### d12（case_dh_drug_12）｜opener: Unearth
- 配置：backbone={'19':5,'22':2,'81':1}，var=16；n=4 d=2 bottom
  maximize；段 z045328(-0.07), z000900(-0.84), z011844(-1.02),
  z011833(-2.09)；anchor=z045328；gt=z000871（0.92）；窗口 >= 0.67。
  iso=1。
- 自答：沿标签 16 计数轴 logS 递减，底端反向外推 2 步至 0.92 落窗
  （正 logS 高溶窗）。
- 文献锚：Wiki-pS0（6 源第二轮）。

### d13（case_dh_drug_13）｜opener: Uncover
- 配置：backbone={'16':10,'22':2,'81':1}，var=19；n=3 d=2 top
  minimize；段 z006911(-1.09), z001339(-2.07), z002750(-2.3)；
  anchor=z002750；gt=z002820（-3.45）；窗口 <= -3.2。iso=1。
- 自答：沿标签 19 计数轴 logS 递增，外推 2 步至 -3.45 落窗。
- 文献锚：ESOL（6 源第三轮）。

### d14（case_dh_drug_14）｜opener: Hunt down
- 配置：backbone={'19':9,'22':3,'81':1}，var=16；n=3 d=1 top
  minimize；段 z000873(0.48), z001366(-1.69), z011133(-2.28)；
  anchor=z011133；gt=z019576（-4.36）；窗口 <= -4.11。iso=1。
- 自答：沿标签 16 计数轴 logS 递减，外推 1 步至 -4.36 深溶窗。
- 文献锚：AqSolDB（6 源第三轮）。

### d15（case_dh_drug_15）｜opener: Trace
- 配置：backbone={'19':16,'22':3}，var=16；n=3 d=2 bottom maximize；
  段 z018592(-1.31), z015533(-3.16), z012240(-4.31)；anchor=z018592；
  gt=z003922（0.83）；窗口 >= 0.58。iso=1。opener Trace 为本批新增
  opener 种（§0.7 记账已补）。
- 自答：沿标签 16 计数轴 logS 递减，底端反向外推 2 步至 0.83 落窗。
- 文献锚：Yalkowsky（6 源第三轮）。

**药物侧终态互异核对（实测回填）**：15 组 backbone+var 组互异；gt
15 个互异且零沿用旧 dh gt；direction maximize 7 / minimize 8；
n：3×9、4×4、5×2；d：1×6、2×9；高溶窗（>= 正/近零）7、深溶窗
（<= 负远端）8；|g_b| 分布：1×12、2×1、4×1、9×1。

**重选更正节（两处根因，援引 reselect_drug.py 与 b9a3576）**：初版
§2 为初选配置，重选轮发现两处确定性缺陷（非选型品味问题）：
① **枚举器按变元计数去重缺陷**——config_pool 枚举器原版把同计数
同分异构体当不同梯度点，首轮 13/15 道段内计数重复退化；修复=镜像
task-03 try_build（族成员重建每 count setdefault 取首）重刷池；
② **d 口径=计数轴绝对差**——初判用位置索引距离误判 d12/d13 可保留，
实算 d=3 超界（task-03 try_build 口径：d=min|gt 计数−段计数|）。
全 15 道在修复后 ok 池内由 `task07_batch5_reselect_drug.py` 按方案 A
确定性重选（硬约束：backbone+var 组互异/批内 gt 互异/排除集实扫/
旧 dh gt 零沿用；形态配额 max/min≈8/7 实测 7/8），终选回填 write.py
DRUG_CASES 后全量落盘（提交 b9a3576）；两脚本均已上冻结硬闸
（Daniel N1）。终选结果经 Task #51 复检与三维评审（Ryan 36 道全量
穷举机检）确认零违规。

## 3. 逐道设计 · 材料 15 道（case_dh_material_01~15，原地覆盖）

共性：K=25、δ=0.25（唯 m13 收窄 δ=0.05，见 §0.5）、evidence_k=3、
grid_key=[family,tc_band,
element_count]、s_feasible_size=len(g_b)、lineage=handwritten-batch5、
干扰项 primary-only（Task #20 收窄裁定保持）。family 名=
{stats 变动真实元素}-{匿名 base 组成串}（task-06 口径）。

### m01（case_dh_material_01）｜opener: Identify｜UCI
- 族：Ba2YCu2.95Fe0.05-Ox（Fe 掺杂 YBCO 氧变分，r²=0.998，9 档）；
  n=5 d=1 top maximize；段 c10191(17.8), c10192(26.2), c10193(37.4),
  c10194(43.5), c10195(52.7)；anchor=c10195；gt=c10196（67.6）；
  窗口 tc >= 67.35。铜氧面高 Tc 端外推。
- 自答：氧计数递增 Tc 单调增（17.8→52.7），外推 1 步至 67.6 落窗。
- 文献锚：P2（YBCO 母体）、P3（cuprate 综述）、P13（数据驱动 Tc）。

### m02（case_dh_material_02）｜opener: Unmask｜UCI
- 族：Ba1.45La1.25Ca0.3Cu3-Ox（r²=0.977，8 档）；n=4 d=2 top
  maximize；段 c08107(9.6), c08108(28.3), c08109(46.4), c08110(67.1)；
  anchor=c08110；gt=c08112（75.2）；窗口 >= 74.95。
- 自答：4 点段外推 2 步至 75.2 落窗；d=2 二跳题。
- 文献锚：P1（Ba-La-Cu-O 起点）、P3。

### m03（case_dh_material_03）｜opener: Detect｜UCI
- 族：Ba2CuHg0.9Re0.1-Ox（Hg-1212 Re 掺杂，r²=0.962，5 档）；n=3 d=1
  top maximize；段 c09639(87.2), c09640(90.5), c09641(94.5)；
  anchor=c09641；gt=c09642（95.0）；窗口 >= 94.75。近 100K 窄窗题。
- 自答：高位段外推 1 步至 95.0 落窗（窗口窄、干扰项过门概率低）。
- 文献锚：P3（cuprate 家族）、P13。

### m04（case_dh_material_04）｜opener: Propose｜UCI
- 族：Hf0.05Mo0.95-Cx（A15 型 Mo 碳化物，r²=0.936，6 档）；n=3 d=2
  top maximize；段 c12877(8.81), c12878(9.87), c12886(10.9)；
  anchor=c12886；gt=c12890（12.4）；窗口 >= 12.15。
- 自答：碳计数递增 Tc 增，外推 2 步至 12.4 落窗。
- 文献锚：P13（数据驱动）、P9 系（金属间化合物脉络）。

### m05（case_dh_material_05）｜opener: Discover｜UCI
- 族：Pb-Tlx（Tl 取代 Pb，r²=0.991，6 档）；n=4 d=1 top minimize；
  段 c16892(7.06), c16893(6.98), c16894(6.88), c16895(6.73)；
  anchor=c16895；gt=c16896（6.43）；窗口 <= 6.68。minimize 低 Tc 题。
- 自答：Tl 计数递增 Tc 单调降，外推 1 步至 6.43 落窗。
- 文献锚：P13。

### m06（case_dh_material_06）｜opener: Pinpoint｜UCI
- 族：LaFeAs-Hx（铁基氢取代，r²=0.964，5 档）；n=3 d=1 top maximize；
  段 c06501(27.5), c06510(29.0), c06519(30.5)；anchor=c06519；
  gt=c06523（32.5）；窗口 >= 32.25。
- 自答：氢计数递增 Tc 增（27.5→30.5），外推 1 步至 32.5 落窗。
- 文献锚：P4（LaOFeAs 母体）、P5（铁基综述）、P17（氢取代脉络）。

### m07（case_dh_material_07）｜opener: Locate｜UCI
- 族：Nb0.8Pd0.2-Hx（Laves 相关氢化物，r²=0.959，5 档）；n=3 d=1
  bottom minimize；段 c15698(3.4), c15701(3.46), c15705(3.56)；
  anchor=c15698；gt=c15683（1.52）；窗口 <= 1.77。底端反向外推。
- 自答：氢计数轴 Tc 递增，反向外推 1 步至 1.52 落窗。
- 文献锚：P13。

### m08（case_dh_material_08）｜opener: Single out｜UCI
- 族：Bi2Sr2CaCu2O8.21-Ix（Bi-2212 碘插层，r²=0.980，5 档）；n=3 d=2
  bottom maximize；段 c12341(71.6), c12343(63.1), c12344(56.3)；
  anchor=c12341；gt=c12338（86.6）；窗口 >= 86.35。碘计数递减端
  反向外推高 Tc。
- 自答：碘计数递减 Tc 递减，底端反向外推 2 步至 86.6 落窗。
- 文献锚：P3（BSCCO 家族）、P13。

### m09（case_dh_material_09）｜opener: Characterize｜UCI
- 族：Mg0.815B2-Alx（MgB2 铝掺杂，r²=0.959，4 档）；n=3 d=1 top
  minimize；段 c05894(35.0), c05954(32.7), c05955(32.0)；anchor=c05955；
  gt=c06024（25.7）；窗口 <= 25.95。
- 自答：铝计数递增 Tc 单调降（掺杂压制超导），外推 1 步至 25.7 落窗。
- 文献锚：P12（MgB2 母体）、P18（Al 掺杂压 Tc，本批新增已核 DOI）。

### m10（case_dh_material_10）｜opener: Isolate｜UCI
- 族：HfV2-Hx（Laves 相氢化物，r²=0.990，4 档）；n=3 d=1 top minimize；
  段 c15691(8.42), c15695(7.33), c15714(4.87)；anchor=c15714；
  gt=c15718（2.2）；窗口 <= 2.45。
- 自答：氢计数递增 Tc 递减，外推 1 步至 2.2 落窗。
- 文献锚：P13。

### m11（case_dh_material_11）｜opener: Unearth｜UCI
- 族：Zr2Rh-Hx（r²=0.983，4 档）；n=3 d=1 bottom maximize；
  段 c15680(10.8), c15686(8.8), c15706(6.0)；anchor=c15680；
  gt=c15677（11.1）；窗口 >= 10.85。底端反向外推。
- 自答：氢计数轴 Tc 递减，反向外推 1 步至 11.1 落窗。
- 文献锚：P13。

### m12（case_dh_material_12）｜opener: Uncover｜UCI
- 族：Ca2CsNb3O10-Lix（层状铌酸盐锂插层，r²=0.911，4 档）；n=3 d=1
  top minimize；段 c13503(5.51), c13504(5.4), c13505(5.31)；
  anchor=c13505；gt=c13506（4.71）；窗口 <= 4.96。
- 自答：锂计数递增 Tc 微降，外推 1 步至 4.71 落窗。
- 文献锚：P10（layered perovskite 脉络）、P13。

### m13（case_dh_material_13）｜opener: Reveal｜UCI
- 族：Bi2Se3-Srx（Sr 插层 Bi2Se3，r²=0.931，4 档）；n=3 d=1 bottom
  minimize；段 c12564(2.6), c12565(2.87), c12566(3.06)；anchor=c12564；
  gt=c12563（2.48）；δ=0.05（族顶间距 0.12<0.25，task-06 判例，§0.5），
  窗口 <= 2.53。低 Tc 拓扑材料家族脉络。
- 自答：Sr 计数轴 Tc 递增，反向外推 1 步至 2.48 落窗。
- 文献锚：P13。

### m14（case_dh_material_14）｜opener: Zero in on｜3DSC
- 族：WO3-Csx（铯钨青铜，3DSC 主键族，r²=0.970，5 档）；n=3 d=1 top
  minimize；段 c03293(6.7), c03294(6.0), c03295(4.0)；anchor=c03295；
  gt=c03298（2.0）；窗口 <= 2.25。本批唯一 3DSC 族源道（provenance
  换 3DSC DOI）。
- 自答：Cs 计数递增 Tc 递减，外推 1 步至 2.0 落窗。
- 文献锚：P20（钨青铜脉络）、3DSC 数据源引文。

### m15（case_dh_material_15）｜opener: Hunt down｜UCI
- 族：Ba2Yb0.25Y0.75Cu2.9-Ox（Ba-Yb 掺 YBCO 氧变分，r²=0.920，
  grid_key=['O-[32*]0.4[39*]1.6[65*]3[67*]0.75[73*]0.25',1,6]）；n=4
  d=1 bottom minimize；段 c08272(25.0), c08273(34.0), c08274(66.0),
  c08275(74.0)；anchor=c08272；gt=c08271（tc=22.0）；窗口 <= 22.25。
- **换族留痕（2026-08-10 三维评审修复轮回填）**：初选为 Bi-2212 Pb
  取代族（Bi1-xPbxSr2CaCu2O，段 c11544~c11546），三因换族：① 该族同
  backbone 实体过多，primary-only 干扰池仅 21 个 < K=25，干扰项选不满；
  ② 初选 top 侧 gt=c08275 属旧 dh gt（违反 §0.2 全换人纪律）；③ 换族
  同时取 bottom 侧。新族段成员含旧 dh gt c08275 身份不碍（§0.2：覆盖后
  回归普通库成员，段成员不回避）。
- 自答：沿氧计数轴 Tc 递增（25.0→74.0），底端反向外推 1 步至 22.0
  落窗（氧缺端压 Tc）。
- 文献锚：P2（YBCO 母体系，P2-4 派生口径：掺取代变分族锚定 YBCO
  母体文献而非 Bi-2212 系）、P13。

**材料侧互异核对（终态实测）**：15 族互异（UCI 14 + 3DSC 1）；gt 15
个互异且零沿用旧 dh gt（旧材料 gt c04592/c04551/c03293/c03296/c10199/
c10545/c08114/c12895/c16897/c06524/c15707/c12344/c08275/c07576/c12566
全避开；个别段成员含旧 gt 身份不碍——覆盖后回归普通库成员，且
§0.2 留痕，m15 段含 c08275 即此情形）。方向 maximize 7 / minimize 8；
n：3×12、4×3、5×0；d：1×15、2×0；
科学系统覆盖：铜氧（YBCO-Fe/La-Ba-Cu-O/Hg-1212/Bi-2212-I/Ba-Yb-YBCO）
5、铁基 1、氢化物（NbPd-H/HfV2-H/Zr2Rh-H）3、A15 碳化物 1、Tl-Pb 1、
MgB2 1、层状铌酸盐 1、Bi2Se3 插层 1、钨青铜（3DSC）1。

## 4. 逐道设计 · spare 材料 3 道（spare_dh_material_01~03，新建）

按 leader 裁定：手写标准不降级（设计意图/文献真实/lineage=
handwritten-batch5/visible set 防泄漏全同正式道）。

### sp1（spare_dh_material_01）｜opener: Expose｜UCI
- 族：Ba2YCu3-Hx（YBCO 氢化，r²=0.982，8 档）；n=3 d=2 top minimize；
  段 c10522(86.4), c10529(82.4), c10532(78.8)；anchor=c10532；
  gt=c10537（60.7）；窗口 <= 60.95。
- 自答：氢计数递增 Tc 递减（氢化压制超导），外推 2 步至 60.7 落窗。
- 文献锚：P2（YBCO 母体）、P13。

### sp2（spare_dh_material_02）｜opener: Forecast
- 族：Ba2Cu2.919YZn0.081-Ox（Zn 掺杂 YBCO 氧变分，r²=0.987，5 档）；
  n=3 d=2 top maximize；段 c10065(17.0), c10066(20.0), c10067(24.0)；
  anchor=c10067；gt=c10069（47.4）；窗口 >= 47.15。
- 自答：氧计数递增 Tc 增，外推 2 步至 47.4 落窗。
- 文献锚：P2、P13。

### sp3（spare_dh_material_03）｜opener: Forecast
- 族：Ba2Cu2.8Ga0.2-Ox（Ga 掺 YBCO 氧变分，r²=0.994，
  grid_key=['O-[32*]1.5[33*]1[39*]0.5[65*]2.8[70*]0.2',3,6]）；n=4 d=1
  top maximize；段 c07576(32.0), c07577(38.0), c07578(50.0),
  c07579(56.0)；anchor=c07579；gt=c07580（tc=61.0）；窗口 >= 60.75。
- **换族留痕（2026-08-10 三维评审修复轮回填）**：初选为 Nd-123 Pr
  掺杂氧变分族（Ba2Cu3Nd0.95Pr0.05-Ox，段 c10654~c10657），因该族
  primary 干扰池不足 K=25（同 backbone 实体过多，干扰项选不满）换族；
  opener 同步改 Expose→Forecast（氧掺增高 Tc 外推语境，§0.7 已同步）。
  段成员含旧 dh gt c07576 身份不碍（§0.2）。
- 自答：4 点段沿氧计数轴 Tc 递增，外推 1 步至 61.0 落窗（氧掺增高
  Tc 脉络，Forecast 语境）。
- 文献锚：P2（YBCO 母体系）、P13。

## 5. 判废记录（四数口径）
投造 33（正式 30 + spare 3），过门 33，判废 0，预检剔除单列：
- 药物预筛③（同组成部分落窗）确定性废弃 2,047 配置（枚举期，
  非投造期判废）；
- 材料侧预检剔除 0——数据学原因必须写清（防"0 像放水"，task-06 §3）：
  UCI 26 族为组成级去重数据（同组成取最高 Tc），3DSC 主键族同组成
  唯一，全库同组成普查一次即过；
- 旧 dh gt 沿用统计（仅留痕）：材 52 / 药 6 可沿用配置，全部放弃
  （自加纪律 gt 全换人）。

## 6. 防护对照表（逐条自查）
1. **可见集族泄漏**（task-04 case_dh_drug_09/14 教训）：干扰池全库
   same_backbone 排除 + gt 同组成排除 + 写盘断言"可见集内同 backbone
   实体数==n"、"可见集长==n+K"、"可见集无落窗实体"、"gt 不在可见集"。
2. **同分异构判法**（ADR-0018 §8）：药物侧 gt 组成全库普查，全满足→
   全纳入 g_b（终选 d07 双实体道留痕 iso=2，|g_b|=2）；部分满足→
   预筛③废弃；材料侧 UCI/3DSC 主键族退化单实体判定，resolution
   逐道按实测改写。
3. **δ 只收窄不放宽**：本批 δ=0.25 全批默认，无放宽；唯 m13 收窄
   δ=0.05（族顶间距 0.12<0.25，task-06 判例，§0.5/§3）。
4. **药物 15 道重选更正**（三维评审修复轮）：初版 §2 为初选配置，
   因枚举器按变元计数去重缺陷 + d 口径=计数轴绝对差两处根因，全
   15 道经 `task07_batch5_reselect_drug.py` 确定性重选后回填（详见
   §2 更正节与终态互异核对，提交 b9a3576）。初版 d08/d15 撞车叙述
   随重选作废。
5. **element_count 口径断言**：材料 18 道（正式 15+spare 3）写盘前
   断言 token 数==互异 label 数（Task #38 加固延续）。
6. **provenance 换源纪律**：m14（3DSC）单独用 3DSC DOI，其余材料
   道 UCI DOI，药物道 6 种溶解度数据源轮换（§0.6），逐道核对。
7. **M11 预算句/六字段交付句**：脚本拼装载入 lint 镜像自检
   （self_lint_open），落盘前逐道过。
