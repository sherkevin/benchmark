# task-07 批次④ 设计意图文档（co_material 30 道）

批次：task-07 手写 150 道 · 批次④（Task #48）；类型2
（constrained_optimization，目标量一律 tc:max）；材料侧口径沿用批②
（fam≠未知批级裁定；tc>0 硬线；element_count 推荐用）；
grid_key=[family, tc_band, element_count]。
lineage="handwritten-batch4"。机器栏（gt/池/格）由 task07_solve.py
回填，本文档不预填。

## 0. 批级设计决策（人定）与口径偏离记录

### 0.1 目标量口径（owner 拍定）
query 公共骨架 Scoring 句 "(higher is better)" 固定（批中禁改共享模块），
故本批 30 道目标量**全部 tc:max**（tc:min 与固定措辞冲突，全批禁用）。
dk/query/objectives 三处严格一致。

### 0.2 查重与换人策略
- 活跃 gt 排除集 123（cs 60 + co_drug 30 + dh 30 + spare 3，从严含
  spare；terrain_probe/preflight 实扫断言）。
- cs_material 30 活跃占格全禁入（实扫，与旧 co 30 格零重合）。
- 旧 co_material 30 不入分母，但沿用批②"旧格换人"策略优先：旧批 30 道
  gt 全是各格 tc 最大值（格内最优设计），直接整档 tc:max 必撞旧 gt，
  故旧格一律收窄 tc 窗（窗顶 ≤ 旧 gt tc，窗内最大唯一、池 5~200、gt
  落原档保 grid_key 不变）。

### 0.3 容量终核（28+2，leader 2026-08-10 追认）
三重探针全量穷举（task07_batch4_terrain_probe / task07_batch4_swap_windows
/ task07_batch4_full_terrain + 末轮跨 ec 拼池探针）：
- snapshot 实算 92,153 实体 / 240,467 性质行；合格实体 4,007 / 73 格
  （crystal ∧ tc>0 ∧ fam≠未知；tc<=0 排除 1,764、未知族排除 11,481 留痕）。
- 池≥5 可行格穷举 = **28 个**：旧格换人 23 + 旧格窄窗 2
  （Other(1,2)/(1,3)）+ 新格 3（Carbon(0,1)、Heavy_fermion|Chevrel(0,4)、
  Other(0,5)）。
- 缺口根因：材料侧池≥5 的 49 格中 30 个是 cs 占格（禁入）、其余几乎全
  是旧 co 格（换人后 23 个可行），新格仅 3 个池≥5；跨 ec 拼池、坏并列
  下潜窗等批②全部技巧用尽（Cuprate ec∈[3,4] 拼池 gt 落 cs 占格被禁）。
- 末轮计数更正：首报 leader "29 道池≥5"系把 Other(1,3) 双窗重复计数，
  更正为 28（2026-08-10 追认消息留痕）。
- **末轮禁入格补位更正（2026-08-10 落盘自检发现）**：上述"新格 3"中
  Carbon(0,1) 与 Heavy_fermion|Chevrel(0,4) 恰属 18 禁入格全集
  （FORBIDDEN_CELLS_MATERIAL，前三重探针漏查此表）；补位探针
  （task07_batch4_replacement_probe，切片法穷举+禁入/cs/批内/活跃四重
  过滤）得 10 个可行补位格，定型三处替换：
  - S01 → Carbon ∧ tc∈[0.152,3.15) ∧ ec∈[1,3]，格 Carbon(0,3)，池 5；
  - S02 原 Carbon(0,3) 与补位后 S01 同格，且 Carbon 档 0 三格全灭
    （(0,1)禁入/(0,2)cs 占/(0,3)归 S01）、档 1 仅 1 成员 → 换族补位
    Oxide ∧ tc∈[0.47,0.7) ∧ ec∈[1,3]，格 Oxide(0,1)，池 5；
  - S07 → Heavy_fermion ∧ tc∈[0.23,0.85) ∧ ec∈[1,2]，格
    Heavy_fermion(0,1)，池 5（文献由 P9 改 P7，gt 即重费米子族）。
  替换后 **28+2 总数不变**（补位格均池≥5 无新增偏离），偏离道仍仅
  S39/S40 两道；已报 leader（2026-08-10）。
- **28+2 方案**：2 道池 3 偏离道（S39/S40，deviation_note 三要素齐全，
  见 §2 第 29/30 道），批内 30 格互异不变；不硬凑、不降池下限、不碰
  未知族。

### 0.4 偏离表（本批共 2 道）
| case | 偏离项 | 实际值 | 三要素摘要 |
|---|---|---|---|
| 29 (S39) | 类型2 池下限 5 | 池 3 | Cuprate 42–45K ec==3 中温铜氧筛选，放宽任一维度即离开该科学问题 |
| 30 (S40) | 类型2 池下限 5 | 池 3 | Ferrite 七元铁基 10–24.1K 档，七元铁基天然稀疏，放宽即离开问题本身 |

### 0.5 TYPE2_OPENERS 占用
现池 68 种；批③ co_drug 用 38~67；本批用 0~29（批内 opener_idx 30 种
互异；M9a 开头两词 29 种/30 道=0.967——池 idx 10/11 两 opener 开头两词
同为 "out of"，池本身含一对两词重复，门禁 ≥0.3 不受影响）。

## 1. 文献清单（沿用批②已 Crossref 核实表 + 本批新增 3 篇：P14m–P16m（SSPD/Hg-130K/BaKBiO₃），DOI 经 Crossref 核实）
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
- P14m SSPD APL 2001（10.1063/1.1388868，本批新增）
- P15m Hg 系 130K Nature 1993（10.1038/363056a0，本批新增）
- P16m BaKBiO3 Nature 1988（10.1038/332814a0，本批新增）

编号说明（修复轮 W1）：新增 3 篇用 m 后缀编号，避免与批③药学
P14~P16 同号文献撞车（P17~P19 亦已被药学占用，不可续号）。

语境参考（语境参考措辞，非直接来源；批②教训2 诚实性口径）：
case 01（Carbon 毫开选材挂 P8 碱金属掺杂富勒烯文献）、case 02
（Oxide 亚 1K 参考样挂 P10 无铜层状钙钛矿文献）、case 08/09/19/20/21
（Other 族常规超导体挂 P13/P14m 数据库与应用文献）、case 10/11/12/13
（Oxide/Other 低 Tc 挂 P10/P13/P16m 族级语境）。

## 2. 逐道设计（30 道）

格式：领域名 ｜约束组合（family ∧ tc 窗 ∧ element_count）｜§3.2 相容性
自答（符合这组条件的材料现实中什么样、用在什么场景）｜题面草案
（opener 索引 + domain_plural，build_type2_query 组装）｜文献。
case 编号按 tc 档升序、档内族名字母序。

### 01 碳基候选的毫开量子器件布线筛选（label S01，补位）
- 约束：family==Carbon ∧ tc∈[0.152,3.15) ∧ ec∈[1,3]；目标 tc:max。
  格 Carbon(0,3)（原设计 Carbon(0,1) 属 18 禁入格，补位探针换格）。
- 自答：碳基超导体（碱金属掺杂富勒烯/石墨插层一脉）Tc 亚 1K~3K 档
  正是毫开稀释制冷平台上量子器件布线与谐振器的现实选材区；1~3 元
  组成在该温区真实存在。
- 题面：opener 0｜carbon-based superconductor candidates for a
  millikelvin quantum-device wiring study。｜P8（语境参考）。
### 02 氧化物候选的亚 1K 参考样标定（label S02，补位换族）
- 约束：family==Oxide ∧ tc∈[0.47,0.7) ∧ ec∈[1,3]；目标 tc:max。
  格 Oxide(0,1)（原设计 Carbon(0,3) 与补位后 S01 同格，Carbon 档 0
  三格全灭、档 1 仅 1 成员，故换族补位）。
- 自答：亚 1K Tc 的氧化物族成员正是稀释制冷机亚开尔文温标定的参考样
  现实画像；叙事温区"sub-kelvin"落于窗内（批②教训1）。
  【修复轮勘误 W3/W4】gt 解码实为 O₂ 单质（库内 Oxide 族标签的上游
  分类 artifact，非无铜层状钙钛矿）；题面相应改 "Oxide-family
  candidates" 与 dk 数据库标签口径对齐，不断言具体化学类别。
- 题面：opener 1｜Oxide-family candidates kept as sub-kelvin reference
  samples for a dilution-refrigerator calibration campaign。｜P10（语境参考）。
### 03 四元铜氧候选的亚 10K 基础研究参照（label S03）
- 约束：family==Cuprate ∧ tc∈[9.2,9.7) ∧ ec==4；目标 tc:max。
- 自答：铜氧家族低 Tc 端（掺杂量偏低/结构畸变成员）落在 9K 上缘，
  用作低温穿透深度与机理研究的参照样现实成立。
- 题面：opener 2｜quaternary cuprate candidates kept as sub-10 K
  reference materials。｜P3。
### 04 三元铁基候选的毫开物性测量选型（label S04）
- 约束：family==Ferrite ∧ tc∈[7.28,9.816666665) ∧ ec==3；目标 tc:max。
- 自答：三元铁基（FeSe 一脉）Tc 8~10K 档是毫开平台物性测量的常见
  对象；铁基族标签指铁基超导体（dk 带消歧注）。
- 题面：opener 3｜ternary iron-based (Ferrite-family) candidates for a
  millikelvin property-measurement campaign。｜P5。
### 05 五元铁基候选的低温磁体预筛（label S05）
- 约束：family==Ferrite ∧ tc∈[7.8,9.6) ∧ ec==5；目标 tc:max。
- 自答：五元铁基（1111/122 掺杂衍生系）在 8~10K 档存在真实成员，
  对应液氦温区小型磁体预筛场景。
- 题面：opener 4｜five-element iron-based candidates pre-screened for a
  liquid-helium magnet study。｜P5。
### 06 三元重费米子候选的强关联参照选型（label S06）
- 约束：family==Heavy_fermion ∧ tc∈[6.55,7.876) ∧ ec==3；目标 tc:max。
- 自答：重费米子三元系（Ce/基化合物一脉）Tc 数 K 级正是强关联参考
  样与量热标定的现实选材。
- 题面：opener 5｜ternary heavy-fermion candidates compiled for a
  strongly-correlated reference study。｜P7。
### 07 低组元重费米子候选的亚 1K 测温标定（label S07，补位）
- 约束：family==Heavy_fermion ∧ tc∈[0.23,0.85) ∧ ec∈[1,2]；目标 tc:max。
  格 Heavy_fermion(0,1)（原设计 Heavy_fermion|Chevrel(0,4) 属 18 禁入
  格，补位探针换格）。
- 自答：低组元重费米子族成员（含单质端）亚 1K Tc 正是稀释制冷机
  平台上超导测温标定的现实参照；文献 P7（heavy-fermion RMP）与 gt 族
  标签直接对应（较原 P9 更切题）。
  【修复轮勘误 M1/W3】gt 解码实为纯 U 单质（ec=1）：原措辞 "binary"
  与约束 ec∈[1,2] 和 gt 均矛盾，题面改 "one-to-two-element"（仿
  case 13/30 范围措辞）；原自答 "Ce 系二元化合物一脉" 同步改为
  "含单质的低组元重费米子族成员"。
- 题面：opener 6｜one-to-two-element heavy-fermion candidates for a
  sub-kelvin thermometry calibration on a dilution-refrigerator
  platform。｜P7。
### 08 三元 Other 族候选的 9.9K 档低温导线选型（label S08）
- 约束：family==Other ∧ tc∈[9.9,9.96) ∧ ec==3；目标 tc:max。
- 自答：Other 族 9.9K 上缘三元是 4.2K 液氦平台导线选型的真实画像。
  【修复轮勘误 W3】gt 解码实为 Hf₀.₆Zr₀.₄V₂（Laves 相），非 Nb-Ti
  衍生；自答去掉具体化学系断言，只保留族级画像。
- 题面：opener 7｜ternary Other-family candidates for a 4.2 K-platform
  cryogenic wiring selection。｜P14m（语境参考）。
### 09 四元 Other 族候选的低温屏蔽层选材（label S09）
- 约束：family==Other ∧ tc∈[8.74,9.659) ∧ ec==4；目标 tc:max。
- 自答：四元常规超导体 9K 档对应低温磁屏蔽与衬层材料现实场景。
- 题面：opener 8｜quaternary Other-family candidates for a cryogenic
  magnetic-shield material review。｜P13（语境参考）。
### 10 五元 Other 族候选的窄带上缘筛选（label S10）
- 约束：family==Other ∧ tc∈[9.535,9.95) ∧ ec==5；目标 tc:max。
- 自答：五元 Other 族在 9.5~9.95K 窄带的成员对应多组元常规超导体的
  低温工程筛选，现实成立。
- 题面：opener 9｜five-element Other-family candidates inside a narrow
  upper-band window。｜P13（语境参考）。
### 11 三元氧化物候选的液氦平台器件选材（label S11）
- 约束：family==Oxide ∧ tc∈[8.5,9.9) ∧ ec==3；目标 tc:max。
- 自答：三元氧化物超导体 9K 档（非铜氧化物一脉）用于液氦平台器件
  与异质结研究现实存在。
- 题面：opener 10｜ternary oxide candidates for a liquid-helium-platform
  device study。｜P10（语境参考）。
### 12 五元氧化物候选的低温传感选型（label S12）
- 约束：family==Oxide ∧ tc∈[6.1,9.5) ∧ ec==5；目标 tc:max。
- 自答：五元氧化物在 6~9.5K 档对应低温超导传感材料的真实候选群。
- 题面：opener 11｜five-element oxide candidates for a cryogenic sensor
  material shortlist。｜P10（语境参考）。
### 13 五/六元氧化物候选的极低温参考样（label S13）
- 约束：family==Oxide ∧ tc∈[2.5,3.11) ∧ ec∈[5,6]；目标 tc:max。
- 自答：多组元氧化物低 Tc 端（3K 上下）是极低温参考样与相变研究的
  现实样本。
- 题面：opener 12｜five-to-six-element oxide candidates kept as
  low-temperature (few-kelvin) reference samples。｜P16m（语境参考）。
  【修复轮措辞 W5】2.5–3.1 K 属液氦泵冷温区，"ultra-low-temperature"
  改 "low-temperature (few-kelvin)"。
### 14 三/四元 Chevrel 候选的 10~12K 制冷机直冷选型（label S14）
- 约束：family==Chevrel ∧ tc∈[10.2,12.3) ∧ ec∈[3,4]；目标 tc:max。
- 自答：Chevrel 相（三元/四元硫族钼酸盐）Tc 10~12K 档正是 GM 制冷机
  直冷磁体与电子学的现实温区。
- 题面：opener 13｜ternary-to-quaternary Chevrel-phase candidates for a
  cryocooler-direct-cooled selection。｜P9。
### 15 三/四元铜氧候选的 28~30K 制冷机温区选型（label S15）
- 约束：family==Cuprate ∧ tc∈[28.5,29.2) ∧ ec∈[3,4]；目标 tc:max。
- 自答：铜氧家族 29K 上缘成员正是制冷机温区磁体示范的历史原型
  温区材料。
  【修复轮勘误 W3】gt 解码实为 La₂CuO₄₊δ（氧过量掺杂，无 Ba），原
  "La-Ba-Cu-O 一脉" 表述勘误。
  【修复轮引文 W2】文献由 P1（Ba-La-Cu-O 专指）改挂 P3（铜氧综述，
  族级对应无化学系冲突）。
- 题面：opener 14｜ternary-to-quaternary cuprate candidates at the upper
  end of the cryocooler window。｜P3。
### 16 五元铜氧候选的 27~29K 档磁体预筛（label S16）
- 约束：family==Cuprate ∧ tc∈[27.0,28.6) ∧ ec==5；目标 tc:max。
- 自答：五元铜氧 27~29K 档成员对应中温磁体预筛，现实由掺杂铜氧系
  支撑。
- 题面：opener 15｜five-element cuprate candidates pre-screened for a
  mid-temperature magnet build。｜P3。
  【修复轮引文 W2】同 case 15，P1 改挂 P3（铜氧综述族级对应）。
### 17 六元铜氧候选的 25~30K 各向异性研究选型（label S17）
- 约束：family==Cuprate ∧ tc∈[25.05,29.8) ∧ ec==6；目标 tc:max。
- 自答：六元铜氧（多层钙钛矿衍生）25~30K 档是各向异性测量的真实
  对象群。
- 题面：opener 16｜six-element cuprate candidates for an anisotropy
  measurement campaign。｜P3。
### 18 三元铁基候选的 26K 档制冷机磁体选型（label S18）
- 约束：family==Ferrite ∧ tc∈[15.5,29.5) ∧ ec==3；目标 tc:max。
- 自答：三元铁基 26K（LaOFeAs 一脉的发现温区）对应制冷机直冷磁体的
  现实选材，铁基族标签消歧入 dk。
- 题面：opener 17｜ternary iron-based candidates for a cryocooler-cooled
  magnet selection。｜P4。
### 19 二元 Other 族候选的 10~20K 中温导线筛选（label S19）
- 约束：family==Other ∧ tc∈[10,20.2) ∧ ec==2；目标 tc:max。
- 自答：二元常规超导体（A15/Nb3Sn 一脉，Tc 18~20K）是 10~20K 制冷机
  温区导线的经典现实选材。
- 题面：opener 18｜binary Other-family candidates for a 10-20 K
  cryocooler-stage conductor screening。｜P13（语境参考）。
### 20 三元 Other 族候选的 10~18K 电子学选材（label S20）
- 约束：family==Other ∧ tc∈[10,18) ∧ ec==3；目标 tc:max。
- 自答：三元常规超导体 10~18K 档对应超导电子学与滤波器件的现实
  制冷机温区。
- 题面：opener 19｜ternary Other-family candidates for a superconducting-
  electronics material review。｜P14m（语境参考）。
### 21 五元 Other 族候选的 15~16K 窄窗筛选（label S21）
- 约束：family==Other ∧ tc∈[15.2,16.2) ∧ ec==5；目标 tc:max。
- 自答：五元 Other 族 15~16K 窄带成员对应多组元中温候选的工程
  筛选，现实成立。
- 题面：opener 20｜five-element Other-family candidates inside a narrow
  15-16 K screening window。｜P14m（语境参考）。
### 22 四元氧化物候选的 29K 档铋氧系选型（label S22）
- 约束：family==Oxide ∧ tc∈[28.5,29.9) ∧ ec==4；目标 tc:max。
- 自答：Ba-K-Bi-O3 型四元铋氧化物 Tc≈29K 是无铜氧化物中温超导的
  代表体系，20K 级制冷机应用现实成立。
- 题面：opener 21｜quaternary oxide candidates for a bismuthate-era
  intermediate-temperature selection。｜P16m。
### 23 三元铁基候选的 32~32.6K 窄带筛选（label S23）
- 约束：family==Ferrite ∧ tc∈[32.0,32.6) ∧ ec==3；目标 tc:max。
- 自答：三元铁基 32K 窄带（122 系下缘成员）对应中温磁体候选带的
  精细筛选，铁基消歧入 dk。
- 题面：opener 22｜ternary iron-based candidates inside a narrow
  intermediate-temperature band。｜P5。
### 24 四元铁基候选的 52~55K 档中温磁体选型（label S24）
- 约束：family==Ferrite ∧ tc∈[52.1,55.0) ∧ ec==4；目标 tc:max。
- 自答：四元铁基 52~55K 档是 40K 级制冷机应用的现实候选。
  【修复轮勘误 W3】gt 解码实为 NdFeAsO₀.₇（氧缺位 Nd 系 1111），非
  SmFeAsO 氟掺杂；自答去掉具体化学系断言。
- 题面：opener 23｜quaternary iron-based candidates for a 40 K-class
  cryocooler application。｜P5。
### 25 五元铁基候选的 50~56K 档电力应用预筛（label S25）
- 约束：family==Ferrite ∧ tc∈[50.5,56.5) ∧ ec==5；目标 tc:max。
- 自答：五元铁基 50~56K 档对应电力传输示范的中温预筛场景。
- 题面：opener 24｜five-element iron-based candidates pre-screened for a
  mid-temperature power application。｜P5。
### 26 三元 Other 族候选的 38~41K 二硼化镁邻带筛选（label S26）
- 约束：family==Other ∧ tc∈[38.5,41.4) ∧ ec==3；目标 tc:max。
- 自答：39K 档三元（MgB2 掺杂衍生一脉）是 30K 级制冷机磁体的现实
  选材邻带。
- 题面：opener 25｜ternary Other-family candidates in the magnesium-
  diboride neighborhood band。｜P12。
### 27 六元铜氧候选的 60~80K 液氮示范预选（label S27）
- 约束：family==Cuprate ∧ tc∈[60,80) ∧ ec==6；目标 tc:max。
- 自答：六元铜氧 60~80K 档（多层铜氧下缘）对应过冷氮 65K 冷却示范
  的预选——叙事温区 65K 落于窗内。
- 题面：opener 26｜six-element cuprate candidates shortlisted for a
  subcooled-nitrogen (65 K-class) demonstrator。｜P3。
### 28 七元铜氧候选的液氮温区磁体选型（label S28）
- 约束：family==Cuprate ∧ tc∈[76.0,99.3) ∧ ec==7；目标 tc:max。
- 自答：七元铜氧 76K 以上正对液氮 77K 温区磁体现实场景——77K
  落于窗内。
  【修复轮勘误 W3】gt 解码实为铋系铜氧（非 Hg/Tl 多层衍生系）；自答
  去掉具体化学系断言，只保留族级画像与温区对应。
  【修复轮引文 W2】文献由 P15m（Hg 系 130K 专指，弱对应）改挂 P3
  （铜氧综述，族级对应无化学系冲突）。
- 题面：opener 27｜seven-element cuprate candidates for a liquid-
  nitrogen-temperature magnet selection。｜P3。
### 29 三元铜氧候选的 42~45K 中温筛选（label S39，池3 偏离）
- 约束：family==Cuprate ∧ tc∈[42.0,45.0) ∧ ec==3；目标 tc:max。
- 自答：三元铜氧 42~45K（铜氧中温带，避开 45.0 双并列）是制冷机
  中温示范的真实样本带。
- 偏离：池 3（<5）。三要素：偏离类型2 池下限 5／实际取 3／该窗内
  三元铜氧库内天然稀疏，放宽 tc 窗即混入 45.0 并列（失去唯一性）或
  离开"中温铜氧精细筛选"科学问题，放宽 ec 即换问题本身。
- 题面：opener 28｜ternary cuprate candidates inside the intermediate
  42-45 K band。｜P1。
### 30 六/七元铁基候选的七元铁基档筛选（label S40，池3 偏离）
- 约束：family==Ferrite ∧ tc∈[10,24.1) ∧ ec∈[6,7]；目标 tc:max。
- 自答：六/七元铁基（多元插层/衍生铁基）10~24K 档是铁基家族高组元
  端真实存在的稀疏样本带，对应新型铁基机理研究。
- 偏离：池 3（<5）。三要素：偏离类型2 池下限 5／实际取 3／七元铁基
  天然稀疏，放宽 ec 区间即混入常规三元铁基（换掉"高组元铁基"问题
  本身），收窄 tc 窗则池更小。
- 题面：opener 29｜six-to-seven-element iron-based candidates for a
  high-complexity iron-based survey。｜P4。

## 3. 判废与回调记录（实时维护）
- preflight 终轮四数：候选 39 / 入选 28 / 判废 11（同格备选 7 +
  不唯一 1 + 池小备选 1 + S39 + S40）/ 无解 0；偏离道 S39/S40 按
  leader 追认 28+2 方案入选（池 3 留痕）。
- 末轮禁入格补位回调（2026-08-10，§0.3）：S01 换格 Carbon(0,3)、
  S02 换族 Oxide(0,1)、S07 换格 Heavy_fermion(0,1)；替换后四数不变，
  原 S02 Carbon 设计因同格/禁入连环冲突退出候选表（留痕本节）。
- 第一轮（整档口径）四数：候选 42 / 入选 4 / 判废 38 / 无解 0，根因
  旧 co 30 道 gt 全为各格 tc 最大值 → 换人窄窗策略（§0.2）。

## 4. 与活跃语料的互异核对（题面级）
- opener_idx 0~29 与批③ co_drug（38~67）零撞；TYPE2_OPENERS 批内
  opener_idx 30 种互异（开头两词 29 种：idx 10/11 同为 "out of"，
  池本身含一对两词重复，变体率 0.967）。
- domain_plural 全部新写（材料侧应用语境：毫开量子器件/制冷机直冷/
  液氮示范等），与 cs_material 30 道的 cs 型题面、旧 co_material 题面
  无复用。

## 5. 文献出处说明
沿用批② REFS + 本批新增 3 篇（P14m/P15m/P16m，DOI 均经 Crossref
核实；m 后缀避免与批③药学同号撞车）。族级对应：Carbon→P8、
Chevrel→P9、Cuprate→P1/P3、Ferrite→P4/P5、Heavy_fermion→P7、
Oxide→P10/P16m、Other（常规超导/MgB2 邻带）→P12/P13/P14m；P15m
仅登记于 REFS 无道引用（原挂 case 28，修复轮因弱对应改挂 P3）。
弱对应道一律降为语境参考措辞（§1 名单），不写作者名（防 N7 擦边）。
批②教训2 核对：每道文献与 gt 族标签/element_count/温区逐一比对
（掩码 formula 按族+组成数核对）。

## 6. 批②三教训防护对照
1. 场景与约束窗口物理相容：逐道自答核对叙事温区落于约束窗内
   （case 27 用 65K 过冷氮落于 [60,80)；case 28 用 77K 落于
   [76,99.3)；其余道只写应用类别不写具体温值）。
2. 文献与 gt 真实化学类别匹配：族标签+ec+温区三重核对，弱对应降
   语境参考（§1）。
3. 术语歧义消解：Ferrite 全部道 dk 带消歧短语（"iron-based
   superconductors, not the magnetic ceramic ferrites"），题面一律
   "iron-based (Ferrite-family)"。
4. 自答须对照 gt 真实化学解码核验（修复轮新增，W3 教训）：批④评审
   发现 6 道自答与 gt 掩码解码后的真实化学不符（02 O₂/07 纯 U/
   08 Hf₀.₆Zr₀.₄V₂/15 La₂CuO₄₊δ/24 NdFeAsO₀.₇/28 铋系铜氧），
   已逐条勘误（§2）；后续批次自答只写族级画像与温区对应，任何具体
   化学系断言须先解码 gt 核验。
