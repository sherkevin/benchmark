# task-06 造题经验 — open 材料第二批（UCI 族 11 道）

> 批次：Task #34（task-06），2026-08-08。产出 `case_dh_material_05~15` 共 11 道，投 11 过 11 判废 0 预检剔除 0。脚本 `src/case_build/build_open_material_cases_task06.py`，账本 `open_material_discard_ledger.json` task06 段。本文沉淀本批独有的五个经验点。

## 1. UCI 族源切换：从"等接口"到"换源即换全套口径"

task-04 只用 3DSC 主键 5 族（UCI 26 族被 E3 接口 B 硬前置锁住），本批 E3 §9 交付后一次性切到 UCI 达标族。切换不是"把族清单换掉"这么简单，至少要同步换四件套：

1. **族清单来源**：qualifying_families_3dsc → qualifying_families_uci（26 族，降级键全集复算通道 fail-closed：`DEGRADED_KEY_NOT_UNIQUE` 与 `PRIMARY_KEY_NOT_UNIQUE` 都拦截）；
2. **唯一性判定链**：UCI 族身份键是降级键形态，判定链**不得依赖降级键唯一性**（监工裁定：降级键只可排除）。本批 26 族成员全库同组成唯一，同组成判法自然退化为单实体判定，不触碰拦截；若某族的判定必须靠唯一性，换主键族重造、不绕过拦截；
3. **干扰项池**：保持 primary-only（Task #20 收窄裁定延续，池 5,771 条）——换源不放松干扰项口径；
4. **结构参数**：UCI 族无空间群维度，grid_key family 位用 stats base 组成串口径，与 3DSC 批的 family 命名空间天然不撞。

教训：**解锁一批新族源时，先列"旧源依赖清单"逐项对账**（族清单/唯一性链/干扰池/结构键/provenance），再开工；本批 12 条 hard_constraints 就是这张对账单的落盘形态。

## 2. PROVENANCE 换引文：防"来源失实"是硬纪律

task-04 批的 provenance 硬编码 3DSC DOI。本批族源是 UCI，若沿用旧引文即成"来源失实"（题面数据出自 unique_m.csv，引文却指向 3DSC 论文）。处置：provenance 逐道换为 UCI 引文——UCI Machine Learning Repository #464 unique_m.csv（Hamidieh 2018，Computational Materials Science 154:346-354，DOI 10.24432/C53P47），并注明"composition-only records，Tc 为文献报告值（SuperCon 谱系）"。

教训：**族源/数据源一切，provenance 必须同批切换且逐道核对**；引文不是装饰字段，独立验证方会联网抽查真实性，失实引文等同数据造假未遂。另注：UCI 为组成级去重数据（同组成取最高 Tc），这正是"全库同组成唯一"成立的数据学依据，应写进 isomer_handling.resolution 留痕。

## 3. 同组成普查预检机制化：从"事故修复"到"前置闸"

task-03/04 时代同组成歧义靠事后普查兜底；本批把它前置为选格期预检：候选 gt 先做全库同组成普查，"部分满足窗口"者前置剔除单列（不冒充判废）。本批实测剔除 0——不是机制没用，而是 UCI 26 族成员全库同组成唯一，普查一次即过。

教训：**预检的价值不在剔除多少，在于把"可能废"变成"确定不废"的证明**；判废 0 + 预检剔除 0 同时成立时，账本必须把"为什么是 0"的数据学原因（同组成唯一）写清楚，否则 0 看起来像放水。

## 4. element_count 口径断言加固（Kim-T1，方案②）

三维评审正确性（Kim）发现：脚本对 grid_key 第三位的 element_count 只算不落断言，口径漂移无闸可拦。采纳方案②（脚本侧断言加固，不动产物）：`build_open_material_cases_task06.py` 写盘前断言 element_count == E1 注册口径（互异 [N\*] label 数，`build_property_counts.element_count_of`），与 task-05 材料 fixed 批锚定的口径同源。Task #38 落地后复验：**全部产物 md5 零影响**（加固只拦未来重跑，且脚本 fail-fast 拒重跑）。

教训：**凡是"算出来直接用"的口径字段，都要配一条写盘断言**——口径分叉（token 计数 vs 注册值，task-05 实测 143 个晶体分叉）不会在造题时报错，只会在 judge/评测侧静默失真。

## 5. opener 池扩容算术：M9a 余量要提前算

本批新引入 3 种 opener（Characterize/Isolate/Unearth），open 全语料 33 道实测 11 种/33 = **0.333**，过 0.3 门槛但**余量仅 0.033**。算术很直白：下一次 dh 扩题若不加 opener，哪怕只扩 3~4 道，变体率就会逼近或跌破 0.3。

教训：**M9a 是算术题不是运气题**——扩题前先算 `现种数/(现道数+扩题数)`，低于门槛就在同一批里把 opener 池扩掉（opener 两两互异、领域中性是硬约束），不要等 lint 红了再补。已列交接（task-06-report.md §5-⑥）。

## 6. 其他备忘

- **δ 收窄判例沿用**：15 道族顶间距 0.19 < 0.25 → δ=0.05（只收窄不放宽，同 task-04 La 族判例）；窗口宽是设计参数、不在 n/d/K 硬约束内，收窄不算放宽、扩宽才是。
- **register_dh_gb.py 幂等语义变更**：由"存在即跳过"改"一致则跳过"（不一致报错）；**改存量条目须显式删键再重跑**，直接改值会被幂等闸吞掉。b_partition.cases 24→35，回读 33/33 逐字一致。
- **gt 查重全语料口径**（task-05 教训延续）：本批查重分母 = 144 个已用 gt（fixed 120 + open 19 + spare 3 + legacy 示例宽扫），一次全过、零撞车。
