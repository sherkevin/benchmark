# task-07 批次② 设计意图文档（cs_material 30 道）

> **本文档先于 case JSON 定稿**（规程四步节奏第 1 步；机器栏 gt/池/格由
> task07_solve.py 回填）。设计意图由工程师（Lee）人定；脚本只算答案，不产生
> 任何约束或题面。生成时间 2026-08-09；依据：手写case操作规程-v1.md、
> 容量探针报告.md（材料 fam≠未知口径 67 可用格）、task07_forbidden_cells.py
> （材料 4 禁入格 = cs_material 04/05/14/15 旧格）、task07_batch2_terrain_probe.py /
> task07_batch2_preflight.py 实测底账（86 候选 → 30 入选）。
>
> **声明（Ray 完整性项）：本批 30 道全部另起设计，不照抄监工审查修正/保留
> 清单；领域名/题面/dk/文献选择均为工程师本批独立手写。**

## 0. 批级设计决策（人定）与口径偏离记录

1. **族口径裁定（本批关键偏离，已 SendMessage 报备 leader）**：任务书"可用族仅五大
   主族"与"5,771 条 / 67 可用格"两数不自洽——terrain_probe 实测对账：
   crystal 总数 17,252；「未知」族 11,481；**5,771 = 17,252 − 11,481（fam≠未知，
   未叠 tc>0）**；叠 tc>0 后 fam≠未知入选 4,007；五大主族白名单 ∧ tc>0 仅 3,743。
   preflight 实测：**五族白名单下 pool≥3 且可落唯一 gt 的空格仅 23 个 < 30，凑不满
   一批**。旁证：①容量探针 67 格即 fam≠未知口径；②4 个材料禁入格全在五族白名单外
   （Carbon/复合族），其存在意义即 fam≠未知口径；③现役 co_material 30 道在用
   Heavy_fermion/Carbon/复合族格。裁定：**按 fam≠未知口径执行**（禁「未知」族硬线
   不变），白名单外扩至 Carbon/Heavy_fermion 等真实族（本批用 Carbon 1、
   Heavy_fermion 2）。
2. **tc>0 硬线**：tc<=0 的 1,764 条在 terrain_probe 断言排除留痕；档0 约束下界
   统一 `tc > 0`（与 common.tc_band 边界严格一致）。
3. **condition 不入题**：实测 condition ≠未知标注 38 条（ambient 37 +
   high_pressure ~96–120 GPa 1），与任务书"16 条"不符（偏离留痕）；且入题会逼出
   "常压"字眼触硬线，本批零使用。
4. **element_count 全批使用**（材料侧推荐）：约束三件套 family/tc/element_count
   全部落 constraints；ec 口径=formula 互异 label 数（与 solve 派生同口径）。
5. **格位来源与避格**：30 格全部为 preflight 实测空格，避开材料 4 禁入格（即时
   断言 18 禁入格全集，Daniel R2）、co_material 30 占格、活跃 gt 63 个
   （co_material 30 + dh_drug 15 + dh_material 15 + spare 3）；dh_material 格第一轴
   =组成串，与族名轴形状互异天然不撞格。
6. **小成员格拼池技术（本批发明）**：五族外扩后仍有多格成员≤2 或整档 tie，采用
   ①tc 窄窗（cs_04 (0,15)、cs_10 [30,40)、cs_24 [8,10)、cs_29 [5,30)）②ec 范围
   拼池（cs_13 ∈[3,5]、cs_19 ∈[6,7]）③tiebreak 方向切换（cs_03/11/13 tc:min）三法
   修复，全部经机器唯一性核；题面同步不泄露窗口内 ec/tc 具体值。
7. **平手规则**：26 道 tc:max（最高转变温度=应用工况裕量最大），4 道 tc:min
   （cs_03/04/11/13，最低转变起点=校准/基线/入口样本语境）；dk 第 4 件措辞与
   solve --tiebreak 参数一致。
8. **候选池**：最小 4（cs_06/20），最大 126（cs_12），全部在 [3,200]，零
   deviation_note。
9. **文献**：16 篇真实文献（P1–P16，DOI 全部经 Crossref API 核实 2026-08-09），
   按族贴题：Cuprate/Ferrite/Heavy_fermion/Chevrel/Carbon/Oxide/Other 各有对口
   文献；provenance 不写作者名（防 KNOWN_REAL_NAMES 擦边）。T45 修复轮新增
   P14–P16 并重挂 cs_14/28/30；cs_27/29 文献与 gt 真实族系无直接对应，降为
   语境参考措辞（Context reference，诚实性修正）。
10. **题面**：30 道互异；开头两词取自 TYPE1_OPENERS 池（本批预扩 25 种至 62）且
    批内 30 种互异（M9a ≥9 达标）；磁体/量子器件/电力传输/制冷语境带专业含义；
    snapshot 为 BackendReader 实算（92,153 实体 / 240,467 性质行）。

## 1. 文献清单（DOI 均经 api.crossref.org 核实，2026-08-09）

| 代号 | 文献 | DOI | 挂题 |
|---|---|---|---|
| P1 | Possible high Tc superconductivity in the Ba-La-Cu-O system, Z. Phys. B (1986) | 10.1007/BF01303701 | 06 |
| P2 | Superconductivity at 93 K in a new mixed-phase Y-Ba-Cu-O compound system, PRL (1987) | 10.1103/PhysRevLett.58.908 | 07/08/11 |
| P3 | From quantum matter to high-temperature superconductivity in copper oxides, Nature (2015) | 10.1038/nature14165 | 09/10/12/13 |
| P4 | Iron-Based Layered Superconductor La[O1-xFx]FeAs (x = 0.05-0.12), JACS (2008) | 10.1021/ja800073m | 15/17/19 |
| P5 | High-temperature superconductivity in iron-based materials, Nat. Phys. (2010) | 10.1038/nphys1759 | 16/18/20 |
| P6 | Superconductivity in the Presence of Strong Pauli Paramagnetism: CeCu2Si2, PRL (1979) | 10.1103/PhysRevLett.43.1892 | 21 |
| P7 | Heavy-fermion systems, Rev. Mod. Phys. (1984) | 10.1103/RevModPhys.56.755 | 22 |
| P8 | Superconductivity at 18 K in potassium-doped C60, Nature (1991) | 10.1038/350600a0 | 01 |
| P9 | Ternary Molybdenum Chalcogenides (Chevrel Phases), ASM Handbook Vol. 2 (1990) | 10.31399/asm.hb.v02.a0001112 | 02/03/04/05 |
| P10 | Superconductivity in a layered perovskite without copper, Nature (1994) | 10.1038/372532a0 | 27/29（语境参考，T45 降级） |
| P11 | Superconductivity in two-dimensional CoO2 layers, Nature (2003) | 10.1038/nature01450 | —（T45 起不再挂题） |
| P12 | Superconductivity at 39 K in magnesium diboride, Nature (2001) | 10.1038/35065039 | 24/26 |
| P13 | Machine learning modeling of superconducting critical temperature, npj Comput. Mater. (2018) | 10.1038/s41524-018-0085-8 | 23/25 |
| P14 | Picosecond superconducting single-photon optical detector, Appl. Phys. Lett. (2001) | 10.1063/1.1388868 | 28（T45 新增，NbN 薄膜对口） |
| P15 | Superconductivity above 130 K in the Hg-Ba-Ca-Cu-O system, Nature (1993) | 10.1038/363056a0 | 14（T45 新增，Hg 系纪录原文） |
| P16 | Superconductivity near 30 K without copper: the Ba0.6K0.4BiO3 perovskite, Nature (1988) | 10.1038/332814a0 | 30（T45 新增，BKBO 四元对口） |

## 2. 逐道设计（30 道）

格式：领域名 ｜约束组合 ｜相容性自答（对材料学家有什么含义）｜题面开头两词 ｜文献。

### 01 二元碳基超导体（分子电子学混合件）
- 约束：family==Carbon ∧ tc∈(0,10) ∧ ec==2；tiebreak tc:max。格 ('Carbon',0,2)，池 12，gt c02647（tc 8.4 K）。
- 自答：碱掺杂 C60 类二元碳基超导体 Tc ~8–18 K 真实存在（P8），与分子电子学
  混合器件低温语境自洽；二元×低 Tc×碳族三约束无矛盾。
- 题面开头：Zero in on the …｜P8。

### 02 二元 Chevrel 相硒化物（无液氦探测器布线）
- 约束：family==Chevrel ∧ tc∈(0,10) ∧ ec==2；tiebreak tc:max。格 ('Chevrel',0,2)，池 10，gt c04867。
- 自答：Chevrel 相（Mo6X8）二元端元低 Tc 真实，无液氦制冷机探测器布线语境合理。
- 题面开头：Mark the …｜P9。

### 03 三元 Chevrel 相（超导测温校准基准）
- 约束：family==Chevrel ∧ tc∈(0,10) ∧ ec==3；tiebreak tc:min。格 ('Chevrel',0,3)，池 32，gt c05412（tc 0.65 K）。
- 自答：超导转变点作固定温标是计量惯例，取族内最低转变起点作校准样本物理合理。
- 题面开头：Confirm the …｜P9。

### 04 四元 Chevrel 相（强场磁体内插可行性）
- 约束：family==Chevrel ∧ tc∈(0,15) ∧ ec==4；tiebreak tc:min。格 ('Chevrel',0,4)，池 5，gt c04332。
- 自答：Chevrel 相四元掺杂调控临界参数是真实研究路线（P9）；窄窗+最低起点=
  可行性基线样本；池 5 为拼池结果（唯一性机器核通过）。
- 题面开头：Choose the …｜P9。

### 05 三元 Chevrel 相（超导电子学材料评审）
- 约束：family==Chevrel ∧ tc∈[10,30) ∧ ec==3；tiebreak tc:max。格 ('Chevrel',1,3)，池 7，gt c04863。
- 自答：Chevrel 相是中温强场经典材料，10–30 K 带与超导电子学器件语境一致。
- 题面开头：Extract the …｜P9。

### 06 低 Tc 铜氧化物（穿透深度校准参考）
- 约束：family==Cuprate ∧ tc∈(0,10) ∧ ec==5；tiebreak tc:max。格 ('Cuprate',0,5)，池 4，gt c00933。
- 自答：gt 实为 B0.04C0.96CuSr2O5 类五元铜掺 Sr 碳酸盐型低 Tc 条目（T45
  勘误：初版误认 La-214 系）；穿透深度基准研究用低 Tc 铜氧化物是惯例
  （P1）。池 4 贴近下限但 ≥3 无需偏离记录。
- 题面开头：Chase down the …｜P1。

### 07 四元铜氧化物（涂层导体缓冲层研究）
- 约束：family==Cuprate ∧ tc∈[10,30) ∧ ec==4；tiebreak tc:max。格 ('Cuprate',1,4)，池 91，gt c03404。
- 自答：YBCO 系缓冲层/界面工程语境下四元铜氧化物中温带候选真实（P2）；大池
  考察检索耐力。
- 题面开头：Fish out the …｜P2。

### 08 四元铜氧化物（制冷机中温电缆筛选）
- 约束：family==Cuprate ∧ tc∈[30,60) ∧ ec==4；tiebreak tc:max。格 ('Cuprate',2,4)，池 53，gt c01889。
- 自答（T45 同步，Kim 重大1）：窗口 [30,60) 全候选 tc<60 K，液氮（77 K）下均
  正常态，液氮场景不相容——语境降级为 cryocooler-based mid-temperature cable
  screening（制冷机直冷中温电缆筛选），dk4 平手理由中性化为 "largest headroom
  above the screening window's lower bound"（P2）。
- 题面开头：Settle on the …｜P2。

### 09 五元铜氧化物（故障限流器线圈）
- 约束：family==Cuprate ∧ tc∈[30,60) ∧ ec==5；tiebreak tc:max。格 ('Cuprate',2,5)，池 47，gt c01962。
- 自答：故障限流器用中温铜氧化物是真实工程方向（P3）；五元掺杂组分现实。
- 题面开头：Home in on the …｜P3。

### 10 六元铜氧化物（各向异性测量）
- 约束：family==Cuprate ∧ tc∈[30,40) ∧ ec==6；tiebreak tc:max。格 ('Cuprate',2,6)，池 17，gt c02703。
- 自答：各向异性测量需多组分体系对比，六元铜氧化物真实；窄窗 [30,40) 为拼池
  收窄（整档 tie，唯一性机器核通过）。
- 题面开头：Lock onto the …｜P3。

### 11 四元铜氧化物（高温带低温恒温器演示，下限样本）
- 约束：family==Cuprate ∧ tc∈[60,100) ∧ ec==4；tiebreak tc:min。格 ('Cuprate',3,4)，池 56，gt c01896（tc 62 K）。
- 自答（T45 同步，Kim 提示7）：60–100 K 为高温超导窗口；题面/领域名已去
  "liquid-nitrogen" 字眼，改为 high-temperature-window cryostat demonstrator
  中性表述（P2），方向切换解决整档 tie。
- 题面开头：Unveil the …｜P2。

### 12 五元铜氧化物（强场磁体线圈）
- 约束：family==Cuprate ∧ tc∈[60,100) ∧ ec==5；tiebreak tc:max。格 ('Cuprate',3,5)，池 126，gt c01822（tc 98 K）。
- 自答：高场磁体用近 100 K 铜氧化物是真实路线（P3）；最大池 126 考察耐力。
- 题面开头：Handpick the …｜P3。

### 13 铜氧化物（纪录 Tc 带入口样本）
- 约束：family==Cuprate ∧ tc>=100 ∧ ec∈[3,5]；tiebreak tc:min。格 ('Cuprate',4,5)，池 14，gt c01821（tc 100 K）。
- 自答：tc≥100 K 纪录带（Hg/Y 系高压纪录语境，P3）取入口样本=带下界；
  ec∈[3,5] 拼池修复 tie（唯一性机器核通过），题面以 "moderate compositional
  complexity" 表述不泄 ec 数值。
- 题面开头：Shortlist the …｜P3。

### 14 六元铜氧化物（强场内插可行性）
- 约束：family==Cuprate ∧ tc>=100 ∧ ec==6；tiebreak tc:max。格 ('Cuprate',4,6)，池 7，gt c01825（tc 132.9 K）。
- 自答（T45 文献重挂）：gt 为六元 Hg 系铜氧化物（tc 132.9 K），文献换 Hg 系
  纪录原文 P15（Superconductivity above 130 K in the Hg-Ba-Ca-Cu-O system）；
  强场内插可行性语境合理。
- 题面开头：Secure the …｜P15。

### 15 二元铁基（低温存储元件研究）
- 约束：family==Ferrite ∧ tc∈(0,10) ∧ ec==2；tiebreak tc:max。格 ('Ferrite',0,2)，池 12，gt c03762。
- 自答：二元铁基（FeSe 型 11 系）低 Tc 真实存在（P4 系铁基发现脉络）；低温
  存储元件语境自洽。
- 题面开头：Procure the …｜P4。

### 16 四元铁基（超导自旋阀试验）
- 约束：family==Ferrite ∧ tc∈(0,10) ∧ ec==4；tiebreak tc:max。格 ('Ferrite',0,4)，池 47，gt c00446。
- 自答：铁基/铁磁近邻效应自旋阀是真实研究方向（P5）；四元 122 系低 Tc 变体现实。
- 题面开头：Source the …｜P5。

### 17 四元铁基（紧凑型制冷机磁体）
- 约束：family==Ferrite ∧ tc∈[10,30) ∧ ec==4；tiebreak tc:max。格 ('Ferrite',1,4)，池 81，gt c03710。
- 自答：铁基 122 系 10–30 K 带与制冷机直冷磁体设计语境一致（P4）。
- 题面开头：Certify the …｜P4。

### 18 五元铁基（持续电流接头测试台架）
- 约束：family==Ferrite ∧ tc∈[10,30) ∧ ec==5；tiebreak tc:max。格 ('Ferrite',1,5)，池 53，gt c00807。
- 自答：持续电流接头是磁体工程真实部件；五元铁基（1111 系，P5）中温带现实。
- 题面开头：Validate the …｜P5。

### 19 六元铁基（多阳离子掺杂系列）
- 约束：family==Ferrite ∧ tc∈[10,30) ∧ ec∈[6,7]；tiebreak tc:max。格 ('Ferrite',1,6)，池 5，gt c00801。
- 自答：铁基多阳离子共掺杂是真实材料探索方式（P4）；ec∈[6,7] 拼池修复小成员
  格（唯一性机器核通过）。T45（Kim 提示5）：初版题面 "a six-element" 自泄
  ec（池含七元候选），已改为 "a ferrite entry with the highest transition
  temperature among qualifying six- and seven-element candidates"（消歧措辞
  并入）。
- 题面开头：Document the …｜P4。

### 20 六元铁基（中温电力电缆筛选）
- 约束：family==Ferrite ∧ tc∈[30,60) ∧ ec==6；tiebreak tc:max。格 ('Ferrite',2,6)，池 4，gt c00507。
- 自答：铁基带材电缆是中温超导电力真实候选（P5）；池 4 ≥3 无需偏离记录。
- 题面开头：Register the …｜P5。

### 21 二元重费米子（量子临界研究）
- 约束：family==Heavy_fermion ∧ tc∈(0,10) ∧ ec==2；tiebreak tc:max。格 ('Heavy_fermion',0,2)，池 29，gt c03052。
- 自答：重费米子超导 Tc 均在个位数 K（P6 CeCu2Si2 脉络）；量子临界点研究用
  二元 Ce/ U 系化合物真实。
- 题面开头：Profile the …｜P6。

### 22 四元重费米子（非传统配对普查）
- 约束：family==Heavy_fermion ∧ tc∈(0,10) ∧ ec==4；tiebreak tc:max。格 ('Heavy_fermion',0,4)，池 95，gt c03170。
- 自答：重费米子是非传统配对主平台（P7 Stewart 综述）；四元 115 系等现实，
  大池 95 考察耐力。
- 题面开头：Audit the …｜P7。

### 23 单质低温超导体（量子传感器线圈）
- 约束：family==Other ∧ tc∈(0,10) ∧ ec==1；tiebreak tc:max。格 ('Other',0,1)，池 42，gt c04957。
- 自答：单质超导体（Nb Tc≈9.2 K 等）是超导史起点，量子传感线圈语境真实（P13
  数据库/机器学习综述脉络）。ec==1 考察组成复杂度下限判读。
- 题面开头：Survey the …｜P13。

### 24 二元金属间化合物（加速器腔薄膜）
- 约束：family==Other ∧ tc∈[8,10) ∧ ec==2；tiebreak tc:max。格 ('Other',0,2)，池 56，gt c04741。
- 自答（T45 勘误，Kim 提示6）：gt 实为 LuTc2 Laves 相二元金属间化合物
  （tc≈9.9 K），初版误认 A15/二硼化物；加速腔薄膜普查语境保留（P12 语境
  脉络）；窄窗 [8,10) 拼池收窄（唯一性机器核通过）。
- 题面开头：Scan for the …｜P12。

### 25 四元金属间化合物（超导金属间化合物数据库普查）
- 约束：family==Other ∧ tc∈[10,30) ∧ ec==4；tiebreak tc:max。格 ('Other',1,4)，池 24，gt c03079。
- 自答（T45 同步，Kim 提示7）：SRF 短名单宣称弱化为 database survey 语境
  （领域名/题面均改 superconducting-intermetallic database survey 口径，P13
  数据库普查脉络）；四元金属间化合物中温带现实。
- 题面开头：Sift for the …｜P13。

### 26 二元化合物（紧凑型超导引线演示）
- 约束：family==Other ∧ tc∈[30,60) ∧ ec==2；tiebreak tc:max。格 ('Other',2,2)，池 6，gt c01369（tc 38.4 K）。
- 自答：MgB2（39 K，二元）引线/电流引应用是真实产品方向（P12）；池 6 偏小
  但 ≥3 无需偏离记录。
- 题面开头：Probe for the …｜P12。

### 27 二元氧化物（超导隧道势垒材料）
- 约束：family==Oxide ∧ tc∈(0,10) ∧ ec==2；tiebreak tc:max。格 ('Oxide',0,2)，池 9，gt c04355。
- 自答（T45 文献降级）：P10（无铜层状钙钛矿）与本道 gt 真实族系无直接对应，
  provenance 降为语境参考措辞（Context reference，诚实性修正）；低 Tc 二元
  氧化物隧道势垒语境保留。
- 题面开头：Announce the …｜P10。

### 28 三元 Oxide 族条目（中温超导普查，去化学化表述）
- 约束：family==Oxide ∧ tc∈[10,30) ∧ ec==3；tiebreak tc:max。格 ('Oxide',1,3)，池 13，gt c04889。
- 自答（T45 纠错，Kim 重大2）：gt 实为 NbN0.98O0.02（氮化物，数据库归 Oxide
  族标签）——初版"三元氧化物/氧化物电子学集成"为误叙。题面/领域名去化学化，
  按数据库标签中性表述 "a ternary entry of the Oxide family"（与 dk1 对齐），
  删 oxide-electronics integration 应用断言；文献换氮化物超导对口真实文献
  P14（NbN 薄膜单光子探测器，DOI Crossref 核实）。
- 题面开头：Detect the …｜P14。

### 29 五元氧化物（异质结近邻效应研究）
- 约束：family==Oxide ∧ tc∈[5,30) ∧ ec==5；tiebreak tc:max。格 ('Oxide',1,5)，池 13，gt c02175。
- 自答（T45 文献降级）：氧化物异质结近邻超导语境保留；P10 与 gt 真实族系无
  直接对应，降为语境参考措辞；[5,30) 跨档拼池修复小成员格（唯一性机器核
  通过）。
- 题面开头：Spot the …｜P10。

### 30 四元氧化物（氧化物多层堆栈设计）
- 约束：family==Oxide ∧ tc∈[30,60) ∧ ec==4；tiebreak tc:max。格 ('Oxide',2,4)，池 7，gt c01633。
- 自答（T45 文献重挂）：gt 为四元 Ba-K-Bi-O 钙钛矿（tc 31.5 K），文献换对口
  原文 P16（Ba0.6K0.4BiO3 无铜钙钛矿 ~30 K）；氧化物多层堆栈中温带候选现实。
- 题面开头：Pinpoint the …｜P16。

## 3. 判废与回调记录（实时维护）

| 轮次 | 事件 | 处置 |
|---|---|---|
| terrain | 任务书三数对账：5,771=fam≠未知未叠 tc；4,007=fam≠未知∧tc>0；3,743=五族∧tc>0；condition 38 条≠任务书 16 条 | 口径裁定 fam≠未知（§0.1/§0.3 留痕，SendMessage 报备） |
| preflight 轮1 | 23 个无冲突候选（五族白名单口径），缺口 7 | 补格：Heavy_fermion/Carbon 外扩 + tie 格窗口收窄 + 小成员格拼池 |
| preflight 轮2-5 | 86 候选（M01–M79）：30 入选；其余判废（撞 co 占格/已选格/tie 双方向不唯一/池<3/gt 撞活跃） | 判废名单与 issues 精确留痕于 task07_batch2_ledger.json preflight_accounting |
| 落盘轮1 | CASES 初版把判废组合误排入 cs_07/09；cs_13 structural ec==5 tie 不唯一；cs_27 题面重词 | CASES 表重写对齐 chosen 集；cs_13 改 ec∈[3,5] 拼池并删题面 ec 提示；cs_27 改词 |
| 落盘轮2 | snapshot 模板字符串含一处多余空格（"全载） ，"） | 模板修正后全量重刷落盘，solve 复验 30/30、lint strict 复跑 30/30 零违规 |
| T45 修复轮 | 三维评审修复（Kim 重大3+提示/Paul/Ray）：cs_08 液氮降级中温制冷机语境；cs_28 去化学化+P14；ferrite 消歧×6（dk1+query 首现）；cs_19 自泄 ec 修正；cs_14→P15/cs_30→P16/cs_27/29→P10 语境降级；cs_11/25 弱化 | 仅动 domain/query/dk/provenance 文字；重跑 solve 机器栏 30/30 与修复前快照（/tmp/b2_machine_snapshot.json）逐字零变化；lint strict 30/30 violations==0；cs 全族 60 道合跑 freeze M13 零命中（drug=30/material=30 对称） |

## 4. 分布概览

- tc 档：档0×11、档1×9、档2×5、档3×2、档4×2（数据现实：档3/4 仅 Cuprate 有
  可行空格，如实留痕）。
- 族别：Carbon 1 / Chevrel 4 / Cuprate 9 / Ferrite 6 / Heavy_fermion 2 /
  Other 4 / Oxide 4。
- tiebreak：tc:max 26 / tc:min 4（03/04/11/13）。
- opener：30 种批内互异（Zero in on / Mark / Confirm / Choose / Extract /
  Chase down / Fish out / Settle on / Home in on / Lock onto / Unveil /
  Handpick / Shortlist / Secure / Procure / Source / Certify / Validate /
  Document / Register / Profile / Audit / Survey / Scan for / Sift for /
  Probe for / Announce / Detect / Spot / Pinpoint）。
